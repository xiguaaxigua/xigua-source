---
title: 025-菜谱应用中的控制器-《用AngularJS开发下一代Web应用》-笔记
date: 2016-05-19 10:52:29
tags: [Angular, 读书笔记]
---
读书笔记。
<!--more-->

## 概要
本文讲述菜谱中用到的控制器。

## 正文

菜谱应用中我们用到了5个控制器。控制器位于app/scripts/controllers/controller.js中，我们一个一个来看。首先来看第一个控制器，也就是List控制器，它的职责是显示系统中的所有菜谱。

```
app.controller('ListCtrl', ['$scope', 'recipes',
    function($scope, recipes) {
  $scope.recipes = recipes;
}]);
```

请注意关于List控制器最重要的一件事情：在构造器中，它不会到服务端获取菜谱。相反，它会直接去处理一个已经获取到的菜谱列表。它的实现方式会在本章关于路由的小节里讲述，它和我们前面看到的MultiRecipeLoader服务有关，你只要记住这一点就可以了。

其他控制器从本质上说都与List控制器非常类似，但是我们仍会一个一个地来看，在这个过程中将会解释一些有趣的特性。

```
app.controller('ViewCtrl', ['$scope', '$location', 'recipe',
    function($scope, $location, recipe) {
  $scope.recipe = recipe;

  $scope.edit = function() {
    $location.path('/edit/' + recipe.id);
  };
}]);
```

View控制器中有一个有趣的特性，那就是它暴露在作用域中的edit函数。edit函数不会去做显示或者隐藏字段之类的工作，它把这些繁重的工作交给Angular去做。edit函数只是把URL改成编辑菜谱的地址，然后Angular就会去做剩余的工作。Angular发现URL改变了，就会去加载对应的视图（同一个菜谱将会变成编辑模式）。

下一步，我们再来看看Edit控制器

```
app.controller('EditCtrl', ['$scope', '$location', 'recipe',
    function($scope, $location, recipe) {
  $scope.recipe = recipe;

  $scope.save = function() {
    $scope.recipe.$save(function(recipe) {
      $location.path('/view/' + recipe.id);
    });
  };

  $scope.remove = function() {
    delete $scope.recipe;
    $location.path('/');
  };
}]);
```

这里的新东西是Edit控制器暴露到作用域中的save和remove方法。

作用域中的save函数将会保存当前的菜谱，保存完成之后，就会把用户重定向到同菜谱查看视图。在这些场景中，回调函数非常有用，在你做完某些事情之后可以用来执行某些动作。

这里有两种方法用来保存菜谱。一种就是以上代码所展示的通过执行$scope.recipe.$save()来保存。这样做是可以的，因为菜谱是一个资源对象，会首先通过RecipeLoader返回。

你还可以通过另一种方法来保存菜谱：

```
Recipe.save(recipe);
```

remove函数同样简单，它会从作用域中删除菜谱，然后把用户重定向到主页面。注意，这里并没有真正从我们的服务器上删掉菜谱，虽然添加这样一个额外的调用也不算什么难事。

接下来我们来看New控制器：

```
app.controller('NewCtrl', ['$scope', '$location', 'Recipe',
    function($scope, $location, Recipe) {
  $scope.recipe = new Recipe({
    ingredients: [ {} ]
  });

  $scope.save = function() {
    $scope.recipe.$save(function(recipe) {
      $location.path('/view/' + recipe.id);
    });
  };
}]);
```

New控制器几乎和Edit控制器一模一样。实际上，你可以试着把这两个控制器融合到一个控制器中。他们主要的不同点在于，New控制器在第一步会创建一个新的菜谱（它是一种资源，所以带有save函数），其余内容都和Edit控制器一样。

最后是Ingredients控制器。这是一个特殊的控制器，在弄明白为什么以及怎么做之前，我们先来看代码：

```
app.controller('IngredientsCtrl', ['$scope',
    function($scope) {
  $scope.addIngredient = function() {
    var ingredients = $scope.recipe.ingredients;
    ingredients[ingredients.length] = {};
  };
  $scope.removeIngredient = function(index) {
    $scope.recipe.ingredients.splice(index, 1);
  };
}]);
```

之前我们所看到的所有控制器都会连接到UI中的特定视图上，但是Ingredients控制器比较特殊，它是菜谱编辑页面上的一个子控制器，用来封装更高层级上不会使用的特定功能。有一件有趣的事情需要注意一下：既然Ingredients是一个子控制器，那么它就可以继承副控制器的作用域（在这个例子中就是Edit/New这两个控制器）。这样一来，Ingredients就可以访问父控制器中的$scope.recipe对象。

Ingredients控制器自身没有什么有趣或特殊的地方，它只是向菜谱的原料数组中添加一种新的原料，或者从原料数组中删除一种指定的原料。

完整代码：

```
'use strict';

var app = angular.module('guthub',
    ['guthub.directives', 'guthub.services']);

app.config(['$routeProvider', function($routeProvider) {
    $routeProvider.
      when('/', {
        controller: 'ListCtrl',
        resolve: {
          recipes: ["MultiRecipeLoader", function(MultiRecipeLoader) {
            return MultiRecipeLoader();
          }]
        },
        templateUrl:'/views/list.html'
      }).when('/edit/:recipeId', {
        controller: 'EditCtrl',
        resolve: {
          recipe: ["RecipeLoader", function(RecipeLoader) {
            return RecipeLoader();
          }]
        },
        templateUrl:'/views/recipeForm.html'
      }).when('/view/:recipeId', {
        controller: 'ViewCtrl',
        resolve: {
          recipe: ["RecipeLoader", function(RecipeLoader) {
            return RecipeLoader();
          }]
        },
        templateUrl:'/views/viewRecipe.html'
      }).when('/new', {
        controller: 'NewCtrl',
        templateUrl:'/views/recipeForm.html'
      }).otherwise({redirectTo:'/'});
}]);

app.controller('ListCtrl', ['$scope', 'recipes',
    function($scope, recipes) {
  $scope.recipes = recipes;
}]);

app.controller('ViewCtrl', ['$scope', '$location', 'recipe',
    function($scope, $location, recipe) {
  $scope.recipe = recipe;

  $scope.edit = function() {
    $location.path('/edit/' + recipe.id);
  };
}]);

app.controller('EditCtrl', ['$scope', '$location', 'recipe',
    function($scope, $location, recipe) {
  $scope.recipe = recipe;

  $scope.save = function() {
    $scope.recipe.$save(function(recipe) {
      $location.path('/view/' + recipe.id);
    });
  };

  $scope.remove = function() {
    delete $scope.recipe;
    $location.path('/');
  };
}]);

app.controller('NewCtrl', ['$scope', '$location', 'Recipe',
    function($scope, $location, Recipe) {
  $scope.recipe = new Recipe({
    ingredients: [ {} ]
  });

  $scope.save = function() {
    $scope.recipe.$save(function(recipe) {
      $location.path('/view/' + recipe.id);
    });
  };
}]);

app.controller('IngredientsCtrl', ['$scope',
    function($scope) {
  $scope.addIngredient = function() {
    var ingredients = $scope.recipe.ingredients;
    ingredients[ingredients.length] = {};
  };
  $scope.removeIngredient = function(index) {
    $scope.recipe.ingredients.splice(index, 1);
  };
}]);
```

以上代码使用了resolve函数，创建了一个名为Guthub的Angular模块，同时创建了应用中用到的所有路由和模板。

以上代码还会把我们所创建的指令和服务关联起来，并且负责描述应用中所涉及的大量路由。

对于每一个路由，我们都会指定的配置项有：URL、用来处理它的控制器、需要加载的模板，记忆一个可选项resolve对象。

resolve对象用来告诉Angular：必须保证每一个resolve键都是符合条件的才能把路由显示给用户。对我们来说，我们需要加载所有菜谱或者带条菜谱，保证在收到服务器的响应之后再显示页面。因此告诉路由提供者我们需要一些菜谱，然后告诉它如何去获取这些菜谱。

路由会连接到第一节定义的两个服务MultiRecipeLoader和RecipeLoader上。如果resolve函数返回了一个Angular promise，那么Angular会在继续下一个动作之前等待promise填充完成。也就是说，它会一直等到获得服务器的响应为止。

然后结果会被作为参数传递给构造函数（参数名将会成为对象的字段名）。

最后，otherwise函数提供了一个默认的URL，当没有匹配到任何路由时，就会跳转到这个URL。

<i>你可能已经注意到了，Edit和New这两个控制器的路由都指向了相同的模板URL——views/recipeForm.html，是怎么回事呢？因为我们在重用编辑模板。根据关联的控制器不同，在菜谱编辑模板中会显示不同的元素。</i>
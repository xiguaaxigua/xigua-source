---
title: “有BOM的UTF-8”和“无BOM的UTF-8”
date: 2016-06-29 20:51:55
tags: [教程]
---
此BOM非彼BOM。
<!--more-->

BOM，Byte Order Mark，即“字节顺序标记”。偶然在知乎上看到（[点击访问](https://www.zhihu.com/question/20167122)），研究了一下，记录如下：

### 维基百科的解释
摘录如下：（[点击访问](https://en.wikipedia.org/wiki/Byte_order_mark)）

The byte order mark (BOM) is a Unicode character, U+FEFF BYTE ORDER MARK (BOM), whose appearance as a magic number at the start of a text stream can signal several things to a program consuming the text.

BOM use is optional, and, if used, should appear at the start of the text stream.

Unicode can be encoded as 8-bit, 16-bit, or 32-bit integers. For the 16- and 32-bit representations, a computer receiving text from arbitrary sources needs to know which byte order the integers are encoded in. Because the BOM itself is encoded in the same scheme as the rest of the document, but has a known value, the consumer of the text can examine these first few bytes to determine the encoding. The BOM thus gives the producer of the text a way to describe the text stream's endianness to the consumer of the text without requiring some contract or metadata outside of the text stream itself.

Once the receiving computer has consumed the text stream, it is free to process the characters in its own native byte order and no longer needs the BOM. Hence the need for a BOM arises in the context of text interchange, rather than in text processing within a closed environment.

翻译如下：
BOM是一种Unicode字符，字节顺序标记，形如[Magic Number](https://en.wikipedia.org/wiki/Magic_number_(programming)#Magic_numbers_in_files) （即幻数），出现在文本文件的头部，用于标识文件是采用哪种格式的编码。

BOM的使用是可选的，但是在使用的时候必须出现在文本文件的头部。

Unicode可以被编码为8位，16位，或32位的整数。对于16位和32位而言，计算机从任意源接受文本时，需要知道使用哪种字节顺序来编码。BOM本身和其它文件一样，以同样的方式编码，但是你可以检查文件的前几个字节来确定该文件的编码方式。从而BOM让我们在未查看文件的元数据时就已经知晓该文件的编码方式。

一旦计算机接收了文本文件，那么计算机将以自己原生的字节序编码该文件，不再需要BOM。因此，BOM出现在文本交换的情况下，而不是在文本处理的封闭空间内。

TIPS：
+ Unicode，又叫统一码、万国码、单一码，是计算机科学领域里的一项业界标准,包括字符集、编码方案等。
+ 幻数，它可以用来标记文件或者协议的格式，很多文件都有幻数标志来表明该文件的格式。

### 简而言之

BOM就像一个人的帽子，看见警察的帽子就知道这个人的职业是警察；看见帽子上有矿灯，就知道这个人的职业是矿工。

UTF-8因为它的编码特性，是字节序无关的，所以UTF-8不需要BOM，尽管Unicode标准允许在UTF-8中使用BOM，所以不含 BOM 的UTF-8才是标准形式。

在UTF-8文件中放置BOM主要是微软的习惯，BOM是为UTF-16和UTF-32准备的，用于标记字节序（byte order）。微软在UTF-8中使用BOM是因为这样可以把UTF-8和ASCII等编码明确区分开，但这样的文件在Windows之外的操作系统里会带来问题。

「UTF-8」和「带 BOM 的UTF-8」的区别就是有没有BOM。即文件开头有没有U+FEFF。

### BOM 和 PHP

PHP并不会忽略BOM，所以在读取、包含或者引用这些文件时，会把BOM作为该文件开头正文的一部分。根据嵌入式语言的特点，这串字符将被直接执行（显示）出来。由此造成即使页面的padding-top设置为0，也无法让整个网页紧贴浏览器顶部，因为在html一开头有这3个字符呢！

windows总是自做聪明的做一些别人无法理解的事情，UTF-8并不需要BOM头啊！

显然，对于网页编程来说，BOM还是去掉的比较好。




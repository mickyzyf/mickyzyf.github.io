---
layout: post
index_img: https://gitee.com/youlan_lan/md_image/raw/master/20210607012255.png
title:  "CSS基础"
date:   2020-5-18 15:14:54
categories: CSS3
tags:	CSS3
excerpt:	一直想系统性整理一下CSS，关于CSS的层叠和继承，就是开发者CSS的三种引入方式，还有一些CSS的选择去需要拿出来重新整理一下
mathjax: true
author:	朱羽飞
---


# CSS基础

## 打好CSS布局的基础

基于CSS进行设计的最主要的优势就是，只要HTML文档结构良好，那么很容易通过添加一层CSS作为修饰，让文档样式变得非常漂亮。把HTML结构看做书一样持久不变，就像把html刻在石头上一样。这样，提交代码后，尽量不要修改html结构，因为修改了html结构会使得css样式错乱。

## css层叠与继承

### 层叠

一个元素的样式，可以通过多种方式来定义，而多种定义方式之间通过复杂的影响关系决定了元素的最终样式。这种复杂既造就了CSS的强大，也导致CSS显得如此“混乱”而难以调试。

对于层叠来说，共有三种主要的样式来源：

* 浏览器对HTML定义的默认样式
* 用户定义的样式
* 开发者定义的样式，可以有三种形式：

  * 定义在外部文件(外链样式)：通过<link>引入的样式
  * 在页面的头部定义（内部样式，也叫内联样式）：通过&lt;style&gt;定义的样式，只在本页面内生效
  * 定义在特定元素上（内嵌样式，也叫行内样式）：这种形式多用于测试，可维护性差

注：（css层叠样式的三种优先级比较）

外链样式 < 内联样式 < 内嵌样式

记忆方法：就近原则(谁靠近元素最近，谁的优先级最高)

特殊情况：
就是如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。
如：

 <head>
     <style type="text/css">
       /*内部样式*/
       h3{color:green;}
     </style>
  
     <!-- 外部样式 style.css -->
     <link rel="stylesheet" type="text/css" href="style.css"/>
     <!-- 设置：h3{color:blue;} -->
 </head>
 <body>
     <h3>测试！</h3>
 </body>

### 优先级附加（IE 中奇怪的应用--CSS的BUG）

IE浏览器下载或者渲染的顺序可能如下

* IE下载的顺序是从上到下的
* JavaScript函数的执行会阻塞IE的下载
* IE渲染的顺序是同时进行的
* 在渲染到页面的某一部分时，其上面的所有部分都已经瞎子完成（但并不是说所有相关联的元素都已经下载完了）
* 解析过程中，停止页面所有往下元素的下载，样式文件比较特殊，在其下载完成后，将和以前下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（含以前渲染的）重新进行样式渲染。并以此方式一直渲染下去，直到渲染页面完成
* Firefox处理下载和渲染的顺序大体相同，除了iframe的渲染。

### 继承

当页面一个元素被另一个元素包裹时，我们称被包裹的元素叫子元素，包裹元素叫父元素。如：

 <p style="color:red">
  这是一个<strong>测试</strong>的p标签
 </p>

当给p添加样式color为red时，此时a的颜色样式被stong继承了，当然也不是所有的css属性都会被子类继承，例如border属性。继续利用上面的一段代码。我们为p元素添加border属性

 <p style="border:1px solid red">
  这是一个<strong>测试</strong>的p标签
 </p>

执行之后会发现，只有一个框而不是测试两个字也有边框,同样css的继承也不是所有元素都能主动继承的，有的并不能直接继承父元素的css属性。当子元素不能能直接继承父元素的属性的时候，我们可以在样式中使用inherit来声明子元素的该属性继承父的子元素。

 <p style="border:1px solid red">
  这是一个<strong style="border:inherit">测试</strong>的p标签
 </p>

这时，我们会发现这个strong元素的border属性跟父元素p一样也有红色边框

## CSS的选择器

### 基础选择器

1. 标签选择器

 *{}     指定通用元素选择器，匹配任何元素

  *{margin:0 auto;padding:0 auto;}

 element  标签选择器，匹配标签的元素

2. 类选择器

 .class   指定元素中包含该class类的元素

3. id选择器

  #id  指定元素的id值是id的元素

选择器的优先级

内嵌样式(行内样式) > id选择器 > Class选择器 > 标签选择器

权重:1000 > 100 > 10 > 1

解释：首先同css优先级一样内嵌（行内）是最高级别的，id选择器次之，因为html中id的定义是有且仅有一个同名id，类选择第三因为，多个元素可以拥有相同的class类名，而标签选择器排在最后是因为，元素标签是html的底层的东西，即不能单独指定某一个元素，也不能指定某一类元素。仅仅表示元素总的标签类。

### 关系选择器（组合选择器）

1. E,F

 多元素选择器，同时匹配所有E元素和F元素，中间用，分隔
2. E F

 后代选择器，匹配E元素后代元素中的F元素
3. E>F

 子元素选择器，匹配E元素子元素中的F元素
4.  E+F

 组合选择器，元素B的任一下一个兄弟元素E

5. E:first-child，E:nth-child(n)

 节点选择器，E元素的直系第一个后代元素

6. B~E

 兄弟选择器，B元素后面的拥有共父元素的E元素

### 属性选择器

1. E[att]

 匹配所有att属性的E元素，不考虑它的值

2. E[att = val]

 匹配所有att属性值等于"val"的E的元素

3. E[att~=val]

 匹配所有att属性具有多个空格分隔的值、其中一个值等于“val”的E元素

4. E[att = val]

 匹配所有att属性具有多个连字号分隔（hyphen-separated）的值、其中一个值以“val”开头的E元素，主要用于lang属性，比如“en”、“en-us”、“en-gb”等等

### 伪类选择器

什么叫伪类选择器，伪就是假装的意思，和一般的DOM中的元素不一样，它并不改变任何DOM内容。只是插入了一些装饰类的元素，这些元素对于用户来说是可见的，但对于DOM来说不可见。伪类的效果可以通过添加一个实际的类来达到。

伪类的分类有两种：

一、动态伪类;

动态伪类，因为这些伪类并不存在于HTML中,而只有当用户和网站交互的时候才能体现出来

 a:link {color:gray;}/*链接没有被访问时，颜色为灰色*/
 a:visited{color:yellow;}/*链接被访问过后，颜色为黄色*/
 a:hover{color:green;}/*鼠标悬浮在链接上时，颜色为绿色*/
 a:active{color:blue;}/*鼠标点中激活链接那一下，颜色为蓝色*/

对于这四个伪类的设置，有一点需要特别注意，Link--visited--hover--active顺序如果错了，可能会带来意想不到的效果。

二、状态伪类

* ":enabled"  可点状态
* ":disabled" 不可点状态
* ":checked" 勾中状态

三、nth伪类选择

* :first-child   选择某个元素的第一个子元素；
* :last-child   选择某个元素的最后一个子元素；
* :nth-child()   选择某个元素的一个或多个特定的子元素；
* :nth-last-child() 选择某个元素的一个或多个特定的子元素，从这个元素的最后一个子元素开始算；
* :nth-of-type()   选择指定的元素；
* :nth-last-of-type() 选择指定的元素，从元素的最后一个开始计算；
* :first-of-type   选择一个上级元素下的第一个同类子元素；
* :last-of-type   选择一个上级元素的最后一个同类子元素；
* :only-child   选择的元素是它的父元素的唯一一个了元素；
* :only-of-type   选择一个元素是它的上级元素的唯一一个相同类型的子元素；
* :empty    选择的元素里面没有任何内容。（IE 6-8不支持）

* ：not 否定选择器，常用form表单中除去submit的input样式

  input:not([type="submit"]){...}

四、伪元素

对元素进行一些调整但又不能写在元素里面的

* :first-line 选择元素的第一行，用于控制第一行文本

* :first-letter 选择元素的第一个字母，用于控制首字母下沉或上浮

* :before 在主要元素之前插入内容，这个content配合使用，见过做多的是清除浮动

* :after 在主要元素之后插入内容，这个content配合使用，见过做多的是清除浮动

之前写过一个用befor和after给页面添加小点缀的东西

 <div class="section-header">
  <h2>页面小点缀</h2>
 </div>
 <style>
 .section-header h2 {
     margin-top: 30px;
     margin-bottom: 30px;
     font-size: 45px;
     font-weight: 600;
     color: #313131;
     font-family: "Lora", serif;
     text-align: center;
     text-transform: uppercase;
     position: relative;
     padding-bottom: 30px;
 }

 /*画两头的线*/
 .section-header h2:before {
     border-left: 50px solid #DADADA;
     border-right: 50px solid #DADADA;
     bottom: 5px;
     content: "";
     height: 1px;
     left: 50%;
     margin-left: -70px;
     position: absolute;
     width: 40px;
 }
 /*画菱形*/
 .section-header h2:after {
     background: #FF8724 none repeat scroll 0 0 padding-box content-box;
     border-bottom: 1px solid #DADADA;
     border-left: 1px solid #DADADA;
     bottom: -9px;
     content: "";
     height: 22px;
     left: 50%;
     margin-left: -16px;
     padding: 0 0 8px 8px;
     position: absolute;
     -webkit-transform: rotate(-45deg);
     -ms-transform: rotate(-45deg);
     transform: rotate(-45deg);
     width: 22px;
 }
 </style>

效果图：

![]({{ "/img/postImg/css-weilei-query.png" | prepend: site.baseurl }})

### 总结

1. 选择器都有一个权值，权值越大越优先；

2. 当权值相等时，后出现的样式表设置要优于先出现的样式表设置；

3. 创作者的规则高于浏览者：即网页编写者设置的CSS 样式的优先权高于浏览器所设置的样式；

4. 继承的CSS 样式不如后来指定的CSS 样式；

5. 在同一组属性设置中标有“!important”规则的优先级最大；

6. 要灵活运用好CSS的选择器的运用，很少的代码实现完美的页面展示，要勤加练习，这样才能完全掌握CSS的样式

---
layout: post
title:  "《HTML5移动Web开发实战》"
date:   2020-11-12 19:06:05
categories: HTML5 
tags: HTML5 
excerpt: HTML5与移动网站相关知识学习
mathjax: true
author:	朱羽飞
---

# 《HTML5移动Web开发实战》

第一章：HTML5与移动网站，介绍HTML5和移动网站的基本概念，包括一些模拟器和仿真器

第二章：移动端的配置和优化，讨论针对多种移动设备的配置和优化，例如禁用文字缩放和优化可视宽度

第三章：移动设备的交互方式，讨论了手势事件等移动交互

第四章：构建快速响应式移动互联网站点，介绍了各种构建快速响应式网站的方法

第五章：移动设备访问，讨论了基于地理位置的移动互联网站和其他HTML5针对设备的功能

第六章：移动富媒体，介绍了可用于移动浏览器的HTML5富媒体元素

第七章：移动设备调试，讲解了如何在移动设备屏幕尺寸的限制下有效地调试移动网站和应用

第八章：服务端性能调优，关注移动网站的服务器性能调优

第九章：移动性能调试，讲解了各种可用于移动性能调优的工具和技术

第十章：拥抱移动互联网特性，讲解了ECMAScript5等一些针对移动设备的功能及性能优化。

# HTML5与移动网站

为什么HTML5没有版本号？

1. 浏览器并不针对HTML的某个版本做支持，而是针对某个功能做支持。就是说如果浏览器支持你使用的某个功能，即使你把文档申明为HTML4，浏览器仍然会按照HTML5的标准来显示页面。
2. 名字可以很简洁

# HTML5与移动网站——配置和优化

## 通过界面图标启动Web应用

 <meta name="viewport" content="width=device-width,minimum-scale=1.0,maximum-scale=1.0,initial-scale=1.0,user-scalable=no" />

    <link rel="apple-touch-icon-precomposed" sizes="114x114" href="icons/apple-touch-icon-114x114-precomposed.png">

    <link rel="apple-touch-icon-precomposed" sizes="72x72" href="icons/apple-touch-icon-72x72-precomposed.png">
    <link rel="apple-touch-icon-precomposed" sizes="57x57" href="icons/apple-touch-icon-57x57-precomposed.png">
    <link rel="apple-touch-icon-precomposed" href="icons/apple-touch-icon-precomposed.png">

 <link rel="shortcut icon" href="icons/apple-touch-icon.png">

## 避免文本字体大小重置

 html{
  -webkit-text-size-adjust:100%;
  -ms-text-size-adjust:100%;
  text-size-adjust:100%;
 }

### px,em 哪个更好？

在Web开发领域，关于应该使用px(像素)还是em(相对长度单位，相对于当前对象内文本的字体尺寸)的争论不绝于耳，但是，这个问题在移动互联网开发领域，争论并没有那么激烈，Yahoo！的用户接口原本使用的单位是em，他们这么做的原因是IE6不支持px级别的缩放。但是，这在移动互联网开发领域，并不是问题，因为，使用IE6的用户已经越来越少了。因此，在大部分场景下，你都可以使用像素来设置字体大小，抛开使用em遇到的各种问题和烦人的计算。

## 优化浏览器视口宽度设置

 <meta content="width=device-width name="viewport">

## 修复移动版Safari的re-flow scale问题

   <meta content="width=device-width, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no" name="viewport">
控制手势缩放

## iphone下全屏模式启动

适用设备： iOS设备

为了让一个Web应用看起来更像一个原生应用，iPhone为Web应用开发这提供了很多独有的特性，你可以全屏模式下启动Web应用，可以添加一个启动界面，甚至添加一个加载进度条之类的，或者为你的Web应用定义一个预加载的页面。

 <meta name="apple-mobile-web-app-capable" content="yes" />

这段代码的意思是当Web应用从界面图标启动时，以全屏模式启动，隐藏浏览器上部的工具栏、地址栏和底部的加载状态栏

 <meta name="apple-mobile-web-app-status-bar-style" content="black" />
这段代码的作用是在浏览器顶部显示一个状态栏

 <link rel="apple-touch-startup-image" href = "img/1/splash.png">
这段代码的作用是，在程序启动、加载的时候，显示一个预加载界面，告诉用户该程序正在加载

由于iPad和iPhone因为屏幕大小的差异，因此需要不同大小的预加载界面，因此，如果你希望你的Web可以动态地选择对应的加载界面，你可以使用如下的JavaScript函数做到：

 var filename = navigator.platform === 'ipad'?'h/':'l/';

 document.write('<link rel="apple-touch-start-image" href="/img/'+filname +"splash.png" />');

## 防止iOS在聚焦时自动缩进

略

## 禁用或限制部分WebKit特性

为狭窄的屏幕添加省略号功能

 .ellipsis{
  text-overflow:ellipsis;
  overflow:hidden;
  white-space:nowrap;
 }

# 移动设备的交互方式

移动设备与桌面设备最大的不同在于交互方式，在桌面设备上，我们利用鼠标移动和点击来交互，而在移动设备上，交互来自触控和手势。

## 利用触控来移动页面元素

利用jQuery来注册touchmove事件。利用touch.pageX和touch.pageY可以获取相对于页面的触摸位置，我们将触摸位置减去div高度和宽度的一半，这样，div的拖动就在它的中心点上。

 var x = touch.pageX - elm.left/2;
 var y = touch.pageY - elm.top/2;

将x和y的值赋给div的CSS位置属性，它就可以移动了。

 $(this).css('left',x+'px');
 $(this).css('top',y+'px');

特殊的，移动版Safari不允许event对象touches和changedTouches属性被拷贝给其他对象。我们可以使用e.originalEvent来解决这个问题。

 var touch = e.originalEvent.touches[0] || e.originalEvent.changedTouches[0];

### jQuery移动版事件

jQuery移动版是一系列的组件

### Zepto

如果你的主要浏览器是基于WebKit，那么你可以考虑Zepto，这个轻量级的jQuery替代品。

## 监测和处理横竖屏切换事件

对于移动浏览器，如果网站是基于流体布局的，那么在横竖屏切换的时候，网站样式也应该有响应的变化。但是对于富交互的网站，有时也需要以特别的方式来处理横竖屏切换。

通过监听widow.onorientationchange事件，当横竖屏切换事件触发时，orientationchange会被触发，同时我们得到event.orientation作为参数传递给该方法并输出结果。

有时，你需要禁用横竖屏的自动切换，比如开发游戏，对于原生应用这很容易，但对于网页应用非常困难。

 <!doctype html>
 <html>
   <head>
   <title>锁定页面为横屏模式</title>
   <meta charset="utf-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <style>
   html, body {
    padding: none;
    margin: none;
   }
   </style>
     <link rel="stylesheet" href="http://code.jquery.com/mobile/1.0/jquery.mobile-1.0.min.css" />
     <script src="http://code.jquery.com/jquery-1.6.4.min.js"></script>
     <script src="http://code.jquery.com/mobile/1.0/jquery.mobile-1.0.min.js"></script>
   </head>
   <body>
     <div id="a">
     </div>
   <script>
    var metas = document.getElementsByTagName('meta');
    var i;
    if (navigator.userAgent.match(/iPhone/i)) {
     for (i=0; i<metas.length; i++) {
      if (metas[i].name == "viewport") {
       metas[i].content = "width=device-width, minimum-scale=1.0, maximum-scale=1.0";
      }
     }
     document.addEventListener("gesturestart", gestureStart, false);
    }
    function gestureStart() {
     for (i=0; i<metas.length; i++) {
      if (metas[i].name == "viewport") {
       metas[i].content = "width=device-width, minimum-scale=0.25, maximum-scale=1.6";
      }
     }
    }
   </script>

   <script>
       $(window).bind('orientationchange',function(event){
         updateOrientation(event.orientation);
       })
    function updateOrientation(orientation) {
         $("#a").html("<p>"+orientation.toUpperCase()+"</p>");
    }
   </script>
  </body>
 </html>

使用window.orientation可以检测当前的屏幕模式，它有四个值:-90、0、90、180。横屏时的值为-90或90，竖屏时的值是0或180。

 switch(window.orientation){
  case 0 ://
  case 180 ://
  case -90 ://
  case 90 ://
 }

## 利用手势来旋转页面元素

当用户在移动版Safari中，使用两指触控来旋转，我们可以检测到旋转的角度，因此，我们可以使用两指触控来旋转页面元素。

 <!doctype html>
 <html>
   <head>
   <title>Mobile Cookbook</title>
   <meta charset="utf-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
   <style>
     #main {
       text-align:center;
     }
   #someElm {
       margin-top:50px;
       margin-left:50px;
    width: 200px;
    height: 200px;
    background:#ccc;
    position:absolute;
   }
   </style>
   </head>
   <body>
     <header>
     </header>
   <div id="main">
    <div id="someElm">
    </div>
   </div>
     <footer>
     </footer>

   <script>
   var rotation =0 ;
     var node = document.getElementById('someElm');

     node.ongesturechange = function(e){
       var node = e.target;
       //alert(e.rotation);
       node.style.webkitTransform = "rotate(" + ((rotation + e.rotation) % 360) + "deg)";
     }

     node.ongestureend = function(e){
       rotation = (rotation + e.rotation) % 360;
     }
   </script>
  </body>
 </html>

只针对Safari有效

## 利用滑动来创建图库

移动设备上常用的功能之一就是滑动。在照片库中浏览照片时，我们会左右滑动来切换照片。在Android设备中，我们会向下滑动来解锁。

 <!doctype html>
 <html>
   <head>
   <title>利用滑动创建图库</title>
   <meta charset="utf-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0">
   <style>
    html, body {
     padding:0;
     margin:10px auto;
    }
    #checkbox {
     border:5px solid #ccc;
     width:30px;
     height:30px;
    }
    #wrapper {
     width:210px;
     height:100px;
     position:relative;
     overflow:hidden;
     margin:0 auto;
    }
    #inner {
     position:absolute;
     width:630px;
    }
    #inner div {
     width:200px;
     height:100px;
     margin:0 5px;
     background:#ccc;
     float:left;
    }
    .full-circle {
      background-color: #ccc;
      height: 10px;
      -moz-border-radius:5px;
      -webkit-border-radius: 5px;
      width: 10px;
      float:left;
      margin:5px;
    }
    .cur {
     background-color: #555;
    }
    #btns {
     width:60px;
     margin:0 auto;
    }
   </style>
   </head>
   <body>
     <header>
     </header>
   <div id="main">
    <div id="wrapper">
     <div id="inner">
      <div></div>
      <div></div>
      <div></div>
     </div>
    </div>
    <div id="btns">
     <div class="full-circle cur"></div>
     <div class="full-circle"></div>
     <div class="full-circle"></div>
    </div>
   </div>
     <footer>
     </footer>
   <script src="http://code.jquery.com/jquery-1.5.2.min.js"></script>
   <script src="http://code.jquery.com/mobile/1.0a4.1/jquery.mobile-1.0a4.1.min.js"></script>
   <script>
   var curNum = 0;
   $('#wrapper').swipeleft(function () {
       //alert('hi');
    $('#inner').animate({
    left: '-=210'
    }, 500, function() {
     // Animation complete.
     curNum +=1;
     $('.full-circle').removeClass('cur');
     $('.full-circle').eq(curNum).addClass('cur');
    });
   });

   $('#wrapper').swiperight(function () {
    $('#inner').animate({
    left: '+=210'
    }, 500, function() {
     // Animation complete.
     curNum -=1;
     $('.full-circle').removeClass('cur');
     $('.full-circle').eq(curNum).addClass('cur');
    });
   });
   </script>
  </body>
 </html>

在移动浏览器打开，并在灰色区域左右滑动，你可以看到它随着手指滑动在左右移动。

### Zepto框架与滑动事件

我们可以使用Zepto框架完成类似的功能，他提供了swipe、swipeLeft、swipeRight、swipeDown。

### YUI与手势事件

YUI提供了手势事件来创建滑动效果

## 利用手势进行图片缩放

 <!doctype html>
 <html>
   <head>
   <title>手势放大缩小</title>
   <meta charset="utf-8">
   <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
   <style>
    #frame {
     width:100px;
     height:100px;
     background:#ccc;
    }
   </style>
   </head>
   <body>
     <header>
     </header>
   <div id="main">
    <div id="frame"></div>
   </div>
     <footer>
     </footer>
   <script src="http://code.jquery.com/jquery-1.5.2.min.js"></script>
   <script src="http://code.jquery.com/mobile/1.0a4.1/jquery.mobile-1.0a4.1.min.js"></script>
   <script>
   var width = 100, height = 100;
   var node = document.getElementById('frame');
   node.ongesturechange = function(e){
    var node = e.target;
    // scale and rotation are relative values,
    // so we wait to change our variables until the gesture ends
    node.style.width = (width *e.scale) + "px";
node.style.height = (height* e.scale) + "px";
   }
   node.ongestureend = function(e){
    // Update the values for the next time a gesture happens
    width *= e.scale;
height*= e.scale;
   }
   </script>
  </body>
 </html>

## 加速按钮反馈

在移动设备浏览器上，Web应用中的按钮的反馈总会比原生应用的按钮反馈慢一点，其实，在移动设备的浏览器上有一个叫做"touchstart"的事件，通过检测该事件来替代按钮点击事件，会让应用更快地得到按钮的反馈。

## 优化polyfills脚本的加载速度

对于任何浏览器来说，加载脚本都是一项很重要且消耗性能的工作，对于移动设备来说更是如此，因为其带宽非常有限。Modernizr为这个问题提供了一个动态加载的解决方案。

## 检测用户客户端

在开发移动互联网站点时，有客户端检测功能将会非常有用，它可以帮我们重定向请求到客户端对应的版本，或者决定是否加载某些脚本或图片。

## 使用书签气泡为应用程序添加桌面快捷方式

在不同设备上添加桌面快捷方式，可以让Web应用看起来更像原生应用。

## 构建可自动伸缩的文本输入框

## 隐藏浏览器的地址栏

## 构建移动互联网的站点地图

# 移动设备访问

## 获取位置信息

从地理信息接口开始，网页应用可以拥有丰富的设备感知的功能和体验。一些基于设备访问的绝佳的创意已经开发出来了，从基于麦克风的音频输入访问和基于摄像头的视频输入访问，到本地数据(如联系人和事件)，甚至时重力感应都包括在内。

基于位置的社交网络，将会从根本上改变用户行为习惯和零售业的运营模式。

## 跨浏览器定位

地理信息接口不一定在所有移动浏览器上都可用，甚至对于支持它的浏览器也一样，浏览器可能有与标准不同的接口。

# 第六章，移动富媒体

多媒体——越来越多的永华直接在线接受音频和观看视频了，我们将为大家介绍如何把这些多媒体元素添加到移动互联网应用中。

离线和存储——对于移动设备来说，网络链接是非常不稳定的，因此离线使用对移动应用来说是非常重要，

性能和集成——通过使用Web Workers，我们可以在移动浏览器上得到更好的性能。

## 移动设备上播放音频

[案例源码](/Demo/H5Web/index.html)

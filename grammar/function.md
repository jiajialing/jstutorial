---
title: 函数
layout: page
category: grammar
date: 2012-12-15
modifiedOn: 2013-01-10
---

## 函数作用域

作用域（scope）指的是变量存在的范围。Javascript只有两种作用域：一种是全局作用域，变量在整个程序中一直存在；另一种是函数作用域，变量只在函数内部存在。

首先，在函数外部声明的变量就是全局变量，它可以在函数内部读取。

{% highlight javascript %}

   var v = 1;

   function f(){
	   console.log(v);
   }

   f()
   // 1

{% endhighlight %}

上面的代码表明，函数f内部可以读取全局变量v。

其次，在函数内部定义的变量，外部无法读取。

{% highlight javascript %}

   function f(){

	   var v = 1;	   
   }

   console.log(v);
   // 显示错误，v未定义

{% endhighlight %}

函数内部定义的变量，会覆盖同名全局变量。

{% highlight javascript %}

   var v = 1; 

   function f(){

	   var v = 2;

	   console.log(v);

   }

   f();
   // 2

   console.log(v);
   // 1

{% endhighlight %}

## 变量提升（hoisting）

Javascript解释器的工作方式是，先读取全部代码，再进行解释。这样的结果就是，函数体内部的var命令声明的变量，不管在什么位置，都会被提升到函数开始处声明，这就叫做“变量提升”。

{% highlight javascript %}

   function foo(x) {
        if (x > 100) {
            var tmp = x - 100;
        }
    }

{% endhighlight %}

上面的代码等同于

{% highlight javascript %}

   function foo(x) {
	   var tmp;
       if (x > 100) {
           tmp = x - 100;
       }
    }

{% endhighlight %}

## 闭包

闭包（closure）就是定义在函数体内部的函数。

{% highlight javascript %}

   function f() {
	   
       var c = function (){}; 

    }

{% endhighlight %}

上面的代码中，c是定义在函数f内部的函数，就是闭包。

闭包的特点在于，c可以读取函数f的内部变量。

{% highlight javascript %}

   function f() {

	   var v = 1;
	   
       var c = function (){
		   return v;
	   }; 

	   return c;

    }

    var o = f();

	o();
	// 1

{% endhighlight %}

原来，在函数f外部，我们是没有办法读取内部变量v的。但是，借助闭包c，可以读到这个变量。

## 立即调用的函数表达式（IIFE）

在Javascript中，一对圆括号“()”是一种运算符，跟在函数名之后，表示调用该函数。比如，print()就表示调用print函数。

有时，我们需要在定义函数之后，立即调用该函数。这时，你不能在函数的定义之后加上圆括号，这会产生语法错误。

{% highlight javascript %}

function(){ /* code */ }();
// SyntaxError: Unexpected token (

{% endhighlight %}

产生这个错误的原因是，Javascript解释器看到function关键字之后，认为后面跟的是函数定义，不应该以圆括号结尾。

解决方法就是让解释器知道，圆括号前面的部分不是函数定义，而是一个表达式，可以对此进行运算。你可以这样写：

{% highlight javascript %}

(function(){ /* code */ }()); 

{% endhighlight %}

也可以这样写：

{% highlight javascript %}

(function(){ /* code */ })(); 

{% endhighlight %}

这两种写法都是以圆括号开头，解释器就会认为后面跟的是一个表示式，而不是函数定义，所以就避免了错误。这就叫做“立即调用的函数表达式”（Immediately-Invoked Function Expression），简称IIFE。

推而广之，任何让解释器以表达式来处理函数定义的方法，都能产生同样的效果，比如下面三种写法。

{% highlight javascript %}

var i = function(){ return 10; }();

true && function(){ /* code */ }();

0, function(){ /* code */ }();

{% endhighlight %}

甚至像这样写

{% highlight javascript %}

!function(){ /* code */ }();

~function(){ /* code */ }();

-function(){ /* code */ }();

+function(){ /* code */ }();

{% endhighlight %}

new关键字也能达到这个效果。

{% highlight javascript %}

new function(){ /* code */ }

new function(){ /* code */ }() // 只有传递参数时，才需要最后那个圆括号。

{% endhighlight %}

通常情况下，只对匿名函数使用这种“立即执行的函数表达式”。它的好处在于，因为调用的时候不必指定函数名，所以避免了污染全局变量。

## 参考链接

- [Immediately-Invoked Function Expression (IIFE)](http://benalman.com/news/2010/11/immediately-invoked-function-expression/)

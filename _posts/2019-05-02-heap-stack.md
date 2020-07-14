---
layout: article
title: JavaScript 堆栈
keywords: js 堆栈,堆栈,js中的堆栈,javascript堆栈
description: js中的堆栈，堆是堆内存的简称，栈是栈内存的简称。
date: 2019-05-02 00:00:00 Z
categories: javascript
---

## 堆（heap）栈（stack）

> 堆是堆内存的简称，栈是栈内存的简称。

堆栈，主要的问题就是内存的使用和分配了。堆是动态分配内存，内存大小不一，也不会自动释放（常见的内存溢出问题）。栈是自动分配相对固定大小的内存空间，并由系统自动释放。池存放常量，所以也叫常量池。

- 基本数据类型

Undefined、Null、Boolean、String、Number、Symbol都是直接按值直接存在栈中，每种类型的数据占用的内存空间大小都是固定的，并且由系统自动分配自动释放

- 引用数据类型

Object，Array，Function这样的数据存在堆内存中，但是数据指针是存放在栈内存中的，当我们访问引用数据时，先从栈内存中获取指针，通过指针在堆内存中找到数据

> 需要注意的是闭包中的基本数据类型变量不保存在栈内存中，而是保存在堆内存中。
<style>li {font-size: 16px; font-weight: bold;}</style>
---

{% highlight javascript %}
let name = 'phuhoang';
let age = 18;

let people = {
  name: '黄存亚',
  age: '20'
};
let hobby = ['乒乓球', '游泳', '码'];
{% endhighlight %}

<div style="display: flex; width: 60%; flex-direction:row; padding: 20px 30px; border: solid 1px #eee; margin-bottom: 20px;">
  <div style="flex: 1">
    <table>
      <tr>
        <th align="center" colspan="2">栈内存</th>
      </tr>
      <tr>
        <th align="center">变量名</th>
        <th align="center">值</th>
      </tr>
      <tr>
        <td align="center">name</td>
        <td align="center">'phuhoang'</td>
      </tr>
      <tr>
        <td align="center">age</td>
        <td align="center">18</td>
      </tr>
      <tr>
        <td align="center">people</td>
        <td align="center">0x0012ff3d</td>
      </tr>
      <tr>
        <td align="center">hobby</td>
        <td align="center">0x0012ffe1</td>
      </tr>
    </table>
  </div>
  <div style="flex: 1; margin-left: 20px">
    <table>
      <tr>
        <th align="center" colspan="2">堆内存</th>
      </tr>
      <tr>
        <th align="center">引用</th>
        <th align="center">值</th>
      </tr>
      <tr>
        <td align="center">0x0012ffe1</td>
        <td align="center">{name: '黄存亚', age: 20}'</td>
      </tr>
      <tr>
        <td align="center">0x0012ffe1</td>
        <td align="center">['乒乓球', '游泳', '码']</td>
      </tr>
    </table>
  </div>
</div>

因此当我们要访问堆内存中的引用数据类型时，实际上我们首先是从变量中获取了该对象的地址指针， 然后再从堆内存中取得我们需要的数据。


---


### 基本数据类型的复制


{% highlight javascript %}
let name = 'phuhoang';
let copyName = name;
copyName = '黄存亚';
console.log(copyName)     // '黄存亚';
{% endhighlight %}

<div style="display: flex; flex-direction:row; padding: 20px 30px; border: solid 1px #eee; margin-bottom: 20px;">
  <div style="flex: 1">
    <h4 style="text-align: center">复制前</h4>
    <table>
      <tr>
        <th align="center">变量名</th>
        <th align="center">值</th>
      </tr>
      <tr>
        <td align="center">name</td>
        <td align="center">'phuhoang'</td>
      </tr>
    </table>
  </div>
  <div style="flex: 1; margin: 0 20px">
    <h4 style="text-align: center">复制后</h4>
    <table>
      <tr>
        <th align="center">变量名</th>
        <th align="center">值</th>
      </tr>
      <tr>
        <td align="center">name</td>
        <td align="center">'phuhoang'</td>
      </tr>
      <tr>
        <td align="center">newName</td>
        <td align="center">'phuhoang'</td>
      </tr>
    </table>
  </div>
  <div style="flex: 1">
    <h4 style="text-align: center">值修改后</h4>
    <table>
      <tr>
        <th align="center">变量名</th>
        <th align="center">值</th>
      </tr>
      <tr>
        <td align="center">name</td>
        <td align="center">'phuhoang'</td>
      </tr>
      <tr>
        <td align="center">newName</td>
        <td align="center">'黄存亚'</td>
      </tr>
    </table>
  </div>
</div>



在这个例子中，name、copyName 都是基本类型，它们的值是存储在栈内存中的，name、copyName 分别有各自独立的栈空间， 所以修改了 copyName 的值以后，name 的值并不会发生变化。


--- 

### 引用数据类型的复制


{% highlight javascript %}
let me = {
  name: 'phuhoang',
  age: 18
};
let copyMe = me;
copyMe.name = '黄存亚';
console.log(me.name)     // '黄存亚';
{% endhighlight %}

在这个例子中，me、copyMe都是引用类型，栈内存中存放地址指向堆内存中的对象， 引用类型的复制会为新的变量自动分配一个新的值保存在变量中， 但只是引用类型的一个地址指针而已，实际指向的是同一个对象， 所以修改 copyMe.name 的值后，相应的 me.name 也就发生了改变。

<div style=" padding: 20px 30px; border: solid 1px #eee; margin-bottom: 20px;">
  <h4 style="text-align: center">复制前</h4>
  <div style="display: flex; flex-direction:row;">
    <div style="flex: 1">
      <table>
        <tr>
          <th align="center" colspan="2">栈内存</th>
        </tr>
        <tr>
          <th align="center">变量名</th>
          <th align="center">值</th>
        </tr>
        <tr>
          <td align="center">me</td>
          <td align="center">0x0012ffe1</td>
        </tr>
      </table>
    </div>
    <div style="flex: 1; margin-left: 20px">
      <table>
        <tr>
          <th align="center" colspan="2">堆内存</th>
        </tr>
        <tr>
          <th align="center">引用</th>
          <th align="center">值</th>
        </tr>
        <tr>
          <td align="center">0x0012ffe1</td>
          <td align="center">{name: 'phuhoang', age: 18}</td>
        </tr>
      </table>
    </div>
  </div>
</div>

<div style=" padding: 20px 30px; border: solid 1px #eee; margin-bottom: 20px;">
  <h4 style="text-align: center">复制后</h4>
  <div style="display: flex; flex-direction:row;">
    <div style="flex: 1">
      <table>
        <tr>
          <th align="center" colspan="2">栈内存</th>
        </tr>
        <tr>
          <th align="center">变量名</th>
          <th align="center">值</th>
        </tr>
        <tr>
          <td align="center">me</td>
          <td align="center">0x0012ffe1</td>
        </tr>
        <tr>
          <td align="center">copyMe</td>
          <td align="center">0x0012ffe1</td>
        </tr>
      </table>
    </div>
    <div style="flex: 1; margin-left: 20px">
      <table>
        <tr>
          <th align="center" colspan="2">堆内存</th>
        </tr>
        <tr>
          <th align="center">引用</th>
          <th align="center">值</th>
        </tr>
        <tr>
          <td align="center">0x0012ffe1</td>
          <td align="center">{name: 'phuhoang', age: 18}</td>
        </tr>
      </table>
    </div>
  </div>
</div>


---

## 栈内存和堆内存的优缺点

在JS中，基本数据类型变量大小固定，并且操作简单容易，所以把它们放入栈中存储。

引用类型变量大小不固定，所以把它们分配给堆中，让他们申请空间的时候自己确定大小，这样把它们分开存储能够使得程序运行起来占用的内存最小。

栈内存由于它的特点，所以它的系统效率较高。

堆内存需要分配空间和地址，还要把地址存到栈中，所以效率低于栈。


---


## 闭包与堆内存

[关于闭包详细内容参见另一篇文章(JavaScript深入理解-闭包（Closure）)](/javascript/2020/03/01/closure.html)

闭包中的变量并不保存中栈内存中，而是保存在堆内存中。 这也就解释了函数调用之后之后为什么闭包还能引用到函数内的变量。

{% highlight javascript %}
function A() {
  let a = 1;
  function B() {
    console.log(a);
  }
  return B;
}
let res = A();
{% endhighlight %}
函数 A 返回了一个函数 B，并且函数 B 中使用了函数 A 的变量，函数 B 就被称为闭包。

函数 A 弹出调用栈后，函数 A 中的变量这时候是存储在堆上的，所以函数B依旧能引用到函数A中的变量。

现在的 JS 引擎可以通过逃逸分析辨别出哪些变量需要存储在堆上，哪些需要存储在栈上。

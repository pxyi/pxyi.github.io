---
layout: article
title: 最简单的方式实现瀑布流效果
keywords: 瀑布流效果,css实现瀑布流,简单的瀑布流实现方式
description: 最简单的方式实现瀑布流效果
date: 2019-03-03 00:00:00 Z
categories: htmlcss
---

瀑布流效果常用于不同规格的图片或商品展示页面，大多包含图片，因无法确定图片大小所以使用瀑布流效果更加美观。
早期实现瀑布流效果大多是先获取所有数据确定其高度之后通过Js分配至各列，虽说其原理挺简单的，但是实现起来至少也得100多行代码，比较浪费时间。

{% highlight css %}
.waterfall-box {
    /* 分几列 */
    column-count: 2;
    /* 列间距，默认30px */
    column-gap: 0;
}
.waterfall-item {
    /* 去除留白 */
    break-inside: avoid;
}
{% endhighlight %}

> 这样Css会根据元素高度自动分配至各列，保存均匀的列表长度。
但是有缺点：这个列表顺序是左边 1 2 3，右边 4 5 6，不符合正常的展示逻辑，正常来说应该左边 1 3 5，右边 2 4 6；所以我们需要一段Js代码来对列表进行排序

所以我们需要一段Js代码来对列表进行排序

{% highlight javascript %}
const oldList = [1, 2, 3, 4, 5, 6, 7]

// 使用reduce函数接受一个初始值{ 0: [], 1: [], length: 2 },
// 初始值包含两个空数组，和一个数组长度(Array.from方法要求将对象转数组时对象内要有这个属性)
// 在reduce函数内根据索引做余2判断，因为分两，余0的加入第一个数组，余1的加入第二个数组
// 最后reduce返回遍历完的对象 {0:[1,3,5,7],1:[2,4,6],length:2}
// 使用Array.from({0:[1,3,5,7],1:[2,4,6],length:2}) 得到 数组 [[1,3,5,7],[2,4,6]]
// 解构数组 使用concat合并，完事
const newList = [].concat(...(Array.from(oldList.reduce((total, cur, index) => {
  total[index % 2].push(cur)  
  return total
}, { 0: [], 1: [], length: 2 }))))

console.log(newList) // [1, 3, 5, 7, 2, 4, 6]
{% endhighlight %}
最后附上html代码
{% highlight html %}
<div class="waterfall-box">
    <div class="waterfall-item" v-for="i in newList"> 
      // Vue语法输出内容
    </div>
</div>
{% endhighlight %}
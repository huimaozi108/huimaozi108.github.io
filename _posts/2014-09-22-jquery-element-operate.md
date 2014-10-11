---
layout: post
title: JQuery中html、append、appendTo、after、insertAfter、before、insertBefore、empty、remove系列方法的使用
category: 技术
tags: [JQuery, JS, HTML]
keywords: JQuery,JS,Javascript,HTML
description:
---

>做前端开发的，免不了的要操作页面HTML代码，JQuery中提供了许多非常便捷的方法，使我们很方便的操作HTML代码。

##相关方法

1. `$.html()`：给元素添加html代码或者清空元素内部的内容（参数为**空字符串**）
2. `$.append()`：向元素的末尾添加元素
3. `$.appendTo()`：该方法同append方法，将元素添加到该方法参数所描述的元素中，该元素可以为HtmlElement对象或者JQuery对象
4. `$.after()`：向元素的后边添加元素，如果该元素后面有同级元素，则将后边的同级元素后移，然后将元素插入
5. `$.before()`：向元素的前边添加元素，如果该元素前面有同级元素，则将前边的同级元素前移，然后将元素插入
6. `$.insertAfter()`：将元素插入到指定元素（**参数指定**）的后面，如果元素后面有元素了，那将后面的元素后移，然后将JQuery对象插入
7. `$.insertBefore()`：将元素插入到指定元素（**参数指定**）的前面，如果元素前面有元素了，那将前面的元素前移，然后将JQuery对象插入
8. `$.empty()`：清空元素**内部**的所以内容，它只清空内部的内容，但标记仍在DOM中
9. `$.remove()`：从DOM中移除整个元素（包含元素本身）
需要指出一点：**JQuery会自动的完善html代码**，比如你执行以下操作

```js
$('content').append('<p>');
```

那么实际插入到DOM中的html代码是

```html
<p></p>
```

下面的所有操作将针对以下代码，代码及效果如下：

*css代码*

```css
.box  
 {  
     width:100px;   
     height:100px;   
     border:2px solid gray;  
     padding:10px;  
     text-align:center;
     float:left;
 }  
 .child  
 {  
     width:70px;   
     height:20px;   
     border:2px solid black;  
     text-align:center;  
 }
```

*html代码*

```html
<div id="left" class="box">  
    <span>left</span>  
</div>  
<div id="center" class="box">  
    <span>center</span>  
</div>  
<div id="right" class="box">  
    <span>right</span>  
</div>
```

效果如下：

<style type="text/css">
    .box  
    {  
        width:100px;   
        height:100px;   
        border:2px solid gray;  
        padding:10px;  
        text-align:center;
        float: left;
    }  
    .child  
    {  
        width:70px;   
        height:20px;   
        border:2px solid black;  
        text-align:center;  
    }
</style>
<div id="left" class="box">
    <span>left</span>
</div>
<div id="center" class="box">
    <span>center</span>
</div>
<div id="right" class="box">
    <span>right</span>
</div>
<div style="clear:both;"></div>

###一、`$.html()`方法的使用

我们执行以下语句：

```js
$('#center').html('<div class="child">$.html()');
```

<iframe src="/post_res/2014-09-22/code/html_html.html"
    name="html1" frameborder="0"  scrolling="no" 
    style="width:80%;"
    onload="this.height=window.frames['html1'].document.body.scrollHeight;"></iframe>

再来看看最终代码

```html
<div id="center" class="box">
    <div class="child">$.html()</div>
</div>
```

这里是完整的html代码，也就是说*JQuery为我们补全了代码*，这一点需要注意，JQuery为我们带来了方便，但是当需要精确控制html代码的时候，需要注意。

`html()`函数的作用原理首先是移除目标元素内部的html代码，然后将新的代码添加到目标元素。

###二、`append()`、`appendTo()`方法的使用

执行以下语句：

    $('#center').append('<span class="child">append()</span>');

*注：为了编程规范，html代码都使用完整的html标记*

效果如下：

<iframe src="/post_res/2014-09-22/code/html_append_appendto.html"
    name="html2" frameborder="0"  scrolling="no" 
    style="width:80%;"
    onload="this.height=window.frames['html2'].document.body.scrollHeight;"></iframe>

最终代码：

```html
<div id="center" class="box">
    <span>center</span>
    <span class="child">$.append()</span>
</div>
```

`append()`方法将html附加到了`<span>center</span>`之后。

`appendTo()`方法的效果与`append()`方法一样，只不过是写法不一样：

    $('<span class="child">$.append()</span>').appendTo('#center');

###三、`after()`、`insertAfter()`方法的使用

执行以下语句：

    $('#center').after('<span class="child" style="float:left;">$.after()</span>');

效果如下：

<iframe src="/post_res/2014-09-22/code/html_after_insertafter.html"
    name="html3" frameborder="0"  scrolling="no" 
    style="width:80%;"
    onload="this.height=window.frames['html3'].document.body.scrollHeight;"></iframe>

after函数的作用是将html代码插入指定元素的后面，如果后面有元素，则将其后移，然后插入html代码。

insertAfter函数与after函数的作用是一样的，上面代码的insertAfter版本为：

    $('<span class="child" style="float:left;">after()</span>').insertAfter('#center');

###四、`before()`、`insertBefore()`方法的使用

这连个函数的原理与after、insertAfter是一样的，只不过before、insertBefore是插入到目标元素的前面，大家可以自行参考after、insertAfter。

###五、`empty()`、`remove()`方法的使用

执行以下代码：

    $('#center').empty();

<iframe src="/post_res/2014-09-22/code/html_empty.html"
    name="html4" frameborder="0"  scrolling="no" 
    style="width:80%;"
    onload="this.height=window.frames['html4'].document.body.scrollHeight;"></iframe>

再来看看最终的html代码：

    <div id="center" class="box"></box>

里面的html代码被情况了，然而元素的本身还是存在的。

执行以下语句：

    $('#center').remove();

<iframe src="/post_res/2014-09-22/code/html_remove.html"
    name="html5" frameborder="0"  scrolling="no" 
    style="width:80%;"
    onload="this.height=window.frames['html5'].document.body.scrollHeight;"></iframe>

最终的html代码：

```html
<div id="left" class="box">
    <span>left</span>
</div>
<div id="right" class="box">
    <span>right</span>
</div>
```

中间的元素整个被删除了，也就是说`remove()`方法将目标元素整个从DOM中移除。
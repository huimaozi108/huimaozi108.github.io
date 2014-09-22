---
layout: post
title: JQuery中html、append、appendTo、after、insertAfter、before、insertBefore、empty、remove系列方法的使用
category: 技术
tags: [JQuery, JS, HTML]
keywords: JQuery,JS,Javascript,HTML
description:
---

>做前端开发的，免不了的要操作页面HTML代码，JQuery中提供了许多非常便捷的方法，使我们很方便的操作HTML代码，具体的方法有：

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
     border:2px solid red;  
     padding:10px;  
     text-align:center;
 }  
 .child  
 {  
     width:70px;   
     height:20px;   
     border:2px solid red;  
     text-align:center;  
 }
```

*html代码*

```html
<div id="top" class="box">  
    <span>top</span>  
</div>  
<div id="middle" class="box">  
    <span>middle</span>  
</div>  
<div id="bottom" class="box">  
    <span>bottom</span>  
</div>
```

效果如下：

<style type="text/css">
    .box  
    {  
        width:100px;   
        height:100px;   
        border:2px solid red;  
        padding:10px;  
        text-align:center;
    }  
    .child  
    {  
        width:70px;   
        height:20px;   
        border:2px solid red;  
        text-align:center;  
    }
</style>
<div id="top" class="box">
    <span>top</span>
</div>
<div id="middle" class="box">
    <span>middle</span>
</div>
<div id="bottom" class="box">
    <span>bottom</span>
</div>

###`$.html()`方法的使用

我们执行以下语句：

```js
$('#middle').html('<div class="child">$.html()');
```

<form>
    <input type="button" value="执行"
        onclick="javascript:$(this).next().find('#middle').html('<div class="child">html');" />
    <div>
        <div id="top" class="box">
            <span>top</span>
        </div>
        <div id="middle" class="box">
            <span>middle</span>
        </div>
        <div id="bottom" class="box">
            <span>bottom</span>
        </div>
    </div>
</form>



---
title: jQuery-笔记
author: god23bin
categories: 前端
tags: 
- jQuery
- 前端
- 笔记
abbrlink: b26d2e2d
description: jQuery 笔记记录
date: 2020-02-10 13:12:33
---



## 初体验-Learn By Imooc

当页面加载完成后，在页面中以居中的方式显示“您好！通过慕课网学习 jQuery 才是最佳的途径”字样。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8"/>
    <title>第一个简单的jQuery程序</title>
    <style type="text/css">
        div{
            padding:8px 0px;
            font-size:12px;
            text-align:center;
            border:1px solid #888;
        }
    </style>
    <script src="https://www.imooc.com/static/lib/jquery/1.9.1/jquery.js"></script>
    <script type="text/javascript">
            $(document).ready(function() {
                    $("div").html("您好！通过慕课网学习jQuery才是最佳的途径。");
            });
    </script>
</head>
<body>
    <div></div>
</body>
</html>

```

> 代码分析：
>
>  $(document).ready 的作用是等页面的文档（document）中的节点都加载完毕后，再执行后续的代码，因为我们在执行代码的时候，可能会依赖页面的某一个元素，我们要确保这个元素真正的的被加载完毕后才能正确的使用。

---

### jQuery对象与DOM对象

对于才开始接触jQuery库的初学者，我们需要清楚认识一点：

**jQuery对象与DOM对象是不一样的**

可能一时半会分不清楚哪些是jQuery对象，哪些是DOM对象，下面重点介绍一下jQuery对象，以及两者相互间的转换。

```html
<!-- 通过一个简单的例子，简单区分下jQuery对象与DOM对象： -->
<p id=”imooc”></p>
<!-- 
我们要获取页面上这个id为imooc的p元素，然后给这个文本节点增加一段文字：“您好！通过慕课网学习jQuery才是最佳的途径”，并且让文字颜色变成红色。
-->

```

- 普通处理，通过标准JavaScript处理：

  ```js
  var p = document.getElementById('imooc');
  p.innerHTML = '您好！通过慕课网学习jQuery才是最佳的途径';
  p.style.color = 'red';
  
  // 通过原生DOM模型提供的document.getElementById(“imooc”) 方法获取的DOM元素就是一个DOM对象，再通过innerHTML与style属性处理文本与颜色。
  
  ```

- jQuery的处理：

  ```js
  var $p = $('#imooc');
  $p.html('您好！通过慕课网学习jQuery才是最佳的途径').css('color','red');
  
  // 通过$('#imooc')方法会得到一个$p的jQuery对象，$p是一个类数组对象。这个对象里面包含了DOM对象的信息，然后封装了很多操作方法，调用自己的方法html与css，得到的效果与标准的JavaScript处理结果是一致的。
  
  ```

通过标准的JavaScript操作DOM与jQuery操作DOM的对比，我们不难发现：

1. 通过jQuery方法包装后的对象，是一个类数组对象。它与DOM对象完全不同，唯一相似的是它们都能操作DOM。
2. 通过jQuery处理DOM的操作，可以让开发者更专注业务逻辑的开发，而不需要我们具体知道哪个DOM节点有那些方法，也不需要关心不同浏览器的兼容性问题，我们通过jQuery提供的API进行开发，代码也会更加精短。

---

### jQuery对象转化成DOM对象

jQuery库本质上还是JavaScript代码，它只是对JavaScript语言进行包装处理，为的是提供更好更方便快捷的DOM处理与开发中经常使用的功能。我们使用jQuery的同时也能混合JavaScript原生代码一起使用。在很多场景中，我们需要jQuery与DOM能够相互的转换，它们都是可以操作的DOM元素，**jQuery是一个类数组对象，而DOM对象就是一个单独的DOM元素**。

**如何把jQuery对象转成DOM对象？**

```js
// 利用数组下标的方式读取到jQuery中的DOM对象

<div>元素一</div>
<div>元素二</div>
<div>元素三</div>

var $div = $('div') //jQuery对象
var div = $div[0] //转化成DOM对象
div.style.color = 'red' //操作dom对象的属性
```

用jQuery找到所有的div元素（3个），因为jQuery对象也是一个数组结构，可以通过数组下标索引找到第一个div元素，通过返回的div对象，调用它的style属性修改第一个div元素的颜色。这里需要注意的一点是**，数组的索引是从0开始的，也就是第一个元素下标是0**

**最主要的是，我们要会通过jQuery自带的get()方法：**

jQuery对象自身提供一个.get() 方法允许我们直接访问jQuery对象中相关的DOM节点，get方法中提供一个元素的索引

```js
var $div = $('div') //jQuery对象
var div = $div.get(0) //通过get方法，转化成DOM对象
div.style.color = 'red' //操作dom对象的属性
```

其实我们翻开源码，看看就知道了，get方法就是利用的第一种方式处理的，只是包装成一个get让开发者更直接方便的使用。

---

### DOM对象转化成jQuery对象

相比较jQuery转化成DOM，开发中更多的情况是把一个DOM对象加工成jQuery对象。

$(参数)是一个多功能的方法，通过传递不同的参数而产生不同的作用。

如果传递给$(DOM)函数的参数是一个DOM对象，jQuery方法会把这个DOM对象给包装成一个新的jQuery对象。

通过$(DOM)方法将普通的DOM对象加工成jQuery对象之后，我们就可以调用jQuery的方法了。

```js
<div>元素一</div>
<div>元素二</div>
<div>元素三</div>

var div = document.getElementsByTagName('div'); //dom对象
var $div = $(div); //jQuery对象
var $first = $div.first(); //找到第一个div元素
$first.css('color', 'red'); //给第一个元素设置颜色

// 通过getElementsByTagName获取到所有div节点的元素，结果是一个dom合集对象，不过这个对象是一个数组合集(3个div元素)。通过$(div)方法转化成jQuery对象，通过调用jQuery对象中的first与css方法查找第一个元素并且改变其颜色。
```

---

## jQuery选择器

页面的任何操作都需要节点的支撑，开发者如何快速高效的找到指定的节点也是前端开发中的一个重点。jQuery提供了一系列的选择器帮助开发者达到这一目的，让开发者可以更少的处理复杂选择过程与性能优化，更多专注业务逻辑的编写。

jQuery几乎支持主流的css1~css3选择器的写法，我们从最简单的也是最常用的开始学起。

### id选择器

id选择器：一个用来查找的ID，即元素的id属性

```js
$("#id");
```

id选择器也是基本的选择器，jQuery内部使用JavaScript函数document.getElementById()来处理ID的获取。原生语法的支持总是非常高效的，所以在操作DOM的获取上，如果能采用id的话尽然考虑用这个选择器

值得注意：**id是唯一的，每个id值在一个页面中只能使用一次。如果多个元素分配了相同的id，将只匹配该id选择集合的第一个DOM元素。但这种行为不应该发生;有超过一个元素的页面使用相同的id是无效的。**

---

### 类选择器

类选择器，顾名思义，通过class样式类名来获取节点。

```js
$(".class");
```

类选择器，相对id选择器来说，效率相对会低一点，但是优势就是可以多选

同样的jQuery在实现上，对于类选择器，如果浏览器支持，jQuery使用JavaScript的原生getElementsByClassName()函数来实现的。

**链式调用**，$(".imooc").css()方法内部肯定是带了一个隐式的循环处理，所以使用jQuery选择节点，不仅仅只是选择上的简单，同时还增加很多关联的便利操作，后续我们还会慢慢的学到其他的优势。

---

### 元素/标签-选择器

元素选择器：根据给定（html）标签名称（比如p标签，hr标签，h1标签，div标签这些）选择所有的元素

```js
// 第一组：通过getElementsByTagName方法得到页面所有的<div>元素
var divs = document.getElementsByTagName('div');
// divs是dom合集对象，可以干嘛？那么做你想做的事，比如通过循环给每一个合集中的<div>元素赋予新的border样式

// 第二组：同样的效果，$("p")选取所有的<p>元素，通过css方法直接赋予样式
var ps = $("p");
// 同理，获取了所有的p标签，即所有的<p>元素

```

---

### 全选择器（*选择器）

在CSS中，经常会在第一行写下这样一段样式

```css
\* {padding: 0; margin: 0;}
```

通配符 \* 意味着给所有的元素设置默认的边距。jQuery中我们也可以通过传递 \* 选择器来选中文档页面中的元素。

```js
$( "*" );
```

---

### 层级选择器

文档中的所有的节点之间都是有这样或者那样的关系。我们可以把节点之间的关系可以用传统的家族关系来描述，可以把文档树当作一个家谱，那么节点与节点直接就会存在父子，兄弟，祖孙的关系了。

选择器中的层级选择器就是用来处理这种关系，即**子元素 后代元素 兄弟元素 相邻元素**

通过一个列表，对比层级选择器的区别

|                | 层级选择器               | 描述                                                         |
| -------------- | ------------------------ | ------------------------------------------------------------ |
| 子选择器       | $("parent > child")      | 选择所有"parent"元素中指定的"child"的直接子元素              |
| 祖先后代选择器 | $("ancestor descendant") | 选择给定的祖先的所有后代元素<br/>一个元素的后代可能是<br/>该元素的一个孩子、孙子、曾孙等等。 |
| 相邻兄弟选择器 | $("prev + next")         | 选择所有紧跟在"prev"元素后的"next"元素                       |
| 一般兄弟选择器 | $("prev ~ siblings")     |                                                              |

1. 层级选择器都有一个参考节点，位置在前面的就是参考节点
2. 后代选择器包含子选择器的选择的内容
3. 一般兄弟选择器包含相邻兄弟选择的内容
4. 相邻兄弟选择器和一般兄弟选择器所选择到的元素，必须在同一个父元素下

---

### 基本筛选-选择器

很多时候我们不能直接通过基本选择器与层级选择器找到我们想要的元素，为此jQuery提供了一系列的筛选选择器用来更快捷的找到所需的DOM元素。筛选选择器很多都不是CSS的规范，而是jQuery自己为了开发者的便利延展出来的选择器

**基本筛选选择器针对的都是元素DOM节点**

筛选选择器的用法与CSS中的伪元素相似，**选择器用冒号“：”开头**，通过一个列表，看看基本筛选器的描述：

| 选择器               | 描述                                                         |
| -------------------- | ------------------------------------------------------------ |
| $(":first")          | 匹配第一个元素                                               |
| $(":last")           | 匹配最后一个元素                                             |
| $(":not(selector)")  | 一个用来过滤的选择器，选择所有元素去除不匹配给定的选择器元素 |
| $(":eq(index)")      | 选择匹配的**集合中**索引值为index的元素                      |
| $( ":gt(index)")     | 选择匹配的**集合中**所有大于给定index(索引值)的元素          |
| $(":even")           | 选择索引值为偶数的元素，从0开始计数                          |
| $(":odd")            | 选择索引值为奇数的元素，从0开始计数                          |
| $( ":lt(index)")     | 选择匹配集合中所有索引值小于给定index参数的元素              |
| $(":header")         | 选择所有标题元素，像h1,h2,h3等                               |
| $(":lang(language)") | 选择指定语言的所有元素                                       |
| $(":root")           | 选择i该文档的根元素                                          |
| $(":animated")       | 选择所有正在执行动画效果的元素                               |

1. :eq(), :lt(), :gt(), :even, :odd 用来筛选他们前面的匹配表达式的集合元素，根据之前匹配的元素在进一步筛选，注意jQuery合集都是从0开始索引
2. gt是一个段落筛选，从指定索引的下一个开始，gt(1) 实际从2开始



### 内容筛选-选择器

基本筛选选择器针对的都是元素DOM节点，如果我们要通过内容来过滤，jQuery也提供了一组内容筛选选择器，当然其规则也会体现在它所包含的子元素或者文本内容上。

| 选择器               | 描述                                    |
| -------------------- | --------------------------------------- |
| $(":contains(text)") | 选择所有包含指定文本的元素              |
| $(":parent")         | 选择所有含子元素或者文本的元素          |
| $(":empty")          | 选择所有不含子元素的元素（包含文本节点) |
| $(":has(selector)")  | 选择元素中至少包含指定选择器的元素      |

1. :contains与:has都有查找的意思，但是contains查找包含“指定文本”的元素，has查找包含“指定元素”的元素
2. 如果:contains匹配的文本包含在元素的子元素中，同样认为是符合条件的。
3. :parent与:empty是相反的，两者所涉及的子元素，包括文本节点

---

### 可见性选择器



### 属性选择器

属性选择器让你可以基于属性来定位一个元素。

可以只指定该元素的某个属性，这样所有使用该属性而不管它的值，这个元素都将被定位，也可以更加明确并定位在这些属性上使用特定值的元素，这就是属性选择器展示它们的威力的地方。

| 选择器                                     | 描述                                                         |
| ------------------------------------------ | ------------------------------------------------------------ |
| $("[attribute\|='value']")                 | 选择指定属性值等于给定字符串或以该文字串为前缀<br>(该字符串后跟—个连字符"-")的元素 |
| **$("[attribute* ='value]")**              | 选择指定属性具有包含一个给定的子字符串的元素<br>(选择给定的属性是以包含某些值的元素) |
| $("[attribute~='value']")                  | 选择指定属性用空格分隔的值中包含一个给定值的元素             |
| **$("[attribute='value']")**               | 选择指定属性是给定值的元素                                   |
| $("[attribute!='value']")                  | 选择不存在指定属性，或者指定的属性值不等于给定值的元素       |
| $("[attribute ='value']")                  | 选择指定屈性是以给定字符串开始的元素                         |
| $("[attribute$='value']")                  | 选择指定属性是以给定值结尾的元素，这个比较是区分大小写的     |
| $("[attribute]")                           | 选择所有具有指定属性的元素，i该属性可以是任何值              |
| $("\[attributeFilter1][attributeFilterN]") | 选择匹配所有指定的屈性筛选器的元素                           |

**在这么多属性选择器中[attr="value"]和[attr*="value"]是最实用的**

[attr="value"]能帮我们定位不同类型的元素，特别是表单form元素的操作，比如说input[type="text"],input[type="checkbox"]等

[attr*="value"]能在网站中帮助我们匹配不同类型的文件

---

### 子元素选择器



### 表单元素选择器

无论是提交还是传递数据，表单元素在动态交互页面的作用是非常重要的。jQuery中专门加入了表单选择器，从而能够极其方便地获取到某个类型的表单元素。

| 选择器         | 描述                                      |
| -------------- | ----------------------------------------- |
| $(":input")    | 选择所有input,textarea,select和button元素 |
| $(":text")     | 匹配所有文本框                            |
| $(":password") | 匹配所有密码框                            |
| $(":radio")    | 匹配所有单选按钮                          |
| $(":checkbox") | 匹配所有复选框                            |
| $(":submit")   | 匹配所有提交按钮                          |
| $(":image")    | 匹配所有图像域                            |
| $(":reset")    | 匹配所有重置按钮                          |
| $(":button")   | 匹配所有按钮                              |
| $(":file")     | 匹配所有文件域                            |

除了input筛选选择器，几乎每个表单类别筛选器都对应一个input元素的type值。

大部分表单类别筛选器可以使用属性筛选器替换。比如 $(':password') == $('[type=password]')

---

### 表单元素属性筛选-选择器

| 选择器         | 描述                     |
| -------------- | ------------------------ |
| $(";enabled")  | 选取可用的表单元素       |
| $(":disabled") | 选取不可用的表单元素     |
| $(":checked")  | 选取被选中的<input>元素  |
| $(":selected") | 选取被选中的<option>元素 |

表单元素属性筛选-选择器也是专门针对表单元素的选择器，可以附加在其他选择器的后面，主要功能是对所选择的表单元素进行筛选。

1. 选择器适用于复选框和单选框，**对于下拉框元素, 使用 :selected 选择器**
2. 在某些浏览器中，选择器:checked可能会错误选取到<option>元素，所以保险起见换用选择器input:checked，确保只会选取<input>元素

---

### 特殊选择器-this

相信很多刚接触jQuery的人，很多都会对$(this)和this的区别模糊不清，**那么这两者有什么区别呢？**

this是JavaScript中的关键字，指的是当前的上下文对象，简单的说就是方法/属性的所有者。

```js
// 下面例子中，imooc是一个对象，拥有name属性与getName方法,在getName中this指向了所属的对象imooc
var imooc = {
    name:"慕课网",
    getName:function(){
        //this,就是imooc对象
        return this.name;
    }
}
imooc.getName(); //慕课网
```

当然在JavaScript中this是动态的，也就是说这个上下文对象都是可以被动态改变的(可以通过call,apply等方法)，具体的大家可以查阅相关资料。

同样的在DOM中this就是指向了这个html元素对象，因为this就是DOM元素本身的一个引用

```js
// 假如给页面一个P元素绑定一个事件:
p.addEventListener('click',function(){
    // this === p
    // 以下两者的修改都是等价的
    this.style.color = "red";
    p.style.color = "red";
},false);
	// 通过addEventListener绑定的事件回调中，this指向的是当前的dom对象，所以再次修改这样对象的样式，只需要通过this获取到引用即可
	this.style.color = "red"
	// 但是这样的操作其实还是很不方便的，这里面就要涉及一大堆的样式兼容，如果通过jQuery处理就会简单多了，我们只需要把this加工成jQuery对象
```

换成jQuery的做法：

```js
$('p').click(function(){
    //把p元素转化成jQuery的对象
    var $this= $(this) 
    $this.css('color','red')
})
// 通过把$()方法传入当前的元素对象的引用this，把这个this加工成jQuery对象，我们就可以用jQuery提供的快捷方法直接处理样式了
```

总而言之：

```js
this 		// 表示当前的上下文对象是一个html对象，可以调用html对象所拥有的属性和方法。
$(this)		// 代表的上下文对象是一个jquery的上下文对象，可以调用jQuery的方法和属性值。
```

## jQuery属性和样式


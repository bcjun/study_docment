## JavaScrip基础

+ **JavaScript**:
  + 动态
  + 弱类型
  + 解释型语言
+ **JavaScript执行方式**
  + 行间事件
  + 页面script标签嵌入
  + 外部引入

+ **JavaScript使用方式**

~~~javascript
//第一种方法：将javascript语句放到window.
<script type="text/javascript">
	window.onload = function()
	{var oDiv = document.getElementById('div1');}
</script>

// 第二种方法：将javascript放到页面最下边(既放在</body>上面)
<script type="text/javascript">
var oDiv = document.getElementById('div1');
</script>

~~~



+ **注释**

  + 单行注释

    > ~~~javascript
    > // 这是一个单行注释
    > ~~~

  + 多行注释

    > ~~~javascript
    > /* 这是一个多行注释*/
    > ~~~

+ **运算符**

  | 符号 | 含义 | 例子            |
  | ---- | ---- | --------------- |
  | +    | 加   | 1+1 =2          |
  | -    | 减   | 2-1=1           |
  | *    | 乘   | 2*2=4           |
  | /    | 除   | 4/2=2           |
  | %    | 取余 | 10%3=1          |
  | ++   | 自加 | a++ 等同于a=a+1 |
  | --   | 自减 | a--等同于a=a-1  |

+ **逻辑运算符**

  | 判断符     | 含义           |
  | ---------- | -------------- |
  | ==         | 判断值是否相等 |
  | ===        | 判断类型跟值   |
  | >          | 大于           |
  | <=         | 小于等于       |
  | !=         | 不等于         |
  | **逻辑符** | **含义**       |
  | &&         | 而且           |
  | \|\|       | 或者           |
  | !          | 否             |

+ **数据类型**

| 类型                   | 含义                  |
| ---------------------- | --------------------- |
| number                 | 数字类型              |
| string                 | 字符串类型            |
| boolean                | 布尔类型              |
| undefined              | 未定义类型既undefined |
| null                   | 空对象                |
| object                 | 复合类型              |
| PS:定义变量前需要加var |                       |

+ **变量命名**

| 变量类型          | 简称                   | 例子             |
| ----------------- | ---------------------- | ---------------- |
| Object            | 对象o                  | oDiv             |
| Array             | 数组a                  | aItems           |
| String            | 字符串s                | sUserName        |
| Integer           | 整数i                  | iItemCount       |
| Boolean           | 布尔值b                | bIsComplete      |
| Float             | 浮点数f                | fPrice           |
| Function          | 函数fn                 | fnHandler        |
| RegExp            | 正则表达式re           | reEmailCheck     |
| PS:匈牙利命名风格 | JavaScript命名尽量遵守 | 命名与js类型无关 |

## 函数循环判断



+ **函数**

~~~javascript
//定义：
function fnAlert(n1,n2){
    alert('hello!');}
//调用:
fnAlert();
~~~

+ **匿名函数**

~~~javascript
// 定义函数不给名子的,叫做匿名函数.匿名函数可直接赋值给事件
<script type="text/javascript">
    
window.onload = function(){
var oBtn = document.getElementById('btn1');
oBtn.onclick = function (){alert('ok!');}
}
</script>
~~~

+ **封闭函数**

~~~javascript
// 封闭函数可以创造一个独立的空间
// 在封闭函数内定义的变量和函数不会影响外部同名的函数和变量，可以避免命名冲突
(function(){
	var iNum01 = 24;
	function myalert()
	{alert('hello!world');}
    myalert()
 })()
~~~



+ **调试打印**

~~~javascript
//弹窗
alert('hello!');
//控制台输出
console.log("hello");
~~~

+ **条件语句**

~~~javascript
// 单层
var iNum01 = 3;
var iNum02 = 5;
var sTr;
if(iNum01>iNum02)
{sTr = '大于';}
else
{sTr = '小于';}
alert(sTr);
//多层判断
var iNow = 1;
if(iNow==1)
{... ;}
else if(iNow==2)
{ ... ;}
else
{... ;}
~~~

+ **循环语句**

~~~javascript
//for循环
for(var index=0; index < aList.length; index++)
{...}
 
//while循环
var index = 0;
while(index < aList.length){...index++;}
 
//do-while循环
var index = 0;
do{ ...index++; }
while(index < aList.length);
~~~

## 元素操作

+ 元素获取

~~~javascript
var oInput = document.getElementById('input1');
var oA = document.getElementById('link1');
~~~

+ 读取元素

~~~javascript
//读取属性及样式
var sValue = oInput.value;
var sType = oInput.type;
var sName = oInput.name;
var sLinks = oA.href;
var sTxt = oDiv.innerHTML; //读取标签中内容
~~~

+ 修改及添加元素

~~~javascript
oA.style.color = 'red';
oInput.value = "hello"
oDiv.innerHTML = '<a href="http://www.itcast.cn">传智播客<a/>';
~~~

## 事件操作

| 常用事件    | 含义             |
| ----------- | ---------------- |
| onclick     | 鼠标点击事件属性 |
| onmouseover | 鼠标移入事件属性 |
| onmouseout  | 鼠标移出事件属性 |

~~~javascript
// 匿名函数事件操作
<script type="text/javascript">
 
window.onload = function(){
var oBtn = document.getElementById('btn1');
oBtn.onclick = function (){alert('ok!');}
}
</script>
~~~

## JavaScript数据结构

+ **数组操作**:

> ~~~javascript
> //定义一个素组
> var aList2 = [1,2,3,'asd'];
> ~~~

| 方法      | 功能                     | 例子                   |
| --------- | ------------------------ | ---------------------- |
| length    | 获取数组长度             | aList.length           |
| join()    | 连接数组成字符串         | aList.join('-')        |
| push()    | 从数组最后面增加成员     | aList.push(5)          |
| pop()     | 从数组最后面删除成员     | aList.pop()            |
| reverse() | 将数组反转               | aList.reverse();       |
| indexOf() | 返回元素第一次出现的索引 | aList.indexOf(1)       |
| splice()  | 在数组中增加或删除成员   | aList.splice(2,1,7,8,) |
|           | 从索引2开始删除1个元素   | 并在此位置增加7,8      |

+ 字符串操作

> ~~~javascript
> //定义 一个字符串 一个整数
> var iNum01 = 12;
> var sNum02 = '12';
> var sNum03 = '12.12';
> ~~~

| 方法         | 功能               | 例子                       |
| ------------ | ------------------ | -------------------------- |
| parseInt()   | 字符串转化为整数   | parseInt(sNum02)           |
| parseFloat() | 字符串转化为小数   | parseFloat(sNum03)         |
| split()      | 字符串分割成数组   | var aList = sTr.split("-") |
| indexOf()    | 字符串是否含某字符 | sTr.indexOf("c")           |
| substring()  | 截取字符串索引3-5  | sTr.substring(3,5);        |

~~~javascript
// 例子
// 字符串反转
var str = 'asdfj12jlsdkf098';
var str2 = str.split('').reverse().join('');
~~~

## 定时器

| 定时器类型    | 作用                   |
| ------------- | ---------------------- |
| setTimeout    | 只执行一次的定时器     |
| clearTimeout  | 关闭只执行一次的定时器 |
| setInterval   | 反复执行的定时器       |
| clearInterval | 关闭反复执行的定时器   |

+ **例子**

> ~~~javascript
> //开启定时器
> var time1 = setTimeout(function,2000);
> var time2 = setInterval(function,2000);
> //关闭定时器
> clearTimeout(time1);
> clearInterval(time2);
> ~~~
>
> 

## Jquery元素获取

+ **jquery**
  + 是JavaScript的函数库
  + 比原生JavaScript快
  + 代码简洁,动画处理比较有优势

+ **Jquery执行方式**
  + 行内式
  + 嵌入式
  + 引入式

+ **Jquery的使用方式**

~~~javascript
// 执行方式
// 使用Jquery时需要导两次包,先导JS再导JQ
<script type="text/javascript">
 	$(document).ready(function()
                   {......});
</script>
// 简写
<script type="text/javascript">
	$(function(){......});
</script>
~~~

+ **jquery选择器**

| 例子                       | 选择器                 |
| -------------------------- | ---------------------- |
| $('#myId')                 | id选择器               |
| $('.myClass')              | 类选择器               |
| $('#ul1 li span')          | 层级选择器             |
| $('input[name=first]')     | 属性选择器             |
| $('li')                    | 标签选择器             |
| **例子**                   | **选择集过滤器**       |
| $('div').has('p')          | 选择包含p元素的div元素 |
| $('div').eq(5);            | 选择包含p元素的div元素 |
| **例子**                   | **选择集转移**         |
| $('#box').prev()           | 元素前面紧挨的同辈元素 |
| $('#box').prevAll()        | 元素之前所有的同辈元素 |
| $('#box').next()           | 元素后面紧挨的同辈元素 |
| $('#box').nextAll()        | 元素后面所有的同辈元素 |
| $('#box').parent()         | 元素的父元素           |
| $('#box').children()       | 元素的所有子元素       |
| $('#box').siblings()       | 元素的同级元素         |
| $('#box').find('.myClass') | #box下面的.myClass元素 |

> ~~~javascript
> // 判断是否选择到了元素
> var $div1 = $('#div1');
> alert($div2.length); // 弹出0就是没选中,其他数值就是选中
> 
> ~~~

## Jquery元素操作

+ **读取元素**

> ~~~javascript
> //获取div的样式
> $("div").css("width");
> // 取出html内容
> var $htm = $('#div1').html()
> // 取出图片的地址
> var $src = $('#img1').prop('src');
> ~~~

+ **操作元素**

> ~~~javascript
> //设置div的样式
> $("div").css("width","30px");
> $("div").css({fontSize:"30px",color:"red"});
> 
> // 设置html内容
> $('#div1').html('<span>添加文字</span>');
> $div1.append("...") // 在html后面追加内容
> $('#div1').remove() //删除标签
> 
> // 设置图片的地址和alt属性
> $('#img1').prop({src: "test.jpg", alt: "Error" });
> 
> // 设置Value属性
> $div.val("hello world");
> ~~~

+ **操作样式类名**

> ~~~javascript
> $("#div1").addClass("divClass2") //为id为div1的对象追加样式divClass2
> $("#div1").removeClass("divClass")  //移除id为div1的对象的class名为divClass的样式
> $("#div1").removeClass("divClass divClass2") //移除多个样式
> $("#div1").toggleClass("anotherClass") //重复切换anotherClass样式
> 
> ~~~

+ **对选择集进行过滤**

> ~~~javascript
> $('div').has('p'); // 选择包含p元素的div元素
> $('div').eq(5); //选择第6个div元素
> ~~~

## Jquery事件操作

| 事件         | 功能                                         |
| ------------ | -------------------------------------------- |
| click()      | 鼠标单击                                     |
| blur()       | 元素失去焦点                                 |
| focus()      | 元素获得焦点                                 |
| mouseover()  | 鼠标进入                                     |
| mouseout()   | 鼠标离开                                     |
| mouseenter() | 鼠标进入                                     |
| mouseleave() | 鼠标离开                                     |
| hover()      | 同时为mouseenter和mouseleave事件指定处理函数 |
| ready()      | DOM加载完成                                  |
| submit()     | 用户递交表单                                 |
| $(this)      |                                              |

> ~~~javascript
> $('#btn1').click(function()
> {// 内部的 $(this)指的是原生对象})
> ~~~

## 事件冒泡

+ 事件冒泡

> ~~~javascript
> 事件会向它的父级标签一级一级传递
> ~~~

+ 事件代理

> ~~~javascript
> //事件代理其实是事件冒泡的一种应用
> 事件加到父级上，通过判断事件来源，执行相应的子元素的操作。
> 事件代理可以极大减少事件绑定次数，提高性能；其次可以让新加入的子元素也可以拥有相同的操作。
> ~~~

+ 事件代理实现

~~~javascript
 var $list = $("#list");
// $list.delegate(子标签选择器，子标签触发事件，事件处理函数）
$list.delegate("li", "click", function(){
    console.log($(this).html());
});
// 新的的列表项，自动绑定点击事件
$list.append("<li>6</li>");
~~~

## Jquery动画

+ **Jquery动画函数**

| 方法          | 功能                   |
| ------------- | ---------------------- |
| fadeIn()      | 淡入                   |
| fadeOut()     | 淡出                   |
| fadeToggle()  | 切换淡入淡出           |
| hide()        | 隐藏元素               |
| show()        | 显示元素               |
| toggle()      | 切换元素的可见状态     |
| slideDown()   | 向下展开               |
| slideUp()     | 向上卷起               |
| slideToggle() | 依次展开或卷起某个元素 |

~~~javascript
$('#div1').animate({width:300,height:300 },1000,'swing',function(){alert('done!');});
// 参数一：要改变的样式属性值，写成字典的形式
// 参数二：动画持续的时间，单位为毫秒，一般不写单位
// 参数三：动画曲线，默认为‘swing’，缓冲运动，还可以设置为‘linear’，匀速运动
// 参数四：动画回调函数，动画完成后执行的匿名函数
// 特殊效果
$btn.click(function(){$('#div1').fadeIn(1000,'swing',function(){alert('done!');});
~~~

## JavaScript对象

~~~ javascript
// 顶级Object类型:
var oPerson = new Object();
oPerson.name = "xiaoming";
oPerson.age = 16; 
oPerson.func = function(){console.log(this.name);};
alert(person.age); // 调用属性和方法：
person.func();
// 对象字面量:
var person2 = { name:'Rose', age: 18,
sayName:function(){alert('My name is' + this.name);}}
alert(person2.age); // 调用属性和方法：
person2.sayName();
~~~

## json

+ 对象格式

  > ~~~javascript
  > '{"name":"xiaoming", "age":20}'
  > ~~~

+ 数组格式

  > ~~~javascript
  > '[{"name":"xiaoming", "age":20},{"name":"xiaofang", "age":22}]'
  > ~~~

+ json字符串转换javascript对象

  ~~~javascript
  var oPerson = JSON.parse(sJson);
  oPerson.name
  ~~~

+ 总结

  > ~~~javascript
  > // json就是一个javascript对象表示法，json本质上是一个字符串。
  > //json有两种格式：1. 对象格式, 2. 数组格式
  > ~~~

## ajax

+ **ajax的作用**

  + ajax 最大的优点是实现**局部刷新**

  + ajax可以让 javascript 发送异步的 http 请求

+ **ajax的使用**

  > jquery将它封装成了一个方法$.ajax()，我们可以直接用这个方法来执行ajax请求。

+ **代码示例**

~~~javascript
<script>
    $.ajax({
    url:'http://t.weather.sojson.com/api/weather/city/101010100',
    type:'GET',
    dataType:'JSON',
    success:function (response) {
        console.log(response);    
    },
    error:function () {
        alert("请求失败,请稍后再试!");
    },
    async:true
});
</script>
~~~

| 参数     | 功能                                               |
| -------- | -------------------------------------------------- |
| url      | 请求地址                                           |
| type     | 请求方式，默认是'GET'，常用的还有'POST'            |
| dataType | 设置返回的数据格式，常用的是'json'格式             |
| data     | 设置发送给服务器的数据，没有参数不需要设置         |
| success  | 设置请求成功后的回调函数                           |
| error    | 设置请求失败后的回调函数                           |
| async    | 设置是否异步，默认值是'true'，表示异步，一般不用写 |

> 同步和异步说明
>
> - 同步是一个ajax请求完成另外一个才可以请求，需要等待上一个ajax请求完成，好比线程同步。
> - 异步是多个ajax同时请求，不需要等待其它ajax请求完成， 好比线程异步

+ **ajax的简写方式**

> $.ajax按照请求方式可以简写成$.get或者$.post方式

~~~javascript
<script>
    $(function(){
    	// get请求
        $.get("http://t.weather.sojson.com/api/weather/city/101010100", 				function(dat,status){console.log(dat);console.log(status);})
            .error(function(){alert("网络异常"); });

    	//post请求
        $.post("test.php", {"func": "getNameAndTime"}, 
        function(data){alert(data.name); console.log(data.time); },"json")
            .error(function(){alert("网络异常");}); 
    });
</script>
~~~

| 参数     | 功能                                                         |
| -------- | ------------------------------------------------------------ |
| url      | 请求地址                                                     |
| data     | 设置发送给服务器的数据, 没有参数不需要设置                   |
| success  | 设置请求成功后的回调函数                                     |
| dataType | 设置返回的数据格式，常用的是'json'格式, 默认智能判断数据格式 |
| error    | 错误异常回调函数                                             |


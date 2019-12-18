# Vue的基本使用

![Vue基础结构]( 配置图片\Vue基础结构.png)

+ **Vue的基本架构**

  > ~~~javascript
  > // 一个vue都会有   el绑定元素  data里面放数据  methods里面放函数
  > window.onload = function () {
  >         var vm = new Vue({
  >         el:'.box',
  >         data: {content: 'Vue的基本使用'},
  >         methods:{fnA:function(){alert("你好")}} });}
  > ~~~

# 修改属性

> ~~~html
> <a :href="url" target="_blank">{{linkdata}}</a>
> ~~~

~~~html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>操作数据</title>
    <script src="./js/vue.js"></script>
    <script>
        window.onload = function () {
            var vm = new Vue({
                el:'.box',
                data:{
                    content:'操作数据',
                    linkdata:'百度链接',
                    url:'http://www.baidu.com'
                }
            });
        }
    </script>
</head>
<body>
    <div class="box">
        <!-- 全写 -->
        <!-- <a v-bind:href="url" target="_blank">{{linkdata}}</a> -->
        <a :href="url" target="_blank">{{linkdata}}</a>
        <p>{{content}}</p>
    </div> 
</body>
</html>
~~~



# 调用方法

> ~~~html
> <button @click='fnAddClick'>计数器:{{count}}</button>
> ~~~

~~~html
<!DOCTYPE html>
<head>
    <meta charset="UTF-8">
    <title>操作数据</title>
    <script src="./js/vue.js"></script>
    <script>
        window.onload = function () {
            var vm = new Vue({
                el:'.box',
                data:{
                    content:'操作数据',
                    linkdata:'百度链接',
                    url:'http://www.baidu.com',
                    count:0
                },
                methods: {
                    fnAddClick:function () {
                       this.count += 1; 
                    }
                }

            });
        }
    </script>
</head>
<body>
    <div class="box">
        <!-- 全写 -->
        <!-- <button v-on:click='fnAddClick'>计数器:{{count}}</button> -->
        <button @click='fnAddClick'>计数器:{{count}}</button>
        <a v-bind:href="url" target="_blank">{{linkdata}}</a>
        <p>{{content}}</p>
    </div> 
</body>
</html>
~~~

# 条件渲染

> ~~~
> v-if
> v-else-if
> v-else
> v-show
> ~~~

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>条件渲染</title>

    <script src="js/vue.js"></script>
    <script>
        window.onload = function(){
            var vm = new Vue({
                el: ".div1",
                data:{
                    flag: 3, }})}
    </script>

</head>
<body>
    
    <div class="div1">
        <p v-if="flag==0"> 如果flag=0则执行这条</p>
        <p v-else-if="flag==1">如果flag=1则执行这条</p>
        <p v-else>其他情况则执行这条</p>

        <!-- v-show=条件 ：条件满足显示p段落,否则就隐藏p段落 display: none/block -->
        <p v-show="flag==4">如果flag=4则显示这条</p>
    </div>

</body>
</html>
~~~

# 列表渲染

~~~html
<script>
    window.onload = function () {
    var vm = new Vue({
        el:'.box',
        data:{
            itemList:[1, 2, 3, 4, 5],
            indexList:['a','b','c','d'],
            objData:{
                name:'小明',
                age:19
            },                      //对象
            objList:[
                {
                    name:'小明',	  //对象列表
                    age:20
                },
                {
                    name:'小红',
                    age:21
                }
            ]
        }
    });
}
</script>  
~~~

+ **列表**

~~~html
<li v-for='item in itemList'>{{item}}</li>
~~~

+ **列表下标**

~~~html
<li v-for='(item,index) in indexList'>角标{{index}}==数值{{item}}</li>
~~~

+ **一个对象**

~~~html
<li v-for='item in objData'>{{item}}</li>
<li v-for='(obj,key) in objData'>属性值{{obj}}-----属性名{{key}}</li>
~~~

+ **对象列表加下标**

~~~html
<li v-for='obj in objList'>属性值1:{{obj.name}}==属性值2:{{obj.age}}</li>
~~~

# 双向绑定数据

> ~~~
> V-model
> ~~~

~~~html
<!-- 01.单行文本框 -->
        <input type="text" v-model='content'>
        <p>{{content}}</p>

<!-- 02.多行文本框 -->
        <textarea v-model='content'></textarea>
        <p>{{content}}</p>
      
<!-- 03.单选框 -->
        <input type="radio" name="sex" value="男" v-model='content'>男
        <input type="radio" name="sex" value="女"  v-model='content'>女
        <p>{{content}}</p>
   
<!-- 04.多选框 -->
        <input type="checkbox" name="lk" value="吃饭" v-model='like'>吃饭
        <input type="checkbox" name="lk" value="睡觉" v-model='like'>睡觉
        <input type="checkbox" name="lk" value="打豆豆" v-model='like'>打豆豆
        <p>{{like}}</p>     
    
<!-- 05.下拉框 -->
        <select name="addr" v-model='address'>
            <option value="北京">北京</option>
            <option value="上海">上海</option>
            <option value="广州">广州</option>
            <option value="深圳">深圳</option>
        </select>
        <p>{{address}}</p>
~~~

# ES6语法介绍

> ~~~
> ES6是JavaScript语言的新版本，它也可以叫做ES2015，之前学习的JavaScript属于ES5，ES6在它的基础上增加了一些语法，ES6是未来JavaScript的趋势，而且vue组件开发中会使用很多的ES6的语法，所以掌握这些常用的ES6语法是必须的。
> ~~~

# 实例生命周期

- beforeCreate
  - vm对象实例化之前
- created
  - vm对象实例化之后
- beforeMount
  - vm将作用标签之前
- mounted
  - vm将作用标签之后
- beforeUpdate
  - 数据或者属性更新之前
- updated
  - 数据或者属性更新之后

![Vue实例对象生命周期]( 配置图片\Vue实例对象生命周期.png)

# 组件


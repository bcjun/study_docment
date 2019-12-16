### TypeScript 

####  TypeScript 基本使用

**安装**

> ~~~
> npm install -g typescript
> ~~~

**查看版本**

> ~~~
> tsc -v
> ~~~

**运行第一个程序**

> + 创建一个ts
>+ 用TypeScript语法写一个程序
> + 编译成js文件并执行
>
> ~~~typescript
> var message:string = "Hello World" 
> console.log(message)
> // 保存为test.ts文件
> // 控制台上执行 》tsc Test.ts《 进行编译成js文件
> // 然后node来执行该js代码 》node test.js《
> ~~~

### TypeScript数据结构

#### 数组

> **数组对象**
>
> ~~~tsx
> var arr_names:number[] = new Array(4)  
>  
> for(var i = 0; i<arr_names.length; i++) { 
>         arr_names[i] = i * 2 
>         console.log(arr_names[i]) 
> }
> ~~~
>
> ~~~tsx
> var sites:string[] = new Array("Google","Runoob","Taobao","Facebook") 
>  
> for(var i = 0;i<sites.length;i++) { 
>         console.log(sites[i]) 
> }
> ~~~
>
> **数组解构**
>
> ~~~tsx
> var arr:number[] = [12,13] 
> var[x,y] = arr // 将数组的两个元素赋值给变量 x 和 y
> console.log(x) 
> console.log(y)
> ~~~

**字符串**

> **字符串定义**
>
> ~~~
> var txt = new String("string");
> 或者更简单方式：
> var txt = "string";
> ~~~
>
> 



#### TypeScript 联合类型

> **基本语法**
>
> ~~~
> Type1|Type2|Type3 
> ~~~
>
> **实例**
>
> ~~~tsx
> var val:string|number 
> val = 12 
> console.log("数字为 "+ val) 
> val = "Runoob" 
> console.log("字符串为 " + val)
> /* 结果
> 数字为 12
> 字符串为 Runoob
> */
> 
> // 如赋值成其他类型的变量就会报错
> ~~~
>
> **联合类型数组**
>
> ~~~tsx
> var arr:number[]|string[]; 
> var i:number; 
> 
> arr = [1,2,4] 
> console.log("**数字数组**")  
>  
>  
> arr = ["Runoob","Google","Taobao"] 
> console.log("**字符串数字**") 
> ~~~

#### TypeScript 接口

> **基本语法**
>
> ~~~
> interface interface_name { 
> }
> ~~~
>
> **例子**
>
> ~~~tsx
> // 定义
> interface IPerson { 
>     firstName:string, 
>     lastName:string, 
>     sayHi: ()=>string 
> } 
> // 使用 
> var customer:IPerson = { 
>     firstName:"Tom",
>     lastName:"Hanks", 
>     sayHi: ():string =>{return "Hi there"} 
> } 
> ~~~
>
> **联合类型和接口**
>
> ~~~tsx
> interface RunOptions { 
>     program:string; 
>     commandline:string[]|string|(()=>string); 
> } 
>  
> // commandline 是字符串
> var options:RunOptions = {program:"test1",commandline:"Hello"}; 
> console.log(options.commandline)  
>  
> // commandline 是字符串数组
> options = {program:"test1",commandline:["Hello","World"]}; 
> console.log(options.commandline[0]); 
> console.log(options.commandline[1]);  
>  
> // commandline 是一个函数表达式
> options = {program:"test1",commandline:()=>{return "**Hello World**";}}; 
> ~~~
>
> **接口和数组**
>
> ~~~tsx
> interface namelist { 
>    [index:number]:string 
> } 
>  
> var list2:namelist = ["John",1,"Bran"] / 错误元素 1 不是 string 类型
> interface ages { 
>    [index:string]:number 
> } 
>  
> var agelist:ages; 
> agelist["John"] = 15   // 正确 
> agelist[2] = "nine"   // 错误
> ~~~
>
> 
>
> **接口继承**
>
> ~~~tsx
> // 单继承
> Child_interface_name extends super_interface_name
> // 多继承
> Child_interface_name extends super_interface1_name, super_interface2_name,…,super_interfaceN_name
> ~~~
>
> **单继承实例**
>
> ~~~tsx
> interface Person { 
>    age:number 
> } 
>  
> interface Musician extends Person { 
>    instrument:string 
> } 
>  
> var drummer = <Musician>{}; 
> /* 
> drummer.age = 27 
> drummer.instrument = "Drums" 
> console.log("年龄:  "+drummer.age)
> console.log("喜欢的乐器:  "+drummer.instrument)
> */
> ~~~

#### TypeScript 类

> **实例**
>
> ~~~tsx
> class Car { 
> // 字段
> engine:string; 
> 
> // 构造函数
> constructor(engine:string) { 
>    this.engine = engine 
> }  
> 
> // 方法
> disp():void { 
>    console.log("函数中显示发动机型号  :   "+this.engine) 
> } 
> } 
> 
> // 创建一个对象
> var obj = new Car("XXSY1")
> 
> // 访问字段
> console.log("读取发动机型号 :  "+obj.engine)  
> 
> // 访问方法
> obj.disp()
> 
> 
> /*  结果
> 读取发动机型号 :  XXSY1
> 函数中显示发动机型号  :   XXSY1
> */
> ~~~
>
> **类的继承**
>
> ~~~tsx
> class Shape { 
> Area:number 
> 
> constructor(a:number) { 
>    this.Area = a 
> } 
> } 
> 
> class Circle extends Shape { 
> disp():void { 
>    console.log("圆的面积:  "+this.Area) 
> } 
> }
> 
> var obj = new Circle(223); 
> obj.disp()
> // 结果
> // 圆的面积:  223
> ~~~
>
> **类的继承**
>
> ~~~tsx
> /*
> 需要注意的是子类只能继承一个父类，TypeScript 不支持继承多个类，但支持多重继承，如下实例：
> */
> 
> class Root { 
> str:string; 
> } 
> 
> class Child extends Root {} 
> class Leaf extends Child {} // 多重继承，继承了 Child 和 Root 类
> 
> var obj = new Leaf(); 
> obj.str ="hello" 
> console.log(obj.str)
> // 结果：hello
> ~~~
>
> **继承类的方法重写**
>
> ~~~tsx
> /*
> 类继承后，子类可以对父类的方法重新定义，这个过程称之为方法的重写。
> 其中 super 关键字是对父类的直接引用，该关键字可以引用父类的属性和方法。
> */
> 
> class PrinterClass { 
> doPrint():void {
>    console.log("父类的 doPrint() 方法。") 
> } 
> } 
> 
> class StringPrinter extends PrinterClass { 
> doPrint():void { 
>    super.doPrint() // 调用父类的函数
>    console.log("子类的 doPrint()方法。")
> } 
> }
> /*
> 父类的 doPrint() 方法。
> 子类的 doPrint()方法。
> */
> var obj = new StringPrinter();
> obj.doPrint();
> ~~~
>
> **属性**
>
> **instanceof 运算符**
>
> ~~~
> instanceof 运算符用于判断对象是否是指定的类型，如果是返回 true，否则返回 false。
> ~~~
>
> **static 关键字**
>
> ~~~
> static 关键字用于定义类的数据成员（属性和方法）为静态的，静态成员可以直接通过类名调用。
> 
> ~~~
>
> **访问控制修饰符**
>
> ~~~
> TypeScript 中，可以使用访问控制符来保护对类、变量、方法和构造方法的访问。TypeScript 支持 3 种不同的访问权限。
> 
> public（默认） : 公有，可以在任何地方被访问。
> 
> protected : 受保护，可以被其自身以及其子类和父类访问。
> 
> private : 私有，只能被其定义所在的类访问。
> ~~~
>
> **抽象类:abstract**
>
> ~~~ts
> //typescript中的抽象类是提供其它类的基类，不能直接被实例化；
> // 用abstract关键字定义的抽象方法和抽象类，不包括具体实现必须在派生类实现。
> // 并且抽象方法只能放在 抽象类里面
> // 抽象类用于定义标准
> abstract class Animal {
> 　　abstract eat():void;
> }
> ~~~

# Other

###  != 和!==

> ~~~
> 1、用法
> 
> 都是用来比较值的。
> 
> 2、比较过程
> 
> != 比较时，若类型不同，会偿试转换类型；
> 
> !== 只有相同类型才会比较。
> 
> 3、比较结果
> 
> !=返回同类型值比较结果 ；
> 
> !== 不同类型不比较，且无结果，同类型才比较；
> ~~~
>



# ES6

### let 与 const

> ~~~
> let 与 const是ES6中新的属性
> ~~~

**let 命令**

+ let 定义的变量在代码块中有用 var在全局中有用
+ let 只能声明一次 var 可以声明多次
+ let 不存在变量提升，var 会变量提升:

> **let 定义的变量在代码块中有用**
>
> ~~~js
> {
>   let a = 0;
>   var b = 1;
> }
> a  // ReferenceError: a is not defined
> b  // 1
> 
> 
> // for 循环计数器很适合用 let
> ~~~
>
> **let 只能声明一次var 可以声明多次**
>
> ~~~js
> let a = 1;
> let a = 2;
> var b = 3;
> var b = 4;
> a  // Identifier 'a' has already been declared
> b  // 4
> ~~~
>
> **let 不存在变量提升，var 会变量提升**
>
> ~~~js
> console.log(a);  //ReferenceError: a is not defined
> let a = "apple";
>  
> console.log(b);  //undefined
> var b = "banana";
> 
> /*变量 b 用 var 声明存在变量提升，所以当脚本开始运行的时候，b 已经存在了，但是还没有赋值，所以会输出 undefined。
> 变量 a 用 let 声明不存在变量提升，在声明变量 a 之前，a 不存在，所以会报错。*/
> ~~~
>
> 

**const 命令**

+ const 声明一个只读变量，声明之后不允许改变。意味着，一旦声明必须初始化，否则会报错

> ~~~js
> const PI = "3.1415926";
> PI  // 3.1415926
> const MY_AGE;  // SyntaxError: Missing initializer in const declaration  
> ~~~
>
> ### 注意要点
>
> const 如何做到变量在声明初始化之后不允许改变的？其实 const 其实保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动。此时，你可能已经想到，简单类型和复合类型保存值的方式是不同的。是的，对于简单类型（数值 number、字符串 string 、布尔值 boolean）,值就保存在变量指向的那个内存地址，因此 const 声明的简单类型变量等同于常量。而复杂类型（对象 object，数组 array，函数 function），变量指向的内存地址其实是保存了一个指向实际数据的指针，所以 const 只能保证指针是固定的，至于指针指向的数据结构变不变就无法控制了，所以使用 const 声明复杂类型对象时要慎重。

### ES6 **解构赋值**

> **数组模型的解构**
>
> ~~~
> 基本
> let [a, b, c] = [1, 2, 3];
> // a = 1
> // b = 2
> // c = 3
> 
> 可嵌套
> let [a, [[b], c]] = [1, [[2], 3]];
> // a = 1
> // b = 2
> // c = 3
> 
> 可忽略
> let [a, , b] = [1, 2, 3];
> // a = 1
> // b = 3
> 
> 不完全解构
> let [a = 1, b] = []; // a = 1, b = undefined
> 
> 剩余运算符
> let [a, ...b] = [1, 2, 3];
> //a = 1
> //b = [2, 3]
> 
> 字符串等
> 在数组的解构中，解构的目标若为可遍历对象，皆可进行解构赋值。可遍历对象即实现 Iterator 接口的数据。
> let [a, b, c, d, e] = 'hello';
> // a = 'h'
> // b = 'e'
> // c = 'l'
> // d = 'l'
> // e = 'o'
> 
> 
> 解构默认值
> let [a = 2] = [undefined]; // a = 2
> 
> 
> 当解构模式有匹配结果，且匹配结果是 undefined 时，会触发默认值作为返回结果。
> let [a = 3, b = a] = [];     // a = 3, b = 3
> let [a = 3, b = a] = [1];    // a = 1, b = 1
> let [a = 3, b = a] = [1, 2]; // a = 1, b = 2
> a 与 b 匹配结果为 undefined ，触发默认值：a = 3; b = a =3
> a 正常解构赋值，匹配结果：a = 1，b 匹配结果 undefined ，触发默认值：b = a =1
> a 与 b 正常解构赋值，匹配结果：a = 1，b = 2
> ~~~
>
> **对象模型的解构**
>
> ~~~
> 基本
> let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
> // foo = 'aaa'
> // bar = 'bbb'
>  
>  
> let { baz : foo } = { baz : 'ddd' };
> // foo = 'ddd'
> 
> 
> 可嵌套可忽略
> let obj = {p: ['hello', {y: 'world'}] };
> let {p: [x, { y }] } = obj;
> // x = 'hello'
> // y = 'world'
> let obj = {p: ['hello', {y: 'world'}] };
> let {p: [x, {  }] } = obj;
> // x = 'hello'
> 
> 
> 不完全解构
> let obj = {p: [{y: 'world'}] };
> let {p: [{ y }, x ] } = obj;
> // x = undefined
> // y = 'world'
> 
> 
> 剩余运算符
> let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40};
> // a = 10
> // b = 20
> // rest = {c: 30, d: 40}
> 
> 
> 解构默认值
> let {a = 10, b = 5} = {a: 3};
> // a = 3; b = 5;
> let {a: aa = 10, b: bb = 5} = {a: 3};
> // aa = 3; bb = 5;
> 
> ~~~
>
> 
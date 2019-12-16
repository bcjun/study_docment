## TypeScript 基本使用

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

**vscode配置自动编译**

 **1.第一步**

> ~~~
> tsc --inti 生成tsconfig.json  改 "outDir": "./js", 
> ~~~

  **2、第二步**

> ~~~ 
> 任务 - 运行任务 监视tsconfig.json
> ~~~

## TypeScript数据结构

>   typescript中为了使编写的代码更规范，更有利于维护，增加了类型校验，在typescript中主要给我们提供了以下数据类型

+ **布尔类型（boolean）**
+ **数字类型（number）**
+ **字符串类型(string)**
+  **数组类型（array）**

+  **元组类型（tuple）**
+ **枚举类型（enum）**
+ **任意类型（any）**
+  **null 和 undefined**
+  **void类型**
+  **never类型**

> **布尔类型**
>
> ~~~
> var flag:boolean=true;
> 
> // flag=123;  //错误
> 
> flag=false;  //正确
> 
> console.log(flag);
> ~~~
>
> **数字类型**
>
> ~~~
> var num:number=123;
> 
> num=456;
> 
> console.log(num);  /正确/
> 
> num='str';    //错误      
> ~~~
>
> **字符串类型(string)**
>
> ~~~
> var str:string='this is ts';
> 
> str='haha';  //正确
> 
> str=true;  //错误
> ~~~
>
> **数组类型（有两种定义方式）**
>
> ~~~ts
> // var arr=['1','2'];  //es5定义数组
> 
> // 1.第一种定义数组的方式
> 
> var arr:number[]=[11,22,33];
> 
> console.log(arr);
> 
> 
> //2.第二种定义数组的方式
> 
> var arr:Array<number>=[11,22,33];
> 
> console.log(arr)
> ~~~
>
> **元祖类型**
>
> ~~~ts
> 
> // 元组类型（tuple）  属于数组的一种
> 
> var arr:Array<number>=[11,22,33];
> 
> console.log(arr)
> 
> 
> //元祖类型
> let arr:[number,string]=[123,'this is ts'];
> 
> console.log(arr);
> ~~~
>
> **枚举类型**
>
> ~~~ts
> 枚举类型（enum）
>     随着计算机的不断普及，程序不仅只用于数值计算，还更广泛地用于处理非数值的数据。
>     例如：性别、月份、星期几、颜色、单位名、学历、职业等，都不是数值数据。  
>     在其它程序设计语言中，一般用一个数值来代表某一状态，这种处理方法不直观，易读性差。
>     如果能在程序中用自然语言中有相应含义的单词来代表某一状态，则程序就很容易阅读和理解。
>     也就是说，事先考虑到某一变量可能取的值，尽量用自然语言中含义清楚的单词来表示它的每一个值，
>     这种方法称为枚举方法，用这种方法定义的类型称枚举类型。
> 
> enum Err {'undefined'=-1,'null'=-2,'success'=1};
> 
> var e:Err=Err.success;
> 
> console.log(e);
>     
>     
>  
> 
> enum Color {blue,red,'orange'};
> 
> 
> var c:Color=Color.red;
> 
> console.log(c);   //1  如果标识符没有赋值 它的值就是下标
> ~~~
>
>  **任意类型（any）**
>
> ~~~ts
> // var num:any=123;
> 
> // num='str';
> 
> // num=true;
> 
> // console.log(num)
> 
> 
> 
> //任意类型的用处
> 
> 
> var oBox:any=document.getElementById('box');
> 
> 
> oBox.style.color='red';
> ~~~
>
> **null 和 undefined**
>
> ~~~ts
> 
> // var num:number;
> 
> // console.log(num)  //输出：undefined   报错
> 
> 
> // var num:undefined;
> 
> // console.log(num)  //输出：undefined  //正确
> 
> 
> 
> //定义没有赋值就是undefined
> // var num:number | undefined;
> 
> // console.log(num);
> 
> // var num:null;
> 
> // num=null;
> 
> 
> 
> //一个元素可能是 number类型 可能是null 可能是undefined
> 
> 
> var num:number | null | undefined;
> 
> 
> num=1234;
> 
> console.log(num)
> ~~~
>
> **void类型**
>
> ~~~ts
> // typescript中的void表示没有任何类型，一般用于定义方法的时候方法没有返回值。
> function run():void{
>     console.log('run')}
> run();
> ~~~
>
> **never类型**
>
> ~~~ts
> // 是其他类型 （包括 null 和 undefined）的子类型，代表从不会出现的值。这意味着声明never的变量只能被never类型所赋值。
> 
> var a:never;
> a=(()=>{
> throw new Error('错误');
> })()
> ~~~

#### 泛型

~~~
泛型：软件工程中，我们不仅要创建一致的定义良好的API，同时也要考虑可重用性。 组件不仅能够支持当前的数据类型，同时也能支持未来的数据类型，这在创建大型系统时为你提供了十分灵活的功能。

在像C#和Java这样的语言中，可以使用泛型来创建可重用的组件，一个组件可以支持多种类型的数据。 这样用户就可以以自己的数据类型来使用组件。

通俗理解：泛型就是解决 类 接口 方法的复用性、以及对不特定数据类型的支持(类型校验)
~~~

> **引入**
>
> ~~~ts
> //同时返回 string类型 和number类型  （代码冗余）
> function getData1(value:string):string{
>     return value;
> }
> 
> function getData2(value:number):number{
>      return value;
> }
> 
> 
> // 如果需要解决上面的问题，any也可以解决这个问题（但是也意味着放弃了类型检查）
> function getData(value:any):any{
>      return '哈哈哈';
> }
> getData(123);
> getData('str');
> ~~~
>
> **泛型**
>
> ~~~tsx
> // 泛型：可以支持不特定的数据类型   要求：传入的参数和返回的参数一致
> 
> T表示泛型，具体什么类型是调用这个方法的时候决定的
> function getData<T>(value:T):T{
>         return value;
>     }
> getData<number>(123);
> getData<string>('1214231');
> getData<number>('2112');       /*错误的写法*/  
> ~~~
>
> **泛型类**
>
> ~~~ts
> // 泛型类：比如有个最小堆算法，需要同时支持返回数字和字符串 a  -  z两种类型。  通过类的泛型来实现
> //类的泛型
> 
> class MinClas<T>{
> 
>     public list:T[]=[];
> 
>     add(value:T):void{
> 
>         this.list.push(value);
>     }
> 
>     min():T{        
>         var minNum=this.list[0];
>         for(var i=0;i<this.list.length;i++){
>             if(minNum>this.list[i]){
>                 minNum=this.list[i];
>             }
>         }
>         return minNum;
>     }
> }
> 
> var m1=new MinClas<number>();   /*实例化类 并且制定了类的T代表的类型是number*/
> m1.add(11);
> m1.add(3);
> m1.add(2);
> alert(m1.min())
> 
> var m2=new MinClas<string>();   /*实例化类 并且制定了类的T代表的类型是string*/
> 
> m2.add('c');
> m2.add('a');
> m2.add('v');
> alert(m2.min())
> 
> ~~~
>
> **泛类型接口**
>
> ~~~ts
> //  ---------------------------例一--------------------------
> interface ConfigFn<T>{
> (value:T):T;
> }
> 
> 
> function getData<T>(value:T):T{
> 
> return value;
> }
> 
> 
> var myGetData:ConfigFn<string>=getData;     
> 
> 
> myGetData('20');  /*正确*/
> 
> 
> // myGetData(20)  //错误
> // ---------------------------列二----------------------------
> interface ConfigFn{
> 
> <T>(value:T):T;
> }
> var getData:ConfigFn=function<T>(value:T):T{
> return value;
>  }
> 
> getData<string>('张三');
> getData<string>(1243);  //错误
> ~~~
>
> 





## typeScript中的函数

+ **函数的定义**

+ **必传参数**
+ **可选参数**

+ **默认参数**

+ **剩余参数**

+ **函数重载**

+ **箭头函数 es6**

> **函数的定义**
>
> ~~~ts
> //函数声明法
> function run():string{
> 
> return 'run';
> }
> 
> 
> //错误写法
> function run():string{
>      return 123;
> }
> ~~~
>
> ~~~ts
> //匿名函数
> 
> var fun2=function():number{
> return 123;
> }
> alert(fun2()); /*调用方法*/
> ~~~
>
> **必需参数**
>
> ~~~ts
> function getInfo(name:string,age:number):string{
> return `${name} --- ${age}`;
>  }
> alert(getInfo('zhangsan',20));
> ~~~
>
> **可选参数**
>
> ~~~ts
> // 可选参数后面加？即可
> function getInfo(name:string,age?:number):string{
> if(age){
> return `${name} --- ${age}`;
> }else{
> return `${name} ---年龄保密`;
> }
> }
> alert(getInfo('zhangsan'))
> alert(getInfo('zhangsan',123))
> ~~~
>
> **默认参数**
>
> ~~~ts
> function getInfo(name:string,age:number=20):string{
> if(age){
> return `${name} --- ${age}`;
> }else{
> return `${name} ---年龄保密`;}}
> 
> alert( getInfo('张三'));
> alert( getInfo('张三',30));
> ~~~
>
> **剩余参数**
>
> ~~~ts
> // 如果有其他的位置参数，需要放在剩余参数前面
> function sum(...result:number[]):number{             
>     var sum=0;
>     for(var i=0;i<result.length;i++){
>         sum+=result[i];  
>     }
>     return sum;
> }
> alert(sum(1,2,3,4,5,6)) ;
> ~~~
>
> **函数重载**
>
> ~~~ts
> // java中方法的重载：重载指的是两个或者两个以上同名函数，但它们的参数不一样，这时会出现函数重载的情况。
> // typescript中的重载：通过为同一个函数提供多个函数类型定义来试下多种功能的目的。
> //ts为了兼容es5 以及 es6 重载的写法和java中有区别。
> function getInfo(name:string):string;
> function getInfo(name:string,age:number):string;
> function getInfo(name:any,age?:any):any{
> if(age){
> 
> return '我叫：'+name+'我的年龄是'+age;
> }else{
> 
> return '我叫：'+name;
> }
> }
> 
> ~~~
>
> **箭头函数**
>
> ~~~ts
> //this指向的问题    箭头函数里面的this指向上下文         
> // setTimeout(function(){
> //     alert('run')
> // },1000)
> 
> 
> //等同上面的函数
> setTimeout(()=>{
> 
> alert('run')
> },1000)
> ~~~

## TypeScript 类

+ **类的定义**

+ **继承**

+ **类里面的修饰符**

+ **静态属性 静态方法**

+ **抽象类 继承 多态**

> **类的定义**
>
> ~~~tsx
> class Car { 
> // 字段
> engine:string; 
> // 构造函数
> constructor(engine:string) { 
> this.engine = engine 
> }  
> // 方法
> disp():void { 
> console.log("函数中显示发动机型号  :   "+this.engine) 
> } 
> } 
> 
> 
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
> // ---------------------基本类------------------------
> 
> class Shape { 
> Area:number 
> 
> constructor(a:number) { 
> this.Area = a 
> } 
> } 
> 
> class Circle extends Shape { 
> disp():void { 
> console.log("圆的面积:  "+this.Area) 
> } 
> }
> 
> var obj = new Circle(223); 
> obj.disp()
> // 结果
> // 圆的面积:  223
> ~~~
>
> ~~~tsx
> // ---------------------类的多重继承------------------------
> 
> 
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
> ~~~ts
> // ---------------------类方法的重写------------------------
> 
> /*
> 类继承后，子类可以对父类的方法重新定义，这个过程称之为方法的重写。
> 其中 super 关键字是对父类的直接引用，该关键字可以引用父类的属性和方法。
> */
> 
> class PrinterClass { 
> doPrint():void {
> console.log("父类的 doPrint() 方法。") 
> } 
> } 
> 
> class StringPrinter extends PrinterClass { 
> doPrint():void { 
> super.doPrint() // 调用父类的函数
> console.log("子类的 doPrint()方法。")
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
> 
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
> **静态方法静态属性**
>
> ~~~ts
> // ----------------------静态方法----------------------
> function Person(){
> this.run1=function(){
> }}
> 
> Person.name='哈哈哈';
> Person.run2=function(){  静态方法}
> var p=new Person();
> Person.run2(); 静态方法的调用
> 
> 
> 
> // ----------------------静态属性----------------------
> 
> class Per{
>     public name:string;
>     public age:number=20;
>     
>     //静态属性
>     static sex="男";
>     constructor(name:string) {
>         this.name=name;
>     }
>     run(){  /*实例方法*/
> 
>         alert(`${this.name}在运动`)
>     }
>     work(){
> 
>         alert(`${this.name}在工作`)
>     }
>     static print(){  /*静态方法  里面没法直接调用类里面的属性*/
> 
>         alert('print方法'+Per.sex);
>     }
> }
> 
> Per.print();
> 
> alert(Per.sex);
> ~~~
>
> **多态**
>
> ~~~ts
> // 父类定义一个方法不去实现，让继承它的子类去实现  每一个子类有不同的表现 
> //多态属于继承
> class Animal {
>     name:string;
>     constructor(name:string) {
>         this.name=name;
>     }
>     eat(){   //具体吃什么  不知道   ，  具体吃什么?继承它的子类去实现 ，每一个子类的表现不一样
>         console.log('吃的方法')
>     }
> }
> 
> class Dog extends Animal{
>     constructor(name:string){
>         super(name)
>     }
>     eat(){
>         return this.name+'吃粮食'
>     }
> }
> 
> class Cat extends Animal{
>     constructor(name:string){
>         super(name)
>     }
>     eat(){
> 
>         return this.name+'吃老鼠'
>     }
> }
> 
> ~~~
>
> **抽象类:abstract**
>
> ~~~ts
> //typescript中的抽象类是提供其它类的基类，不能直接被实例化；
> // 用abstract关键字定义的抽象方法和抽象类，不包括具体实现必须在派生类实现。
> // 并且抽象方法只能放在 抽象类里面
> // 抽象类用于定义标准(即抽象类实现的方法，子类也必须实现)
> abstract class Animal {
> 　　abstract eat():void;
> }
> ~~~





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

## typescript接口

> 接口的作用：在面向对象的编程中，接口是一种规范的定义，它定义了行为和动作的规范，在程序设计里面，接口起到一种限制和规范的作用。接口定义了某一批类所需要遵守的规范，接口不关心这些类的内部状态数据，也不关心这些类里方法的实现细节，它只规定这批类里必须提供某些方法，提供这些方法的类就可以满足实际需要。 typescrip中的接口类似于java，同时还增加了更灵活的接口类型，包括属性、函数、可索引和类等。

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
>  firstName:string, 
>  lastName:string, 
>  sayHi: ()=>string 
> } 
> // 使用 
> var customer:IPerson = { 
>  firstName:"Tom",
>  lastName:"Hanks", 
>  sayHi: ():string =>{return "Hi there"} 
> } 
> ~~~
>
> **联合类型和接口**
>
> ~~~tsx
> interface RunOptions { 
>  program:string; 
>  commandline:string[]|string|(()=>string); 
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
> [index:number]:string 
> } 
> 
> var list2:namelist = ["John",1,"Bran"] / 错误元素 1 不是 string 类型
> interface ages { 
> [index:string]:number 
> } 
> 
> var agelist:ages; 
> agelist["John"] = 15   // 正确 
> agelist[2] = "nine"   // 错误
> ~~~
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
> age:number 
> } 
> 
> interface Musician extends Person { 
> instrument:string 
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

# 









##### 数组

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
> ~~~ts
> var txt = new String("string");
> 或者更简单方式：
> var txt = "string";
> ~~~
>
> 



#### TypeScript 联合类型

> **基本语法**
>
> ~~~ts
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

> Other
>

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
# Python基础

+ **执行方式**

  + 脚本

    > python xx.py

  + IDE

+ **Python**

  > 动态、强类型、解释型语言

  ~~~python
  动态：数据类型在执行时确定
  静态：数据类型在执行前确定
  
  强类型： 不同类型的不能做计算（Python）
  弱类型： 不同类型可以做计算（JavaScript）
  
  解释型语言: 每次执行代码，需要重新用解释器执行一遍。
  编译型语言: 执行前可以通过编译器生成可执行程序，执行时运行可执行程序。
  ~~~

+ **注释**

  ~~~python
  #单行注释（行注释）
  
  """
  多行注释（块注释） 调用函数的时候能够看到注释 
  """
  ~~~

+ **算数运算符**

| 符号 | 含义 | 例子          |
| ---- | ---- | ------------- |
| +    | 加   | 10 + 20 = 30  |
| -    | 减   | 10 - 20 = -10 |
| *    | 乘   | 10 * 20 = 200 |
| /    | 除   | 10 / 20 = 0.5 |
| //   | 取整 | 9 // 2 = 4    |
| %    | 取余 | 9 % 2 = 1     |
| **   | 幂   | 2 ** 3 = 8    |

+ 变量类型

| 数字类型       | 含义     |
| -------------- | -------- |
| int            | 整数型   |
| float          | 浮点型   |
| bool           | 布尔型   |
| complex        | 复数型   |
| **非数字类型** | **含义** |
| str            | 字符串   |
| list           | 列表     |
| tuple          | 元祖     |
| dict           | 字典     |
| set            | 集合     |

> ~~~python
> #在 Python 中，所有 非数字型变量 都支持以下特点：
> """
> 1.都是一个 序列，也可以理解为 容器
> 2.取值 []
> 3.遍历 for in
> 4.计算长度、最大/最小值、比较、删除
> 5.链接 + 和 重复 *
> 6.切片
> """
> ~~~
>



+ **变量命名**

  +  标示符可以由 字母、下划线 和 数字 组成

  + 不能以数字开头

  + 不能与关键字重名

    > ```python
    > #查看关键字
    > import keyword
    > print(keyword.kwlist)
    > ```

  + 尽量采用大驼峰小驼峰命名法

+ **格式符号**

  > 下面是完整的，它可以与％符号使用列表:

  | 格式符号 |            转换            |
  | :------: | :------------------------: |
  |    %c    |            字符            |
  |    %s    |           字符串           |
  |    %d    |      有符号十进制整数      |
  |    %u    |      无符号十进制整数      |
  |    %o    |         八进制整数         |
  |    %x    | 十六进制整数（小写字母0x） |
  |    %X    | 十六进制整数（大写字母0X） |
  |    %f    |           浮点数           |
  |    %e    |   科学计数法（小写'e'）    |
  |    %E    |   科学计数法（大写“E”）    |
  |    %g    |      ％f和％e 的简写       |
  |    %G    |       ％f和％E的简写       |

+ **常用的数据类型转换**

  |          函数          |                        说明                         |
  | :--------------------: | :-------------------------------------------------: |
  |    int(x [,base ])     |                  将x转换为一个整数                  |
  |       float(x )        |                 将x转换为一个浮点数                 |
  | complex(real [,imag ]) |        创建一个复数，real为实部，imag为虚部         |
  |        str(x )         |                将对象 x 转换为字符串                |
  |        repr(x )        |             将对象 x 转换为表达式字符串             |
  |       eval(str )       | 用来计算在字符串中的有效Python表达式,并返回一个对象 |
  |       tuple(s )        |               将序列 s 转换为一个元组               |
  |        list(s )        |               将序列 s 转换为一个列表               |
  |        chr(x )         |           将一个整数转换为一个Unicode字符           |
  |        ord(x )         |           将一个字符转换为它的ASCII整数值           |
  |        hex(x )         |         将一个整数转换为一个十六进制字符串          |
  |        oct(x )         |          将一个整数转换为一个八进制字符串           |
  |        bin(x )         |          将一个整数转换为一个二进制字符串           |

### 循环判断

**判断**



+ **比较运算符**

  | 运算符 | 描述                                                         | 示例                           |
  | :----- | :----------------------------------------------------------- | :----------------------------- |
  | ==     | 检查两个操作数的值是否相等，如果是则条件变为真。             | 如a=3,b=3，则（a == b) 为 True |
  | is     | 不仅会比较两个值是否相等,还会比较内存地址是否相等            |                                |
  | !=     | 检查两个操作数的值是否相等，如果值不相等，则条件变为真。     | 如a=1,b=3，则(a != b) 为 True  |
  | >      | 检查左操作数的值是否大于右操作数的值，如果是，则条件成立。   | 如a=7,b=3，则(a > b) 为 True   |
  | <      | 检查左操作数的值是否小于右操作数的值，如果是，则条件成立。   | 如a=7,b=3，则(a < b) 为 False  |
  | >=     | 检查左操作数的值是否大于或等于右操作数的值，如果是，则条件成立。 | 如a=3,b=3，则(a >= b) 为 True  |
  | <=     | 检查左操作数的值是否小于或等于右操作数的值，如果是，则条件成立。 | 如a=3,b=3，则(a <= b) 为 True  |

+ **逻辑运算符**

  | 运算符 | 逻辑表达式 | 描述                                                         | 实例                                     |
  | :----- | ---------- | :----------------------------------------------------------- | :--------------------------------------- |
  | and    | x and y    | 布尔"与"：如果 x 为 False，x and y 返回 False，否则它返回 y 的值。 | True and False， 返回 False。            |
  | or     | x or y     | 布尔"或"：如果 x 是 True，它返回 True，否则它返回 y 的值。   | False or True， 返回 True。              |
  | not    | not x      | 布尔"非"：如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not True 返回 False, not False 返回 True |

  > 0以外数值都是为真

+ **if判断语句**

  > ~~~python
  > if score>=90 and score<=100:
  >     print('本次考试，等级为A')
  > elif score>=80 and score<90:
  >     print('本次考试，等级为B')
  > elif score>=70 and score<80:
  >     print('本次考试，等级为C')
  > elif score>=60 and score<70:
  >     print('本次考试，等级为D')
  > elif score>=0 and score<60:
  >     print('本次考试，等级为E')
  > # elif必须和if一起使用，否则出错
  > # se 一般用在最后，即所有条件都不满足时使用
  > ~~~

+ **三目运算**

  > ~~~python
  > # 为真时的结果 if 判断条件 else 为假时的结果（注意，没有冒号）
  > max = a if a > b else b# 使用三目运算符求较大值
  > # Python 中的三目运算符目的是得到一个结果，未必就是将该结果return，或者进行简单的变量赋值
  > ~~~

**循环**

+ **while循环**

  ~~~python
  i = 0
      while i < 5:
          print("i=%d" % i)
          i+=1
  ~~~

+ **for循环**

  ~~~python
  #循环五次
  for i in range(5):
      print(i)
  #遍历
  for x in 'itheima':
      print(x)
  ~~~

+ **break和continue**

  ~~~python
  break/continue只能用在循环中，除此以外不能单独使用
  break/continue在嵌套循环中，只对最近的一层循环起作用
  ~~~

### Python数据结构

### 字符串

### Python内置函数

**getattr**

![](D:\常用资料\笔记\2.7Typora_note\配置图片\Python内置函数getattr.png)

**hastattr**

![Python内置函数hasattr](D:\常用资料\笔记\2.7Typora_note\配置图片\Python内置函数hasattr.png)

**setattr**

![Python内置函数setattr](D:\常用资料\笔记\2.7Typora_note\配置图片\Python内置函数setattr.png)

**all**

> ~~~python
> # 如果列表里面的数据都存在则返回True,如果有不存在的则返回False
> if all([mobile, password, sms_code, openid_sign]) is False:
>             return http.HttpResponseForbidden('缺少必传参数')
> ~~~

### 数据的相互转换

**元组转字典**

> **字典转元组**
>
> ~~~python
> a = {"age":25,}
> b = list(a.items())
> # 结果输出：[('age', 25)]
> ~~~
>
> **元组转字典**
>
> ~~~python
> a = ("age",20)
> b = dict([a])
> print(b)
> # 输出结果:{'age': 20}
> ~~~

**列表转字典**

> 列表转字典
>
> ~~~python
> a = [1,2,3]
> b = [4,5,6]
> c = dict(zip(a,b))
> print(c)
> ~~~
>
> 列表转元祖
>
> ~~~
> lista = [1,2,3,4,5,6,7,8,9]
> listb = ["q","w","e","r","t","y","u","i","o"]
> listc = [1,2,3,4,5,6,7,8,9]
> data1 =tuple(zip(listb,lista,listc))
> #(('q', 1, 1), ('w', 2, 2), ('e', 3, 3), ('r', 4, 4), ('t', 5, 5), ('y', 6, 6), ('u', 7, 7), ('i', 8, 8), ('o', 9, 9))
> 
> ~~~

**字典按照值排序**

> **第一种方案(多个字典)**
>
> ~~~python
> import operator
> # 必须是一个列表，多个字典才能排序
> list_of_dicts=[{"age":28,"aa":8},{"age":2}]
> list_of_dicts.sort(key=operator.itemgetter('age'))
> # 结果：[{'age': 2}, {'age': 28, 'aa': 8}]
> ~~~
>
> **第二种方案(多个字典)**
>
> ~~~python
> f = [{'name': 'abc', 'age': 50}, {'name': 'def', 'age': 30}, {'name': 'ghi', 'age': 25}]  # 列表中的元素为字典
> 
> f2 = sorted(f,key = lambda x:x['age'])
> print(f2)
> # 结果:[{'name': 'ghi', 'age': 25}, {'name': 'def', 'age': 30}, {'name': 'abc', 'age': 50}]
> ~~~
>
> **第三种方案(大字典)**
>
> ~~~python
> d = {'d1':2, 'd2':4, 'd4':1,'d3':3,}
> res = sorted(d.items(),key=lambda d:d[1],reverse=True)
> print(res)
> # [('d2', 4), ('d3', 3), ('d1', 2), ('d4', 1)]
> ############################################也可以按找键排序
> d = {'d1':2, 'd2':4, 'd4':1,'d3':3,}
> res = sorted(d.items(),key=lambda d:d[0],reverse=True)
> print(res)
> # 结果: [('d4', 1), ('d3', 3), ('d2', 4), ('d1', 2)]
> ~~~
>
> PS:
>
> ~~~python
> [('d4', 1), ('d3', 3), ('d2', 4), ('d1', 2)]# 这种事有序字典
> # 可以直接强转为字典
> dict(res)
> ~~~



# Python高级

 property

- 作用:
  - 像访问属性一样访问函数
  - 方便控制数据的有效性，
  - 精细化控制访问权限
  - 隐藏实现细节
- 种类
  - 1.类实现方式
  - 2.装饰器实现方式

### property类实现方式

- ​	尽量使用私有get/set方法,不然就可以通过类里面的函数去修改属性了.

```python
"""	
提供了精准权限访问,可以设置哪些用户可以访问数据或者修改数据
还可以对用户需要修改的数据进行过滤
"""

class Account(object):
    def __init__(self, name, money):
        self.__name = name    # 帐户人姓名
        self.__balance = money    # 帐户余额

    def __get_name(self):
        return self.__name	

    def __set_balance(self,money):
        if isinstance(money, int):
            if money >= 0:
                self.__balance = money
            else:
                raise ValueError('输入的金额不正确')
        else:
            raise ValueError('输入的金额不是数字')

    def __get_balance(self):
        return self.__balance
    # 使用 property 类来为属性设置便利的访问方式
    name = property(__get_name)
    balance = property(__get_balance, __set_balance)


ac = Account('tom', 10)
print(ac.name)
print(ac.balance)
ac.balance = 1000
print(ac.balance)


```



### property装饰器实现方式

- @property 只提供读取数据的功能
- @xxx.setter 可以提供修改数据的功能

```python
class Account(object):
    def __init__(self, name, money):
        self.__name = name    # 帐户人姓名
        self.__balance = money    # 帐户余额

    # property 只能对获取方法装饰，并且获取方法不需要再写 get
    @property
    def name(self):
        return self.__name

    # 如果 property 装饰的属性还有 set 方法，需要写到 get方法后定义
    @property
    def balance(self):
        return self.__balance

    # 实现 set 方法， 格式： @xxx.setter ,xxx 要和property装饰的方法名一致
    @balance.setter
    def balance(self, money):
        if isinstance(money, int):
            if money >= 0:
                self.__balance = money
            else:
                raise ValueError('输入的金额不正确')
        else:
            raise ValueError('输入的金额不是数字')
ac = Account('tom', 10)
print(ac.name)
print(ac.balance)
ac.balance = 1000
print(ac.balance)
```

### 上下文管理器

- with语句的使用

  > 可以简化代码, 无论with语句中代码执行成功还是失败, 都会关闭文件; 无需使用try进行处理

- 上下文管理器类实现:

  - 必须实现两个方法

  > ```python
  > # 1. enter__`: 上文方法, 做准备工作, 返回操作对象
  > 
  > # 2. `__exit__`: 下文方法, 做收尾作用, 比如关闭文件, 关闭游标,数据库连接 
  > ```

- 上下文管理器装饰器

  > ```python
  > # @contextmanager装饰方法
  > # 注意: 使用装饰器实现的上下文管理器, 需要自己处理异常.
  > ```

**上下文类实现方式**

```python
class file(object):
    def __init__(self,name,mode):
        self.name = name
        self.mode = mode

    def __enter__(self):
        print("开启上文")
        self.obj_open = open(self.name,mode=self.mode)
        return self.obj_open

    def __exit__(self, e_type, e_val, e_tb):
        print("关闭下文")
        print(e_type, e_val, e_tb)
        self.obj_open.close()


with file("a","w")as f :
    f.write("唐海你好")
```

**上下文装饰器实现方式**

```python
# 装饰器实现方式
from contextlib import contextmanager
@contextmanager
def my_open(file_name, mode):
    data = open(file_name, mode)
    print("上文")
    yield data
    print("下文")
    data.close()

with my_open("aa","w")as f:
    print("正在努力的执行过程中")
    f.write("世界你好")
```

### 生成器

- 作用:
  - 按照程序员的规则生成数据，用一个生成一个
- 可以节省可多内存资源
- 

**列表生成器**

- 生成器推导式

```python
genrator = (i*2 for i in range(3))
value = next(genrator)
print(value)

value = next(genrator)
print(value)

value = next(genrator)
print(value)
```

- yield生成器

```python
def	genrator(num):
    for i in range(num):
        print("生成数据前")
        yield 1*2
        print("生成数据后")
        
gen = genrator(5)
value=next(gen)
print(value)
```

### 浅拷贝深拷贝

- 浅拷贝

> ```python
> copy.copy()
> ```

- 深拷贝

  > ```python
  > copy.deepcopy()
  > ```

- 浅拷贝和深拷贝的共同点:

  - 不可变类型,不管是深拷贝还是浅拷贝.直接赋值,并不会开辟新的储存空间

- 浅拷贝和深拷贝的不同点:

  - 浅拷贝: 对可变类型, 只会拷贝一层.
  - 深拷贝: 拷贝所有可变类型, 多层拷贝.

  

### 正则表达式

- re模块

|                 方法                 |            含义             |
| :----------------------------------: | :-------------------------: |
|      re.match(pattern, string)       | 匹配一次,且必须开头能匹配到 |
|      re.search(pattern, string)      |     匹配一次,全文本匹配     |
|       findall(pattern, string)       |     匹配多次,全文本匹配     |
| re.sub(pattern, repl, string, max=0) |     替换多次,全文本替换     |
|             参数pattern              |    可以增加正则的复用率     |

  

- 正则匹配规则

  |    单字符匹配    |                        含义                         |              |
  | :--------------: | :-------------------------------------------------: | :----------: |
  |        .         |              匹配任意1个字符（除了\n）              |              |
  |       [ ]        |                 匹配[ ]中列举的字符                 |              |
  |        \d        |                   匹配数字，即0-9                   | digit(数字)  |
  |        \D        |               匹配非数字，即不是数字                |              |
  |        \s        |          匹配空白，即 空格，tab键(\r\n\t)           | space(空格)  |
  |        \S        |                     匹配非空白                      |              |
  |        \w        |       匹配单词字符，即a-z、A-Z、0-9、_、汉字        |  word(单词)  |
  |        \W        |                   匹配非单词字符                    |              |
  |  **多字符匹配**  |                      **含义**                       |              |
  |        *         |     匹配前一个字符出现0次或者无限次，即可有可无     |              |
  |        +         |    匹配前一个字符出现1次或者无限次，即至少有1次     |              |
  |        ?         | 匹配前一个字符出现1次或者0次，即要么有1次，要么没有 |              |
  |       {m}        |              {m} 匹配前一个字符出现m次              |              |
  |      {m,n}       |         {m,n}   匹配前一个字符出现从m到n次          |              |
  |       {,n}       |           匹配前面的字符出现了小于等于n次           |              |
  |       {m,}       |           匹配前面的字符出现了大于等于m次           |              |
  | **匹配开头结尾** |                      **含义**                       |              |
  |        ^         |                   匹配字符串开头                    | [^a]表示取反 |
  |        $         |                   匹配字符串结尾                    |              |
  |   **匹配分组**   |                      **含义**                       |              |
  |        \|        |               匹配左右任意一个表达式                |              |
  |       (ab)       |              将括号中字符作为一个分组               |              |
  |       \num       |              引用分组num匹配到的字符串              |              |
  |    (?P<name>)    |                     分组起别名                      |              |
  |    (?P=name)     |          引用别名为name分组匹配到的字符串           |              |

- 贪婪:

  - 尽可能多的匹配字符

    > "*","?","+","{m,n}" 这些多字符匹配默认就是贪婪的.

- 非贪婪:

  - 尽可能少的匹配字符

    > 在"*","?","+","{m,n}"后面加上？，使贪婪变成非贪婪。

**例子**

- 分组

  > ```python
  > match_obj = re.match('^qq:(\d{6,11})$', 'qq:105676')
  > print(match_obj.group(1))
  > ```

- \num

  > ```python
  > # \\1 表示引用第一个分组.这里反斜杠中有一个反斜杠是表示转义的
  > match_obj = re.match('<([A-Za-z1-6]+)>.*</\\1>', '<html>hh</html>')
  > print(match_obj.group())
  > 
  > # \\1 表示第一个分组 \\2 表示第二个分组
  > # \\num 所代表的匹配规则,跟前面的分组里是对应的.
  > match_obj = re.match('<([A-Za-z1-6]+)><([A-Za-z1-6]+)>.*</\\2></\\1>', '<html><h1>www.itcast.cn</h1></html>')
  > print(match_obj.group())
  > ```

- 起别名(?P<name>)  (?P=name)

  > ```python
  > # 给分组起别名: (?P<name>)
  > # 使用别名引用分组: (?P=name)
  > match_obj = re.match('<(?P<parent>[A-Za-z1-6]+)><(?P<child>[A-Za-z1-6]+)>.*</(?P=child)></(?P=parent)>', '<html><h1>www.itcast.cn</h1></html>')
  > print(match_obj.group())
  > 
  > ```

**常用正则表达式**

| 正则                                          | 作用      |
| --------------------------------------------- | --------- |
| ^[\u4e00-\u9fa5]{0,}$                         | 匹配汉字  |
| ^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$ | Email地址 |

### 装饰器

- **闭包**

  - 定义:

    > 1.必须要有函数嵌套
    >
    > 2.内部函数使用外部函数的变量(参数)
    >
    > 3.外部函数返回内部函数的引用

  - 实现

    > ```
    > def func_out():
    > 	num = 10
    > 	def func_inner(num2):
    > 		rs = num + num2
    > 		print(rs)
    > 	return func_inner
    > ```

- **通用装饰器**

  > ```python
  > # 可以装饰任意参数, 任意返回值的函数
  > def log(func):
  >  #写在这里的代码,在未执行被装饰函数前,执行
  >  def inner(*args, **kwargs):
  >      # 这里可以放被装饰前执行的代码
  >      rs = func(*args, **kwargs)
  >      # 这里可以放被装饰后执行的代码
  >      return rs
  >  return inner
  > ```

- **多重装饰器**

  > 多重装饰器：使用多个装饰器, 对同一个函数进行装饰

  ```python
  #先使用靠近被装饰器进行装饰 既先执行@make_p 再执行@make_div
  @make_div
  @make_p 
  def content():
   return '今天天气真好!'
  ```

  

- **带参装饰器**

  > ```python
  > # 如果装饰器需要传入参数, 就需要在装饰器外部, 再套一个函数, 专门用于接受参数
  > def log(flag):
  > def decorator(func):
  > def inner(num1, num2):
  >     if flag == '+':
  >         print('正在进行加法计算...')
  >     elif flag == '-':
  >         print('正在进行减法计算...')
  > 
  >     return func(num1, num2)
  > return inner
  > # 返回原装饰器的外部函数
  > return decorator
  > 
  > @log('+')
  > # 本质
  > # 1. decorator = log('+')
  > # 2. @decorator =>  sum_num = decorator(sum_num)
  > def sum_num(num1, num2):
  > rs = num1 + num2
  > print(rs)
  > 
  > @log('-')
  > def sub_num(num1, num2):
  > rs = num1 - num2
  > print(rs)
  > 
  > sum_num(20, 10)
  > sub_num(20, 10)
  > ```
  >
  > 

- **类装饰器**

  > ```python
  > class Check(object):
  >  def __init__(self, fn):
  >      # 初始化操作在此完成
  >      self.__fn = fn
  > 
  >  # 实现__call__方法，表示对象是一个可调用对象，可以像调用函数一样进行调用。
  >  def __call__(self, *args, **kwargs):
  >      # 添加装饰功能
  >      print("请先登陆...")
  >      self.__fn()
  > 
  > 
  > @Check
  > def comment():
  >  print("发表评论")
  > 
  > 
  > comment()
  > ```

- **注意**

  - 装饰器是一种特殊的闭包
  - 装饰器内部函数只能使用外部函数变量,不能修改,如果要修改需要用nonlocal 对变量进行申明

### 高级设计模式

**单例模式**

> 实现方式一
>
> ```
> class Simple(object):
>  _instance = None
>  def __new__(cls, *args, **kwargs):
>      if cls._instance is None:
>          cls._instance=super().__new__(cls)
>      return cls._instance
>  def __init__(self):
>      pass
> bb = Simple()
> ```
>
> 实现方式二
>
> ```
> # 方法一：使用装饰器实现单列
> def singleton(cls):
>  instances = {}
> 
>  def wrapper(*args, **kwargs):
>      if cls not in instances:
>        	# {"类名": 类的对象}
>          instances[cls] = cls(*args, **kwargs)
>          
>      # 下一次直接返回对象
>      return instances[cls]
> 
>  return wrapper
> 
> 
> @singleton
> class Foo(object):
>  pass
> 
> foo1 = Foo()
> foo2 = Foo()
> ```
>

# async

- 线程

  - GIL：

    > 全局解释器锁；解释器层面的锁。不是我们自己能决定的。

  - 线程锁

    > 因为线程资源共享的缘故，造成资源不太安全，用来保证数据安全的。自己上锁。

- 进程

  - 进程

    > 程序:一个软件
    >
    > 进程:一个正在运行的软件

  - 并发并行

    > 并发:指的是任务数多于cpu核数
    > 并行:当任务数小于或者等于cpu核数时

  - 异步同步

    > 异步:同时执行(步伐不一致,你走的快,你就走的步伐多)
    > 同步:同步就是协同步调，按预定的先后次序进行运行(步伐一致,你走一步,我走一步.)

  - 阻塞非阻塞

    > 阻塞:等待
    > 非阻塞:不等待

  

### 线程

- **实现方式一**

  ```python
  #threading是在thread上面的一个封装.
  import threading
  # target: 指向新开启的线程要执行的代码
  #线程执行任务并传参有两种方式:
      #元组方式传参(args) ：元组方式传参一定要和参数的顺序保持一致。
      #字典方式传参(kwargs)：字典方式传参字典中的key一定要和参数名保持一致。
  t1 = threading.Thread(target=function1,args=(param,))
  t1.start()  # 启动线程，既然线程开始执行
  ```

  

- **实现方式二**

  ```python
  # python的threading.Thread类有一个run方法，用于定义线程的功能函数可以在自己的线程类中覆盖该方法,
  #当调用start()后可以启动该线程
  
  import threading
  # 自定义类，继承threading.Thread
  class MyThread(threading.Thread):
      def run(self):
          code
  # 通过MyThread创建线程对象
  t1 = MyThread()
  t1.start()
  ```

- **实现方式三**

  ```python
  #线程池
  from processing.dummy import Pool
  pool = Pool(10)
  pool.map(self.function,list_param)
  ```

  

- **实现方式四**

  ```python
  from processing.dummy import Pool
  pool = Pool(10)
  #分别发送每个url地址的请求，并将所有响应保存在列表中传给parse_page
  pool.map_async(function,list_param,callback)
  # 关闭线程池，不再接受新的任务
  pool.close()
  # 让主线程阻塞
  pool.join()
  ```

  

- **线程锁**

  - 什么是线程锁

    > 多线程的创建与执行都是无序的
    >
    > 无法控制线程调度程序        
    >
    > 如果多个线程同时对同一个全局变量操作，会出现资源竞争问题，从而数据结果会不正确，即会遇到线程安全问题

  - 解决方案

    > ```python
    > #解决方案:当多个线程几乎同时修改某一个共享数据的时候，需要进行同步控制      
    > #索取锁
    > mutex = threading.Lock()
    > #锁定
    > mutex.acquire()
    > #释放
    > mutex.release()
    > ```

  

- **线程守护**

  > ```python
  > # 方式一
  > thread_obj = threading.Thread(target=show_info, daemon=True)
  > #方式二
  > thread_obj.setDaemon(True)
  > #因为线程不需要新开辟内存，所以当遇到子线程时会直接先去执行子线程里面的代码。 但是当子线程有阻塞时，又会跳出来去执行主线程的代码。
  > #对应的因为进程需要重新开辟资源,所以遇到子进程时,会先挂起,还是先执行子进程里面的程序
  > ```

  | 主线程 | 非守护线程 | 守护线程 | 状态                   |
  | ------ | ---------- | -------- | ---------------------- |
  | 完毕   | 完毕       | 未完毕   | 杀死守护线程           |
  | 完毕   | 未完毕     | 未完毕   | 等待非守护线程执行完毕 |
  | 未完毕 | 完毕       | 未完毕   | 等待主线程执行完毕     |

  | 总结                                                         |
  | ------------------------------------------------------------ |
  | [守护线程不是亲生的,所以不会单独等守护线程](https://blog.csdn.net/u013210620/article/details/78710532#1%E5%AE%88%E6%8A%A4%E5%AD%90%E8%BF%9B%E7%A8%8B%E9%9D%9E%E5%AE%88%E6%8A%A4%E5%AD%90%E8%BF%9B%E7%A8%8B%E5%B9%B6%E5%AD%98) |

  

### 进程

- **进程实现**

```python
import multiprocessing
#进程执行任务并传参有两种方式:
#元组方式传参(args): 元组方式传参一定要和参数的顺序保持一致。
#字典方式传参(kwargs): 字典方式传参字典中的key一定要和参数名保持一致。
process_obj = multiprocessing.Process(target=func,args=(param,))
process_obj.start()
注意：创建进程对象，创建一个启动后就是一个进程。
```

> #Process语法结构:
> 	target：如果传递了函数的引用，可以认为这个子进程就执行这里的代码
> 	args：给target指定的函数传递的参数，以元组的方式传递
> 	kwargs：给target指定的函数传递命名参数
> #Process实例对象常用属性
> 	start()：启动子进程实例（创建子进程）
> 	is_alive()：判断进程子进程是否还在活着
> 	join([timeout])：是否等待子进程执行结束，或等待多少秒
> 	terminate()：不管任务是否完成，立即终止子进程
> 	pid：当前进程的pid（进程号）
>
> #其他
>
> ​	os.getpid()  获取当前进程编号  
>
> ​	os.getppid()  获取父进程的编号

- **pool的使用**

```python
from multiprocessing import Pool

def worker(msg):
    time.sleep(random.randint(3,10))
    
if __name__ == '__main__':
    pool = Pool(3)  # 定义一个进程池，最大进程数3
    for i in range(0,10):
      
        pool.apply_async(worker,(i,)) # 每次循环将会用空闲出来的子进程去调用目标
        
    pool.close()  # 关闭进程池，关闭后po不再接收新的请求
    pool.join()  # 等待po中所有子进程执行完成，必须放在close语句之后

```

> apply_async(func[, args[, kwds]]) ：使用非阻塞方式调用func（并行执行，堵塞方式必须等待上一个进程退出才能执行下一个进程），
>
> args为传递给func的参数列表，
>
> kwds为传递给func的关键字参数列表；
>
> close()：关闭Pool，使其不再接受新的任务；
>
> terminate()：不管任务是否完成，立即终止；
>
> join()：主进程阻塞，等待子进程的退出， 必须在close或terminate之后使用# Pool().apply_async(要调用的目标,(传递给目标的参数元祖,))
>
> 所有参数同上

- **Queue的使用:**

```python
# 队列先进先出,就像排队一样
# 盏先进后出,就像一个盒子一样,放在上面的先拿出来
from multiprocessing import Process, Queue
# 写数据进程执行的代码:
def write(q):
	q.put("value")


# 读数据进程执行的代码:
def read(q):
    value = q.get()
    print(value)

if __name__=='__main__':
# 父进程创建Queue，并传给各个子进程：
    queue = Queue()
    put = Process(target=write, args=(queue,))
    get = Process(target=read, args=(queue,))
    # 启动子进程pw，写入:
    put.start()
    # 等待pw结束:
    put.join()
    # 启动子进程pr，读取:
    get.start()
    get.join()
```

> q.qsize()：返回当前队列包含的消息数量；
>
> q.empty(): 如果队列为空,返回True，反之False ；Queue.full()：如果队列满了，返回True,反之False；
>
> q.get([block[, timeout]])：获取队列中的一条消息，然后将其从列队中移除，block默认值为True；
> q.put(item,[block[, timeout]])：将item消息写入队列，block默认值为True；

- **pool 中使用Queue**

```python
"""
如果要使用Pool创建进程，就需要使用multiprocessing.Manager()中的Queue()，而不是multiprocessing.Queue()，
"""
from multiprocessing import Manager,Pool
# 吧数据写入到队列中
def writer(q):
    for i in "itcast":
        q.put(i)
#从队列中取出数据
def reader(q):
    for i in range(q.qsize()): #q.qsize()可以判断当前队列的长度
        print(q.get())
if __name__=="__main__":
    pool = Pool()
    queue = Manager().Queue()  # 使用Manager中的Queue
    pool.apply_async(writer, (queue,))
    time.sleep(1)# 先让上面的任务向Queue存入数据，然后再让下面的任务开始从中取数据
    pool.apply_async(reader, (queue,))
    pool.close()
    pool.join()
```

- 守护进程

> ```python
> #设置守护主进程方式： 
> process_obj.daemon = True
> #销毁子进程方式： 
> process_obj.terminate()
> ```

| 主进程     | 非守护进程 | 守护进程   | 状态                |
| ---------- | ---------- | ---------- | ------------------- |
| 执行完毕   | 执行完毕   | 未执行完毕 | 杀死守护进程        |
| 执行完毕   | 未执行完毕 | 未执行完毕 | 杀死守护,不杀非守护 |
| 未执行完毕 | 未执行完毕 | 未执行完毕 | 等待主进程执行完毕  |

| 总结                                                         |
| ------------------------------------------------------------ |
| [守护进程不是亲生的,只要主进程结束就得叫他去陪葬;主进程不死他就没事,但是不杀非守护进程](https://blog.csdn.net/u013210620/article/details/78710532#1%E5%AE%88%E6%8A%A4%E5%AD%90%E8%BF%9B%E7%A8%8B%E9%9D%9E%E5%AE%88%E6%8A%A4%E5%AD%90%E8%BF%9B%E7%A8%8B%E5%B9%B6%E5%AD%98) |

> 守护进程就一个让终端程序在后台运行.
>
> 也可以理解,守护进程防止意外终止程序.

### 协程

- **实现方式一**

```python
#无法跳过IO操作
# 手动切换
import time
def work1():
    while True:
        print("----work1---")
        yield
        time.sleep(2)

def work2():
    while True:
        print("----work2---")
        yield
        time.sleep(2)
def main():
    w1 = work1()
    w2 = work2()
    while True:
        next(w1)
        next(w2)
if __name__ == "__main__":
    main()
```

- **方式二**

```python
#无法跳过IO操作
#切换方式较方式一简便,但本质还是手动切换
import greenlet,time
def test1():
    while True:
        print("---A--")
        gr2.switch()
        time.sleep(5)
def test2():
    while True:
        print("---B--")
        gr1.switch()
        time.sleep(5)
gr1 = greenlet.greenlet(test1)
gr2 = greenlet.greenlet(test2)
# 切换到gr1中运行
gr1.switch()
```

- **方式三(重点)**

```python
# 可以跳过IO操作
# 自动切换
from gevent import monkey
import gevent,time
# 最好每次打上补丁既使用monkey.patch_all()，
#否则遇到耗时操作不会自动切换协程,会被阻塞.
# 将程序中用到的耗时操作的代码，换为gevent中自己实现的模块
monkey.patch_all()  
def coroutine_work(coroutine_name):
    for i in range(10):
        print(coroutine_name, i)
        time.sleep(5)

gevent.joinall([
        gevent.spawn(coroutine_work, "work1"),
        gevent.spawn(coroutine_work, "work2")
])
```

### 线程进程协程总结

1. 进程是操作系统资源分配的单位
2. 线程是CPU调度的单位
3. 进程切换需要的资源最大，效率很低
4. 线程切换需要的资源一般，效率一般
5. 协程切换任务资源很小，效率高
6. 多进程、多线程根据cpu核数不一样可能是并行的，但是协程是在一个线程中 所以是并发

> ```
> 虽然多线程是可以利用多和CUP的
> 但是Python多线程只能利用单核CUP，因为GIL的存在。
> 不过可以通过多进程，或者核心代码用C或者C++写
> ```

| 多任务 | 比较                                                         |
| ------ | ------------------------------------------------------------ |
| 进程   | 有自己独立资源，并且进程之间是不共享数据的。所以多进程切换有很多磁盘IO操作，所以性能损失大 |
| 协程   | 没有自己独立的地址空间，并且进程之间是共享数据的，所以多线程切换性能较小,也会导致数据安全问题 |
| 协程   | 完全就是一个函数一样，所以完全没有性能损失。                 |
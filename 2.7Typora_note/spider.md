# response

> ~~~python
> import requests 
> 
> # 目标url
> url = 'https://www.baidu.com' 
> 
> # 向目标url发送get请求
> response = requests.get(url)
> 
> # 打印响应内容
> print(response.text)
> ~~~
>
> ##### response的常用属性：
>
> - `response.text` 响应体 str类型
> - `respones.content` 响应体 bytes类型
> - `response.status_code` 响应状态码
> - `response.request.headers` 响应对应的请求头
> - `response.headers` 响应头
> - `response.request.cookies` 响应对应请求的cookie
> - `response.cookies` 响应的cookie（经过了set-cookie动作）

# requests

### 发送带参数GET请求

> response = requests.get(url, headers=headers, params=kw,cookies=cookie_dict,proxies = proxies) 
>
> ~~~
> # 方式一：利用params参数发送带参数的请求
> import requests
> 
> headers = {"User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36"}
> 
> # 这是目标url
> # url = 'https://www.baidu.com/s?wd=python' 
> 
> # 最后有没有问号结果都一样
> url = 'https://www.baidu.com/s?' 
> 
> # 请求参数是一个字典 即wd=python
> kw = {'wd': 'python'} 
> 
> # 带上请求参数发起请求，获取响应
> response = requests.get(url, headers=headers, params=kw) 
> 
> # 当有多个请求参数时，requests接收的params参数为多个键值对的字典，比如 '?wd=python&a=c'-->{'wd': 'python', 'a': 'c'}
> 
> print(response.content)
> ~~~
>
> 设置代理
>
> ~~~
> proxies = { 
>       "http": "http://12.34.56.79:9527", 
>       "https": "https://12.34.56.79:9527", }
> ~~~
>
> 

### 发送带参数POST请求

>  response = requests.post(url, data = data,headers=headers,json=body,proxies = proxies,cookies=cookie_dict)
>
> ~~~
>  response = requests.post("http://www.baidu.com/", data = data,headers=headers)
> ~~~
>
> 处理cookie的几种方式
>
> ~~~python
> 1.cookie字符串放在headers中
>     headers = {
>     "User-Agent":xxx
>     "Cookie":xxx}
> 
> 2.把cookie字典放传给请求方法的cookies参数接收
>                                         requests.get(url,headers=headers,cookies=cookie_dict}
> 
> 3.使用requests提供的session模块(保存cookie，下一次请求会带上前一次的cookie
> 实现和服务端的长连接，加快请求速度)
> session = requests.session()
> response = session.get(url,headers)
> 
> ~~~

### 总结

> get请求参数
>
> ~~~
> url			请求路径
> headers		请求头
> cookies		cookie
> proxies		代理
> timeout		超时
> params		查询字符串
> ~~~
>
> post请求参数
>
> ~~~
> url			请求路径
> headers		请求头
> proxies		代理
> cookies		cookie
> data		表单数据
> json		请求体
> 
> ~~~
>
> 其他公有
>
> ~~~
> response = requests.get(url,timeout=3)
> 能够保证在3秒钟内返回响应，否则会报错
> 这个方法还能够拿来检测代理ip的质量
> 
> response = requests.get(url,verify=False)
> 请求方法中添加verify=False能够实现请求过程中不验证证书
> 
> requests.utils.dict_from_cookiejar能够实现cookiejar转化为字典
> ~~~
>
> retrying模块
>
> ~~~
> # parse.py
> import requests
> from retrying import retry
> 
> headers = {}
> 
> #最大重试3次，3次全部报错，才会报错
> @retry(stop_max_attempt_number=3) 
> def _parse_url(url)
>     #超时的时候回报错并重试
>     response = requests.get(url, headers=headers, timeout=3) 
>     #状态码不是200，也会报错并重试
>     assert response.status_code == 200
>     return response
> 
> 
> def parse_url(url)
>     try: #进行异常捕获
>         response = _parse_url(url)
>     except Exception as e:
>         print(e)
>         #报错返回None
>         response = None
>     return response
> ~~~
>
> 能实现报错并重试

# 编码

> ~~~
> UTF-8是Unicode的实现方式之一，UTF-8是它是一种变长的编码方式，可以是1，2，3个字节
> 
> 
> .任何语言、任何操作系统、任何编码，都可以和Unicode编码 互相转换。
> ~~~

# 数据提取

### jsonpath

> ~~~
>     安装方法：pip install jsonpath
> 
>     官方文档：http://goessner.net/articles/JsonPath
> ~~~
>
> 用来解析多层嵌套的json数据;

![jsonpath的方法](配置图片\jsonpath的方法.png)

例子：

> ~~~
> book_dict = { 
>   "store": {
>     "book": [ 
>       { "category": "reference",
>         "author": "Nigel Rees",
>         "title": "Sayings of the Century",
>         "price": 8.95
>       },
>       { "category": "fiction",
>         "author": "Evelyn Waugh",
>         "title": "Sword of Honour",
>         "price": 12.99
>       },
>       { "category": "fiction",
>         "author": "Herman Melville",
>         "title": "Moby Dick",
>         "isbn": "0-553-21311-3",
>         "price": 8.99
>       },
>       { "category": "fiction",
>         "author": "J. R. R. Tolkien",
>         "title": "The Lord of the Rings",
>         "isbn": "0-395-19395-8",
>         "price": 22.99
>       }
>     ],
>     "bicycle": {
>       "color": "red",
>       "price": 19.95
>     }
>   }
> }
> 
> from jsonpath import jsonpath
> 
> print(jsonpath(book_dict, '$..author')) #  # 返回列表，如果取不到将返回False
> ~~~
>
> 返回的数据['Nigel Rees', 'Evelyn Waugh', 'Herman Melville', 'J. R. R. Tolkien']
>
> | JSONPath                                 | Result                    |
> | ---------------------------------------- | ------------------------- |
> | `$.store.book[*].author`                 | store中的所有的book的作者 |
> | `$..author`                              | 所有的作者                |
> | `$.store.*`                              | store下的所有的元素       |
> | `$.store..price`                         | store中的所有的内容的价格 |
> | `$..book[2]`                             | 第三本书                  |
> | `$..book[(@.length-1)]` | `$..book[-1:]` | 最后一本书                |
> | `$..book[0,1]` | `$..book[:2]`           | 前两本书                  |
> | `$..book[?(@.isbn)]`                     | 获取有isbn的所有数        |
> | `$..book[?(@.price<10)]`                 | 获取价格大于10的所有的书  |
> | `$..*`                                   | 获取所有的数据            |

### 数据提取之正则

- re.match（从头找一个）

- re.search（找一个）

- re.findall（找所有）

- re.sub（替换）

- re.compile（编译，提升匹配速度）

  - 返回一个模型P，具有和re一样的方法，但是传递的参数不同

  - 匹配模式需要传到compile中

    ```python
    p = re.compile("\d",re.S)
    p.findall("chuan1zhi2")
    ```

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
  > ```

**常用正则表达式**

| 正则                                          | 作用      |
| --------------------------------------------- | --------- |
| ^[\u4e00-\u9fa5]{0,}$                         | 匹配汉字  |
| ^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$ | Email地址 |

中文的 unicode 编码范围 主要在 [u4e00-u9fa5]

### xpath

> lxml有xpath方法
>
> ~~~
> XPath (XML Path Language) 是一门在 HTML\XML 文档中查找或者解析信息的语言
> ~~~
>
> 1. lxml库的安装: `pip install lxml`
> 2. lxml的导包:`from lxml import etree`
>
> ~~~
> lxml转换解析类型的方法:etree.HTML(text)
> lxml解析数据的方法:data.xpath("//div/text()")
> 需要注意lxml提取完毕数据的数据类型都是列表类
> ~~~
>
> 将对象转成文本
>
> ~~~
> from lxml.html import fromstring, tostring
> html_obj.to_string() # html_obj 为Elment对象
> 
> ~~~
>
> 

| 表达式   | 描述                                                       |
| -------- | ---------------------------------------------------------- |
| nodename | 选中该元素。                                               |
| /        | 从根节点选取、或者是元素和元素间的过渡。                   |
| //       | 从匹配选择的当前节点选择文档中的节点，而不考虑它们的位置。 |
| .        | 选取当前节点。                                             |
| ..       | 选取当前节点的父节点。                                     |
| @        | 选取属性。                                                 |
| text()   | 选取文本。                                                 |
| @href    | 获取所有的a标签的href                                      |

| 通配符 | 描述                 |
| ------ | -------------------- |
| *      | 匹配任何元素节点。   |
| @*     | 匹配任何属性节点。   |
| node() | 匹配任何类型的节点。 |

**例子**

| 路径表达式                          | 结果                                       |
| ----------------------------------- | ------------------------------------------ |
| //book/title \| //book/price        | 选取 book 元素的所有 title 和 price 元素。 |
| //title \| //price                  | 选取文档中的所有 title 和 price 元素。     |
| "//div[@class='f18 mb20']/a/text()" | 文本内容                                   |
| "//div[@class='f18 mb20']/a/@href"  | 特定属性值                                 |

### BeautifulSoup4

> ~~~
> pip install beautifulsoup4
> from bs4 import BeautifulSoup
> Beautiful Soup 主要的功能也是如何解析和提取 HTML/XML 数据。
> ~~~
>
> lxml 只会局部遍历，而Beautiful Soup 是基于HTML DOM的，会载入整个文档，解析整个DOM树，因此时间和内存开销都会大很多，所以性能要低于lxml。
>
> ~~~
> 通过soup = BeautifulSoup(html, 'lxml')方式指定lxml解析器。
> ~~~
>
> 创建 Beautiful Soup 对象的三种途径
>
> ~~~
> # 1.直接使用默认解析器
> soup = BeautifulSoup(html)
> # 2.指明解析器
> soup = BeautifulSoup(html, 'lxml')
> 
> # 3.打开本地 HTML 文件的方式来创建对象
> #soup = BeautifulSoup(open('index.html'))
> 
> #格式化输出 soup 对象的内容
> print(soup.prettify())
> ~~~
>
> find()：返回网页中第一个符合匹配的结果（通传入标签和属性）
> find_all()：返回网页中所有符合匹配的结果 列表
> select()：返回网站所有符合匹配的结果列表（但是语法是CSS选择器）
> find("div", {"class" : "f18 mb20", "id" : "abcd"})
> find_all("div", {"class" : "f18 mb20"})
>
> ~~~
> find_all(name, attrs, recursive, text, **kwargs)
> name 参数
> 查找所有名字为 name 的tag
> 
> keyword 参数
> soup.find_all(class_="sister")
> 
> text 参数
> 通过 text 参数可以搜索文档中的字符串内容
> ~~~

**CSS选择器**

> ##### 通过标签选择器查找
>
> ~~~
> print(soup.select('title'))
> #[<title>The Dormouse's story</title>]
> 
> print(soup.select('a'))
> #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
> 
> print(soup.select('b'))
> #[<b>The Dormouse's story</b>]
> ~~~
>
> ##### 通过类选择器查找
>
> ~~~
> print(soup.select('.sister'))
> #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
> ~~~
>
> ##### 通过 id 选择器查找
>
> ```
> print(soup.select('#link1'))
> #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
> ```
>
> ##### 层级选择器 查找
>
> ~~~
> print(soup.select('p #link1'))
> #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>]
> ~~~
>
> ##### 通过属性选择器查找
>
> ~~~
> print(soup.select('a[class="sister"]'))
> #[<a class="sister" href="http://example.com/elsie" id="link1"><!-- Elsie --></a>, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]
> ~~~

**具体数据获取**

> ##### 获取文本内容 get_text()
>
> 以上的 select 方法返回的结果都是列表形式，可以遍历形式输出，然后用 get_text() 方法来获取它的内容。
>
> ~~~
> soup = BeautifulSoup(html, 'lxml')
> for title in soup.select('title'):
>     print(title.get_text()
> ~~~
>
> ##### 获取属性 get('属性的名字')
>
> ~~~
> soup = BeautifulSoup(html, 'lxml')
> print(soup.select('a')[0].get('href'))
> ~~~

# 高性能爬虫

### 多进程爬虫

### ![多线程爬虫](D:\常用资料\笔记\2.7Typora_note\配置图片\多线程爬虫.png)

> 多线程，分模块，使用队列通信
>
> ~~~python
> import requests
> from lxml import etree
> from queue import Queue
> import threading
> 
> 
> class Qiubai:
>     def __init__(self):
>         self.temp_url = "https://www.qiushibaike.com/8hr/page/{}/"
>         self.headers= {"User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X \
>         10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36"}
>         self.url_queue = Queue()
>         self.html_queue = Queue()
>         self.content_list_queue = Queue()
> 
>     def get_url_list(self):#获取url列表
>         for i in range(1,14):
>             self.url_queue.put(self.temp_url.format(i))
> 
>     def parse_url(self):
>         while True: #在这里使用，子线程不会结束，把子线程设置为守护线程
>             url = self.url_queue.get()
>             # print(url)
>             response = requests.get(url,headers=self.headers)
>             self.html_queue.put(response.content.decode())
>             self.url_queue.task_done()
> 
> 
>     def get_content_list(self):  #提取数据
>         while True:
>             html_str = self.html_queue.get()
>             html = etree.HTML(html_str)
>             div_list = html.xpath("//div[@id='content-left']/div")
>             content_list = []
>             for div in div_list:
>                 content = {}
>                 content["content"] = div.xpath(".//div[@class='content']/span/text()")
>                 content_list.append(content)
>             self.content_list_queue.put(content_list)
>             self.html_queue.task_done()
> 
>     def save_content_list(self):
>         while True:
>             content_list = self.content_list_queue.get()
>             for content in content_list:
>                 print(content) # 此处对数据进行保存操作
>             self.content_list_queue.task_done()
> 
> 
>     def run(self):
>         thread_list = []
>         #1.url_list
>         t_url = threading.Thread(target=self.get_url_list)
>         thread_list.append(t_url)
>         #2.遍历，发送请求，
>         for i in range(3):  #三个线程发送请求
>             t_parse = threading.Thread(target=self.parse_url)
>             thread_list.append(t_parse)
>         #3.提取数据
>         t_content = threading.Thread(target=self.get_content_list)
>         thread_list.append(t_content)
>         #4.保存
>         t_save = threading.Thread(target=self.save_content_list)
>         thread_list.append(t_save)
> 
>         for t in thread_list:
>             t.setDaemo
> ~~~
>
> 

### 多线程爬虫

> ~~~pYthon
> import requests
> from lxml import etree
> from multiprocessing import Process
> from multiprocessing import JoinableQueue as Queue
> 
> 
> class QiubaiSpider:
>     def __init__(self):
>         self.url_temp = "https://www.qiushibaike.com/8hr/page/{}/"
>         self.headers = {"User-Agent":"Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_3) \
>         AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36"}
>         self.url_queue = Queue()  #保存url
>         self.html_queue = Queue() #保存html字符串
>         self.content_queue = Queue() #保存提取到的数据
> 
>     def get_url_list(self):
>         for i in range(1,14):
>             self.url_queue.put(self.url_temp.format(i))
> 
>     def parse_url(self):
>         while True:
>             url = self.url_queue.get()
>             html_str =  requests.get(url,headers=self.headers).content.decode()
>             self.html_queue.put(html_str)
>             self.url_queue.task_done()
> 
>     def get_content_list(self):
>         while True:
>             html_str = self.html_queue.get()
>             html = etree.HTML(html_str)
>             div_list = html.xpath("//div[@id='content-left']/div")
>             content_list = []
>             for div in div_list:
>                 content = {}
>                 content["content"]=div.xpath(".//div[@class='content']/span/text()")
>                 content_list.append(content)
>             self.content_queue.put(content_list)
>             self.html_queue.task_done()
> 
>     def save_content_list(self):
>         while True:
>             content_list = self.content_queue.get()
>             for content in content_list:
>                 print(content) # 此处对数据进行保存操作
>             self.content_queue.task_done()
> 
>     def run(self):
>         process_list = []
>         #1. url_list
>         t_url = Process(target=self.get_url_list)
>         process_list.append(t_url)
>         #2. 遍历，发送请求
>         for i in range(5):#创建5个子进程
>             t_parse = Process(target=self.parse_url)
>             process_list.append(t_parse)
>         #3. 提取数据
>         t_content = Process(target=self.get_content_list)
>         process_list.append(t_content)
>         #4. 保存
>         t_save = Process(target=self.save_content_list)
>         process_list.append(t_save)
> 
>         for t in process_list:
>             t.daemon=True #把进线程设置为守护线程，主进程技术，子进程结束
>             t.start()
> 
>         for q in [self.url_queue,self.html_queue,self.content_queue]:
>             q.join()  #让主进程阻塞
> 
>         print("主进程结束")
> 
> if __name__ == '__main__':
>     qiubai = QiubaiSpider()
>     qiubai.run()
> ~~~
>
> 

### 线程池爬虫

> ~~~python
> import requests
> from lxml import etree
> from queue import Queue
> from multiprocessing.dummy import Pool
> import time
> 
> 
> class QiubaiSpider:
>     def __init__(self):
>         self.url_temp = "https://www.qiushibaike.com/8hr/page/{}/"
>         self.headers = {"User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X \
>         10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36"}
>         self.queue = Queue()
>         self.pool = Pool(5)
>         self.is_running = True
>         self.total_requests_num = 0
>         self.total_response_num = 0
> 
>     def get_url_list(self):  # 获取url列表
>         for i in range(1, 14):
>             self.queue.put(self.url_temp.format(i))
>             self.total_requests_num += 1
> 
>     def parse_url(self, url):  # 发送请求，获取响应
>         return requests.get(url, headers=self.headers).content.decode()
> 
>     def get_content_list(self, html_str):  # 提取段子
>         html = etree.HTML(html_str)
>         div_list = html.xpath("//div[@id='content-left']/div")
>         content_list = []
>         for div in div_list:
>             content = {}
>             content["content"] = div.xpath(".//div[@class='content']/span/text()")
>             print(content)
>             content_list.append(content)
>         return content_list
> 
>     def save_content_list(self, content_list):  # 保存数据
>         for content in content_list:
>             print(content)  # 此处对数据进行保存操作
> 
>     def exetute_requests_item_save(self):
>         url = self.queue.get()
>         html_str = self.parse_url(url)
>         content_list = self.get_content_list(html_str)
>         self.save_content_list(content_list)
>         self.total_response_num += 1
> 
>     def _callback(self, temp):
>         if self.is_running:
>             self.pool.apply_async(self.exetute_requests_item_save, callback=self._callback)
> 
>     def run(self):
>         self.get_url_list()
> 
>         for i in range(2):  # 控制并发
>             self.pool.apply_async(self.exetute_requests_item_save, callback=self._callback)
> 
>         while True:  # 防止主线程结束
>             time.sleep(0.0001)  # 避免cpu空转，浪费资源
>             if self.total_response_num >= self.total_requests_num:
>                 self.is_running = False
>                 break
> 
> 
> if __name__ == '__main__':
>     qiubai = QiubaiSpider()
>     qiubai.run()
> ~~~
>
> ~~~
> from processing.dummy import Pool
> pool = Pool(10)
> # 将每一个url地址 做为参数传给send_request()发送，并将响应传给parse_page
> for url in self.url_list:
>     pool.apply_async(self.send_request, args=[url], callback=self.parse_page)
> 
> # 关闭线程池，不再接受新的任务
> pool.close()
> # 让主线程阻塞
> pool.join()
> ~~~
>
> 

### 协程池爬虫

> ~~~
> # coding=utf-8
> import gevent.monkey
> gevent.monkey.patch_all()
> from gevent.pool import Pool
> 
> import requests
> from lxml import etree
> from queue import Queue
> import time
> 
> 
> class QiubaiSpider:
>     def __init__(self):
>         self.url_temp = "https://www.qiushibaike.com/8hr/page/{}/"
>         self.headers = {"User-Agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X \
>         10_13_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/64.0.3282.186 Safari/537.36"}
>         self.queue = Queue()
>         self.pool = Pool(5)
>         self.is_running = True
>         self.total_requests_num = 0
>         self.total_response_num = 0
> 
>     def get_url_list(self):  # 获取url列表
>         for i in range(1, 14):
>             self.queue.put(self.url_temp.format(i))
>             self.total_requests_num += 1
> 
>     def parse_url(self, url):  # 发送请求，获取响应
>         return requests.get(url, headers=self.headers).content.decode()
> 
>     def get_content_list(self, html_str):  # 提取段子
>         html = etree.HTML(html_str)
>         div_list = html.xpath("//div[@id='content-left']/div")
>         content_list = []
>         for div in div_list:
>             content = {}
>             content["content"] = div.xpath(".//div[@class='content']/span/text()")
>             print(content)
>             content_list.append(content)
>         return content_list
> 
>     def save_content_list(self, content_list):  # 保存数据
>         for content in content_list:
>             print(content)  # 此处对数据进行保存操作
> 
>     def exetute_requests_item_save(self):
>         url = self.queue.get()
>         html_str = self.parse_url(url)
>         content_list = self.get_content_list(html_str)
>         self.save_content_list(content_list)
>         self.total_response_num += 1
> 
>     def _callback(self, temp):
>         if self.is_running:
>             self.pool.apply_async(self.exetute_requests_item_save, callback=self._callback)
> 
>     def run(self):
>         self.get_url_list()
> 
>         for i in range(2):  # 控制并发
>             self.pool.apply_async(self.exetute_requests_item_save, callback=self._callback)
> 
>         while True:  # 防止主线程结束
>             time.sleep(0.0001)  # 避免cpu空转，浪费资源
>             if self.total_response_num >= self.total_requests_num:
>                 self.is_running = False
>                 break
> 
> 
> if __name__ == '__main__':
>     qiubai = QiubaiSpider()
>     qiubai.run()
> ~~~



### 大杂烩

> **线程池版多线程**
>
> ~~~
> 
> #coding:utf-8
> 
> import time
> try:
>     from Queue import Queue
> except:
>     from queue import Queue
> 
> #import threading
> # 导入多进程模块的 线程池
> from multiprocessing.dummy import Pool
> 
> import requests
> from lxml import etree
> 
> 
> class DoubanSpider(object):
>     def __init__(self):
>         self.url_list = ["https://movie.douban.com/top250?start=" + str(page) for page in range(0, 225 + 1, 25)]
>         self.headers = {"User-Agent" : "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko"}
>         self.queue = Queue()
> 
> 
>     def send_request(self, url):
>         print("[INFO]: send request {}".format(url))
>         response = requests.get(url, headers=self.headers)
>         time.sleep(1)
>         return response
> 
> 
>     def parse_page(self, response):
>         html = response.content
>         html_obj = etree.HTML(html)
>         node_list = html_obj.xpath("//div[@class='info']")
> 
>         for node in node_list:
>             title = node.xpath("./div[@class='hd']/a/span[1]/text()")[0]
>             score = node.xpath(".//span[@class='rating_num']/text()")[0]
> 
>             self.queue.put(title + "\t" + score)
> 
> 
>     def main(self):
>         # for url in self.url_list:
>         #     response = self.send_request(url)
>         #     self.parse_page(response)
> 
>         # thread_list = []
>         # for url in self.url_list:
>         #     thread = threading.Thread(target=self.send_request, args=[url])
>         #     thread.start()
>         #     thread_list.append(thread)
> 
>         # # 让主线程阻塞，等待所有子线程执行戒色胡
>         # for thread in thread_list:
>         #     thread.join()
> 
> 
>         # 创建10个线程的线程池对象
>         pool = Pool(10)
>         # # 通过多线程发送所有的url地址请求，并将响应保存在 response_list
>         # response_list = pool.map(self.send_request, self.url_list)
>         # # 解析每个响应
>         # none_list = [self.parse_page(response) for response in response_list]
> 
>         # 将每一个url地址 做为参数传给send_request()发送，并将响应传给parse_page
>         for url in self.url_list:
>             pool.apply_async(self.send_request, args=[url], callback=self.parse_page)
> 
>         # 关闭线程池，不再接受新的任务
>         pool.close()
>         # 让主线程阻塞
>         pool.join()
> 
> 
> 
> 
> if __name__ == '__main__':
>     spider = DoubanSpider()
>     start = time.time()
>     spider.main()
>     end = time.time()
>     # 3sec
>     print("[INFO]: using time {}".format(end - start))
> 
> 
> ~~~
>
> **协程**
>
> ~~~
> #coding:utf-8
> 
> import time
> try:
>     from Queue import Queue
> except:
>     from queue import Queue
> 
> #import threading
> # 导入多进程模块的 线程池
> #from multiprocessing.dummy import Pool
> 
> 
> 
> import requests
> from lxml import etree
> 
> import gevent
> from gevent.pool import Pool # 协程池
> 
> from gevent import monkey
> monkey.patch_all()
> 
> class DoubanSpider(object):
>     def __init__(self):
>         self.url_list = ["https://movie.douban.com/top250?start=" + str(page) for page in range(0, 225 + 1, 25)]
>         self.headers = {"User-Agent" : "Mozilla/5.0 (Windows NT 10.0; WOW64; Trident/7.0; rv:11.0) like Gecko"}
>         self.queue = Queue()
> 
> 
>     def send_request(self, url):
>         print("[INFO]: send request {}".format(url))
>         response = requests.get(url, headers=self.headers)
>         time.sleep(1)
>         return response
> 
> 
>     def parse_page(self, response):
>         html = response.content
>         html_obj = etree.HTML(html)
>         node_list = html_obj.xpath("//div[@class='info']")
> 
>         for node in node_list:
>             title = node.xpath("./div[@class='hd']/a/span[1]/text()")[0]
>             score = node.xpath(".//span[@class='rating_num']/text()")[0]
> 
>             self.queue.put(title + "\t" + score)
> 
> 
>     def main(self):
>         # pool = Pool()
>         # for url in self.url_list:
>         #     pool.apply_async(self.send_request, args=[url], callback=self.parse_page)
> 
>         # pool.join()
> 
> 
> 
>         # job_list = []
>         # for url in self.url_list:
>         #     job = gevent.spawn(self.send_request, url)
>         #     job_list.append(job)
> 
>         # g_list = gevent.joinall(job_list)
>         # for g in g_list:
>         #     self.parse_page(g.get())
> 
>         _ = [self.parse_page(g.get()) for g in gevent.joinall([gevent.spawn(self.send_request,url) for url in self.url_list])]
> 
> 
> 
> if __name__ == '__main__':
>     spider = DoubanSpider()
>     start = time.time()
>     spider.main()
>     end = time.time()
>     # 3sec
>     print("[INFO]: using time {}".format(end - start))
> 
> 
> ~~~
>
> 

### 总结

> 线程协程的写法一摸一样，只不过导包的写法不一样
>
> ~~~python
> # from multiprocessing.dummy import Pool	线程
> from gevent.pool import Pool			协程
> 
> pool = Pool(50)
> 
> result_list = pool.map(func, list)  : 将 list中的每个元素依次做为参数传给func执行，并返回所有结果的列表
> 
> pool.map_async(func, list, callback) : 将 list中的每个元素依次做为参数传给func执行，最后将所有结果的列表传递给 callback 回调函数执行
> 
> result = pool.apply(func, args) ： 将args做为参数传递给func执行，并返回结果（同步阻塞）
> 
> pool.apply_async(func, args, callback) : 将args做为参数传递给func执行，并将结果传递给callback回调函数执行
> 
> # 线程池需要close() ，协程不需要
> pool.close()
> pool.join()
> ~~~

# selenium

> ### selenium
>
> ~~~
> Selenium是一个Web的自动化测试工具，最初是为网站自动化测试而开发的，Selenium 可以直接运行在浏览器上，它支持所有主流的浏览器（包括PhantomJS这些无界面的浏览器），可以接收指令，让浏览器自动加载页面，获取需要的数据，甚至页面截屏
> ~~~
>
> ###  PhantomJS
>
> ~~~
> PhantomJS 是一个基于Webkit的“无界面”(headless)浏览器，它会把网站加载到内存并执行页面上的 JavaScript
> ~~~
>
> ### Chromedriver
>
> ~~~
> Chromedriver 也是一个能够被selenium驱动的浏览器，但是和PhantomJS的区别在于它是有界面的
> ~~~

**加载页面**

~~~
selenium通过控制浏览器，所以对应的获取的数据都是elements中的内容

```python
from selenium import webdriver 
# 指定driver的绝对路径
# driver = webdriver.PhantomJS(executable_path='/home/worker/Desktop/driver/phantomjs') 
driver = webdriver.Chrome(executable_path='/home/worker/Desktop/driver/chromedriver')
# 向一个url发起请求
driver.get("http://www.itcast.cn/")
# 把网页保存为图片
driver.save_screenshot("itcast.png")
# 退出模拟浏览器
driver.quit() # 一定要退出！不退出会有残留进程！
```
~~~

> 

# scrapy框架

### 快速入门

> ~~~
> item组件,以及中间件默认是未开启的.需要手动开启
> ~~~



### ![scrapy_all](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/scrapy_all.png)

| 组件                   | 功能                                                         |
| ---------------------- | ------------------------------------------------------------ |
| `Scrapy Engine(引擎)`  | **调用各个组件**的方法,并执行组件的功能和**传递数据**        |
| `Scheduler(调度器)`    | **请求去重**(记录每个sha1指纹,每次来的新请求就和指纹集合对比) |
| `Downloader（下载器）` | 接受并发送**请求**,返回**响应**                              |
| `Spider（爬虫）`       | 构建第一批请求,**解析响应**                                  |
| `Item Pipeline(管道)`  | 对数据做后续**存储**                                         |

> - `Downloader Middlewares（下载中间件）`：你可以当作是一个可以自定义扩展下载功能的组件。
> - `Spider Middlewares（Spider中间件）`：你可以理解为是一个可以自定扩展和操作`引擎`和`Spider`中间`通信`的功能组件（比如进入`Spider`的Responses;和从`Spider`出去的Requests）



**scrapy基本命令**

![7.1](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/7.1.png)

| 命令         | 命令示例                            | 功能                       |
| ------------ | ----------------------------------- | -------------------------- |
| bench        | scrapy bench                        | 测试下爬取性能             |
| commands     | 没有这个命令                        |                            |
| fetch        | scrapy fetch "http://www.baidu.com" | 抓取一个请求               |
| gensprder    |                                     | 创建爬虫(可以创建多个爬虫) |
| runspider    |                                     | 运行爬虫()                 |
| setting      |                                     |                            |
| shell        | scrapy shell "http://www.baidu.com" | 可以在ipython中测试代码    |
| startproject |                                     |                            |
| version      |                                     |                            |
| view         |                                     |                            |

**爬虫默认目录结构**

> ~~~
> # 创建爬虫项目 目录结构如下图
> scrapy startproject mySpider	
> # 新建爬虫文件
> scrapy genspider itcast "itcast.cn"
> # 执行爬虫文件
> scrapy crawl itcast
> ~~~
>
> 

![爬虫结构目录](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/%E7%88%AC%E8%99%AB%E7%BB%93%E6%9E%84%E7%9B%AE%E5%BD%95.png)

**一个简单的爬虫**

> ~~~
> 下面两个图片,是编写一个爬虫,最重要的三个文件.
> 另外还需要导setting.py中去启用,以及配置请求头等.
> ~~~

![scrapy组件item](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/scrapy%E7%BB%84%E4%BB%B6item.png)



![spider_组件数据保存](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/spider_%E7%BB%84%E4%BB%B6%E6%95%B0%E6%8D%AE%E4%BF%9D%E5%AD%98.png)


##  [os 模块](https://www.runoob.com/python/os-file-methods.html)

| 方法                                    | 功能             |
| --------------------------------------- | ---------------- |
| os.rename("oldfile.txt", "newfile.txt") | 重命名           |
| os.getcwd()                             | 获取当前目录     |
| os.chdir("path")                        | 切换路径         |
| os.listdir()                            | 获取目录列表     |
| os.mkdir("newfile")                     | 创建文件夹       |
| os.mknod("test.py")                     | 创建文件         |
| os.remove("file.txt")                   | 删除文件         |
| os.rmdir("file")                        | 删除文件夹       |
| os.system("program")                    | 打开程序         |
| os.getpid()                             | 获取当前进程编号 |
| os.getppid()                            | 获取父进程的编号 |

## [sys模块](https://blog.csdn.net/qq_38526635/article/details/81739321)

| 方法                                                         | 功能                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| sys.argv                                                     | 命令行参数                                                   |
| sys.path                                                     | path是一个目录列表,供Python从中查找第三方扩展模块            |
| sys.exit(n)                                                  | 调用sys,exit(n)可以中途退出程序，sys.exit(0)表示正常退出，n不为0时，会引发SystemExit异常,从而在主程序中可以捕获该异常。 |
| sys.modules.keys()                                           | 返回所有已经导入的模块列表                                   |
| sys.platform                                                 | 获取当前执行环境的平台                                       |
| [参考网址](https://blog.csdn.net/Alawaka2018/article/details/80784893) |                                                              |



## [json 模块](https://www.cnblogs.com/tjuyuan/p/6795860.html)

| 方法         | 功能                          |
| ------------ | ----------------------------- |
| json.dumps() | 编码.把Python数据转成json数据 |
| json.loads() | 解码.把json数据转成Python数据 |







## random 模块

|       方法       | 功能                                         |      |
| :--------------: | -------------------------------------------- | ---- |
|   choice(list)   | 从列表中随机抽出一个值                       | 常用 |
|     random()     | 生成一个[0,1)之间的随机小数                  | 常用 |
|   dandint(a,b)   | 生成一个a到b的随机整数                       | 常用 |
| randrange(a,b,c) | 成一个a到b，步长为c的随机数                  |      |
|   uniform(a,b)   | 生成一个a到b的随机小数                       |      |
|    shuffle()     | 打乱并返回打乱后的列表                       |      |
|   sample(a，b)   | a类型数据中取出b个数据，以列表的形式返回出来 |      |
|      seed()      | 种子函数；默认值为当前时间戳                 |      |

## time模块

| 方法             | 功能                          |      |
| ---------------- | ----------------------------- | ---- |
| time.time()      | 获取时间戳                    | 常用 |
| time.ctime()     | Sun Jul 14 17:10:56 2019      | 常用 |
| time.strftime()  | 把struct_time对象输出成字符串 | 常用 |
| time.strptime()  | 把字符串转成struct_time对象   |      |
| time.gmtime()    | 会产生一个srtuct_time对象     |      |
| time.localtime() | 会产生一个srtuct_time对象     |      |
| time.mktime()    | 把struct_time对象转成时间戳   |      |

> ~~~python
> #struct_time对象
> time.struct_time(tm_year=2019, tm_mon=7, tm_mday=14, tm_hour=9, tm_min=12, tm_sec=18, tm_wday=6, tm_yday=195, tm_isdst=0) 
> ~~~

| 年   | 月   | 日   | 时   | 分   | 秒   |
| ---- | ---- | ---- | ---- | ---- | ---- |
| %Y   | %m   | %d   | %H   | %M   | %S   |

| 月名 | 星期 | %p        |
| ---- | ---- | --------- |
| %B   | %A   | 上午/下午 |

 ~~~python
time.strftime("%Y-%m-%d %H:%M:%S")
#2019-07-26 20:37:05

import datetime
now_time = datetime.datetime.now()
datetime.datetime.now().strftime('%Y-%m-%d')
 ~~~



## Pickle模块

**功能**

| 函数  | 功能                                | 示例                                |
| ----- | ----------------------------------- | ----------------------------------- |
| dump  | 将obj对象序列化存入已经打开的file中 | pickle.dump(obj, file, [,protocol]) |
| load  | 将file中的对象序列化读出。          | pickle.load(file)                   |
| dumps | 将obj对象序列化为bytes形式          | pickle.dumps(obj[, protocol])       |
| loads | 从bytes中读出序列化前的obj对象。    | pickle.loads(bytes)                 |

## Base64(可逆加密)

| 函数      | 功能                                   | 示例                    |
| --------- | -------------------------------------- | ----------------------- |
| b64encode | 将bytes类型数据加密成bytes密码         | base64.b64encode(bytes) |
| b64encode | 将bytes或string密码解密成bytes类型数据 | base64.b64decode(bytes) |

## hashlib(不可逆加密)

| 方法    | 功能                             |
| ------- | -------------------------------- |
| hashlib | 加密的时候只能放bytes类型的数据, |
|         |                                  |

> ~~~python
> import hashlib
> 
> key = "hjkKajHSJAhJkhnjKHKJHSKZj".encode()
> str_bytes = "hello world".encode()
> # ######## md5 ########
> md5 = hashlib.md5(key)
> md5.update(str_bytes)     #注意转码
> res = md5.hexdigest()
> print("md5加密结果:",res)
> 
> # ######## sha1 ########
> 
> sha1 = hashlib.sha1(key)
> sha1.update(str_bytes)
> res = sha1.hexdigest()
> print("sha1加密结果:",res)
> 
> # ######## sha256 ########
> 
> sha256 = hashlib.sha256(key)
> sha256.update(str_bytes)
> res = sha256.hexdigest()
> print("sha256加密结果:",res)
> 
> 
> # ######## sha384 ########
> 
> sha384 = hashlib.sha384(key)
> sha384.update(str_bytes)
> res = sha384.hexdigest()
> print("sha384加密结果:",res)
> 
> # ######## sha512 ########
> 
> sha512= hashlib.sha512(key)
> sha512.update(str_bytes)
> res = sha512.hexdigest()
> print("sha512加密结果:",res)
> 
> 
> import hmac
> 
> h = hmac.new(key)
> h.update(str_bytes)
> print("hmac加密结果:",h.hexdigest())
> ~~~
>
> md5加密结果: 697ff76c41cc51878a6a724b61f35f78
> sha1加密结果: 4af4d3f0e6a2270b6d7e64c83fb56b1cd695c79a
> sha256加密结果: 7bc44b836fbd70e71341cbb8b5e0594a222fd2e19bae6d6f61610fd119597515
> sha384加密结果: dcb176d085f5f33f97ef482723c88f01b7715ca62c7321bd45569356b3d2280f5a6312e7b16f90bc25f7b70bf4635ca1
> sha512加密结果: 237abb059464f983222a08bd63b765bbedf01983e7253185c4334002c979e80e64738406fd3b5fd472e83d2f4c46e7ab7dded6936587102a5993cbb931a80b79
> hmac加密结果: a6ac245575beecc10ed51976d83b8658

~~~
加密的时候都是可以放入私钥的,也可以不放入私钥.
~~~

**JWT**

> ~~~python
> import base64
> import hashlib
> import json
> 
> handler = base64.b64encode(json.dumps(a).encode())
> payload = base64.b64encode(json.dumps(b).encode())
> token  = hashlib.sha256(key)
> token.update(handler+payload)
> res = sha256.hexdigest()
> print(handler+'.'.encode()+payload+'.'.encode()+res.encode())
> ~~~

## CSV

> **将元祖或列表写入到csv中**
>
> ~~~
> # 以元祖或者列表的方式写入
> with open("top250.csv","a") as f:
>     data = csv.writer(f)
>     data.writerow((title_xpath,score_xpath,))
> ~~~
>
> **将字典写入到csv中**
>
> ~~~
> import csv
> 
> headers = ['Symbol', 'Price', 'Date', 'Time', 'Change', 'Volume']
> rows = [{'Symbol':'AA', 'Price':39.48, 'Date':'6/11/2007',
>         'Time':'9:36am', 'Change':-0.18, 'Volume':181800},
>         {'Symbol':'AIG', 'Price': 71.38, 'Date':'6/11/2007',
>         'Time':'9:36am', 'Change':-0.15, 'Volume': 195500},
>         {'Symbol':'AXP', 'Price': 62.58, 'Date':'6/11/2007',
>         'Time':'9:36am', 'Change':-0.46, 'Volume': 935000},
>         ]
> 
> with open('stocks.csv','w') as f:
>     f_csv = csv.DictWriter(f, headers)
>     f_csv.writeheader()
>     f_csv.writerows(rows)
> ~~~

## [psutil模块]( https://www.cnblogs.com/saneri/p/7528283.html )

> 安装
>
> ~~~
> pip install psutil
> ~~~

| 方法                     | 例子                            | 功能              |
| ------------------------ | ------------------------------- | ----------------- |
| cpu_times()              | psutil.cpu_times()              | 获取cpu的完整信息 |
| cpu_count()              | psutil.cpu_count()              | 获取cpu逻辑个数   |
| cpu_count(logical=False) | psutil.cpu_count(logical=False) | 获取cpu物理个数   |
| cpu_percent()            | psutil.cpu_percent()            | 获取cpu的使用率   |
| virtual_memory()         | psutil.virtual_memory()         | **内存信息**      |
| psutil.disk_usage('/')   |                                 | **磁盘信息**      |
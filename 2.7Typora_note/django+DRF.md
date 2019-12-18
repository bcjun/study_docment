[TOC]





~~~mermaid
graph LR;
    浏览器-->Nginx;
    Nginx-->静态数据;
    Nginx-->uWSGI;
    uWSGI-->Django;
    Django-->Redis;
    Django-->Mysql;
    
    linkStyle 0 stroke:#0ff,stroke-width:2px;
    linkStyle 2 stroke:#ff3,stroke-width:2px;
    linkStyle 1 stroke:#ff3,stroke-width:2px;
    linkStyle 2 stroke:#ff3,stroke-width:2px;
    linkStyle 3 stroke:#ff9,stroke-width:2px;
    linkStyle 4 stroke:#ff3,stroke-width:2px;
    linkStyle 5 stroke:#ff3,stroke-width:2px;
~~~



# django项目准备阶段

### Django准备阶段常用命令

> ~~~shell
> # 创建工程
> django-admin startproject 工程名称
> # 创建子应用
> python manage.py startapp 子应用名称
> # 运行服务器 默认8000端口
> python manage.py runserver  ip:端口
> # 创建超级管理员用户
> python manage.py createsuperuser
> ~~~

### 虚拟环境搭建

+ 虚拟环境作用

  >  搭建独立的python运行环境, 每一个项目拥有单独python运行环境。

+ 搭建虚拟环境步骤

  > ~~~shell
  > # 安装创建虚拟环境的包
  > sudo pip install virtualenv
  > sudo pip install virtualenvwrapper
  > # 创建虚拟环境,不能用sudo创建
  > mkvirtualenv django_py3 -p python3
  > # 进入虚拟环境 workon 按两下tab键能列出所有虚拟环境
  > workon 虚拟环境名称
  > ~~~

+ 搭建虚拟环境时需要用到的其他命令

  > ~~~shell
  > # 查看python3的安装路径
  > which python3
  > # 查看虚拟环境中的第三方包,需要先进到虚拟环境中
  > pip list
  > # 退出虚拟环境   
  > deactivate
  > ~~~

### 创建工程

+ 创建工程

  > ~~~shell
  > django-admin startproject 工程名称
  > ~~~

+ 工程目录文件的作用

  > ~~~python
  > #项目的整体配置文件。
  > settings.py
  > #项目的URL配置文件。
  > urls.py 
  > # 项目部署上线后,项目的启动文件，入口文件。
  > wsgi.py 
  > # 是项目管理文件，通过它管理项目，项目在开发阶段的启动入口
  > manage.py 
  > ~~~

### 创建子应用

+ 创建子应用

  > ~~~shell
  > python manage.py startapp 子应用名称
  > ~~~

+  子应用目录文件的作用

  > ~~~python
  > # 文件跟网站的后台管理站点配置相关。
  > admin.py 
  > # 文件用于配置当前子应用的相关信息。
  > apps.py 
  > # 目录用于存放数据库迁移历史文件。
  > migrations 
  > # 文件用户保存数据库模型类。
  > models.py 
  > # 文件用于开发测试用例，编写单元测试。
  > tests.py 
  > # 文件用于编写Web应用视图。
  > views.py 
  > ~~~

+ 注册子应用

  > ~~~
  > 将子应用中的apps.py中的Config类添加工程配置文件: settings.py中的INSTALLED_APPS列表中
  > ~~~

### 创建视图配置路由

> Django是在views.py文件中来编写Web应用程序的业务逻辑代码。

+ 创建子应用路由文件urls.py

> ~~~python
> # 配置视图路由
> urlpatterns = [
>     # 每个路由信息都需要使用url函数来构造
>     # url(路径, 视图)
>     url(r'^index/$', views.index),
> ]
> ~~~
>
> 

+ 在工程总路由demo/urls.py中添加子应用的路由数据

> ~~~python
>  urlpatterns = [
>       	# django默认包含的
>         url(r'^admin/', admin.site.urls),
>         # 配置子应用路由
>         url(r'^users/', include('users.urls')), 
>     ]
> ~~~

### 工程setting.py详解

~~~python
 
# __file__为当前路径 
# abspath(__file__)可返回绝对路径
#os.path.dirname(__file__) 当前目录的上一层目录
# 当前项目的根目录 
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
# 密钥
SECRET_KEY = '_1@b!66u-q(3l&qob&u15c^wg*7@@loiayc&m0ay@lpx!%7)42'
# 调试模式：True （开发阶段）
#         False (上线部署)
DEBUG = True

# 默认支持链接的ip: 127.0.0.1
ALLOWED_HOSTS = []

# 安装子应用
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # 安装子应用users
    'users.apps.UsersConfig',
]

# 中间件
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    # 'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# 项目的根路由
ROOT_URLCONF = 'zdemo.urls'

# 模板配置
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
# WSGI接口
WSGI_APPLICATION = 'zdemo.wsgi.application'


# 数据库配置
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}

# 用户密码
AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME':'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]

# 国际化（本地化）
# LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'zh-hans'

# TIME_ZONE = 'UTC' 默认英国格林威治时间
TIME_ZONE = 'Asia/Shanghai'
# 是否加载国际化支持机制。
USE_I18N = True

# Django的格式化系统可以在模板中使用当前_地区_特定的格式，来展示日期、时间和数字。也可以处理表单中输入的本地化。
# USE_L10N当它被开启时，访问相同内容的两个用户可能会看到以不同方式格式化的日期、时间和数字，这取决于它们的当前地区的格式。
USE_L10N = True

# USE_TZ开启后保证存储到数据库中的是 UTC 时间；
# 在函数之间传递时间参数时，确保时间已经转换成 UTC 时间；

USE_TZ = True



# 静态服务器配置
# 静态文件路径: 127.0.0.1:8000/static/xxx.png
# 静态文件路由的前缀:
STATIC_URL = '/static/'

# 静态文件的实际路径（存储的位置）
STATICFILES_DIRS = [os.path.join(BASE_DIR, "static_files")]
~~~

### reverse反解析

+ 作用:

  根据 `函数名称` 反向推理获取到 `url路由地址`。

+ 用途:

  > 当某一个url配置的地址发生变化时，页面上使用反向解析生成地址的位置不需要发生变化。

+ 方式一:

  ~~~python
  # index为视图函数
  # 能反向解析出访问视图index的路径
  # 但是这种方式只能解析出当前视图文件里的路由
  reverse(index)  #如 /users/index/
  ~~~

+ 方式二:

  > 能解析整个项目里的任何一个视图路由

  ~~~python
  #在总路由里使用namespace参数定义路由的命名空间
  url(r'^users/', include('users.urls', namespace='users')),
  ~~~

  ~~~python
  # 在定义普通路由时，可以使用name参数指明路由的名字
  urlpatterns = [
      url(r'^index/$', views.index, name='index'),
      url(r'^say', views.say, name='say'),]
  ~~~

  ~~~python
  #users是指总路由的别名  index是指子路由别名
  url = reverse('users:index')
  ~~~

# 请求与响应

### HTTP请求的传参方式

+  **URL路径**

  > 比如： /weather/beijing/2018，可以在url路由中用 `正则表达式` 提取；

+ **查询字符串**

  > 比如： url路径/?key1=value1&key2=value2

+ **请求体**

  > ​    1、表单数据form​
  >
  > ​    2、非表单: json、xml text ...

+ **请求头**

  > 请求头中可以携带参数，但是不常用

### HTTP请求的数据提取方式

+ **路由正则提取**

  > ~~~python
  > # 例子提取后面两个参数/weather/beijing/2018
  > /weather/(?P<city>[a-z]+)/(?P<year>[1-9]+)/
  > # 然后在视图路径中多传递两个参数
  > def index(request,citsy,year)
  > # request是必传的
  > ~~~

  

+ **request.GET** 

  > ~~~python
  > # 用于获取查询字符串
  > # 类型为QueryDict对象 QueryDict对象支持一键多值
  > 一键一值：request.GET.get("key")
  > 一键多值：request.GET.getlist("key")
  > ~~~

+ **request.POST**

  > ~~~python
  > # 用于获取表单数据
  > # 类型为QueryDict对象
  > 一键一值：request.POST.get("key")
  > 一键多值：request.POST.getlist("key")
  > ~~~

+ **request.body**

  > ~~~python
  > # 返回值为：二进制数据  bytes类型
  > # 是json类型
  >   bytes类型  --->  json字符串  --->  python字典
  > # 文本类型 
  >   bytes类型  --->  字符串
  > ~~~

+ **request.META**

  > ~~~python
  > # 字典类型
  > # 不常用
  > ~~~
  >
  
+ **补充**

  > ~~~python
  > # 能够通过请求体（body）传参数的 `请求方式`：除了GET请求外，其他都可以：POST PUT PATCH DELETE
  > ~~~

  > ~~~python
  > # 对于字典对象获取value两种方式：
  > 
  > # 方法一：当key不存在，就会报错
  > dict["key"] 
  > 
  > # 方法二：当key不存在，不会报错，使用默认值
  > dict.get("key", "默认值")
  > ~~~

### 服务器响应的方式

+ **HttpResponse**

  > ~~~python
  > HttpResponse(content=响应体, content_type=响应体数据类型, status=状态码)
  > ~~~

  > ~~~python
  > # 拓展：响应头可以直接将HttpResponse对象当做 `字典` 进行响应头键值对的设置
  > response = HttpResponse()
  > response['itcast'] = 'python  # 自定义响应头itcast, 值为python
  > ~~~

+ **JsonResponse**

  > HttpResonse子类
  >
  > 作用：若要返回json数据，可以使用JsonResponse来构造响应对象

  > ~~~python
  > # 方法：
  > return JsonResponse(python字典)
  > 
  > # 理解底层原理：
  > 	1.帮助我们将数据转换为json字符串	
  >     	2.默认设置响应头Content-Type为： application/json
  > ~~~

+ **重定向**

  > 作用：能够根据url地址跳转到另外一个网页 或者 视图函数

  > ~~~python
  > # 方法1： 跳转到黑马官网
  > redirect('http://www.itheima.com')
  > 
  > 
  > #方法2：跳转到某个视图函数
  > # 先通过反解析方法获取视图函数的url，再跳转
  > # 重定向路由前面必须加斜杠,在这里斜杠代表从根路由
  > url = reverse('函数别名')
  > redirect(url)
  > ~~~

+ **cookie**
  
  + cookie作用
    
    > ~~~python
    > 状态保持[缓存]，比如保持用户登录信息，以键值对方式保存用户信息到 `浏览器`。
    > ~~~
    
  + 特点
  
    > ~~~python
    > 1.Cookie是由服务器端生成，发送给客户端浏览器。
    > 2.浏览器会将Cookie的key/value保存。
    > 3.下次请求同一网站时就发送该Cookie给服务器（前提是浏览器设置为启用
    > 4.Cookie是基于域名安全的
    > ~~~
  
  + 应用
  
    > ~~~python
    > 1.设置cookie:
    > 响应对象.set_cookie(key, value, max_age=过期时长)
    > 
    > 2.获取cookie:
    > 请求对象.COOKIES.get(key, "")
    > 
    > 3.删除cookie:
    > response.delete_cookie('hello')
    > ~~~
  > response.delete_cookie('password')
  > # 注意!!!设置是从响应对象上设置的,获取是从请求对象中获取的
  > ~~~
  > 
  > ~~~
  
+ **Session**

  + Session作用

    > 状态保持[缓存]，比如保存用户登录信息，以键值对方式保存用户信息到 `服务器` 通过配置将内容存储到`redis数据库`。

  + 特点

    > ~~~python
    > 1.在服务器端进行状态保持的方案就是Session
    > 
    > 2.以字典的方式将数据保存到服务器端。
    > 
    > 3.session依赖于cookie。(最终会返回session_id,需要借助cookie告知浏览器)
    > ~~~

  +  应用

    > ~~~python
    > 1.设置session:
    > request.session["key"] = "value"
    >       
    > 2.获取session:
    > request.session.get("key", "")
    >     
    > 3.删除session:
    >  # 方法1 根据key清除value
    > del request.session["like"]
    >  # 方法2 清除所有数据
    > request.session.clear()
    > ~~~

  + session实现原理

    

  + 拓展

    > ~~~python
    > # 设置过期时长，默认是：2个星期过期
    > # 参数：过期的秒数
    > request.session.set_expiry(3600)
    > # 如果session 过期时间设置为None 表示使用默认的14天, 如果设置为0 代表关闭浏览器就过期
    > # 如果cookie 过期时间设置为None 表示关闭浏览器就过期, 如果设置为0, 代表直接删除
    > ~~~

+ **总结:**
  + session是基于cookie的
  + cookie是把数据保存到客户端
  + session是把数据保存到服务端
  + session其实就是一个字典,所有操作同字典

# 类视图

### 基本类视图实现

+ **作用**

  > ~~~python
  > 为了让一个视图对应的路径提供了多种不同HTTP请求方式的支持时.在Django中可以使用类来定义一个视图，称为类视图。
  > ~~~

+ **特点**

  > ~~~python
  > 1.定义类视图需要继承自Django提供的父类View
  > 2.在类视图中分别实现对应的：GET请求，POST请求...等方法处理不同业务逻辑
  > 3.配置路由时，使用类视图的as_view()方法来添加
  > ~~~

+ **优点**

  > ~~~python
  > 1.代码可读性好
  > 2.类视图相对于函数视图有更高的复用性
  > ~~~

+ **应用**

  + view.py

  > ~~~python
  > class RegisterView(View):
  >     """类视图：处理注册"""
  > 
  >     def get(self, request):
  >         """处理GET请求，返回注册页面"""
  >         return render(request, 'register.html')
  > 
  >     def post(self, request):
  >         """处理POST请求，实现注册逻辑"""
  >         return HttpResponse('这里实现注册逻辑')
  > ~~~

  + urls.py

  > ~~~python
  > urlpatterns = [
  >     # 视图函数：注册
  >     # url(r'^register/$', views.register, name='register'),
  >     # 类视图：注册
  >     url(r'^register/$', views.RegisterView.as_view(), name='register'),
  > ]
  > ~~~


### 类视图实现原理

> ~~~python
> # 代码有删减,只保留主要逻辑部分
> class View(object):
>     http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']
>     
>     @classonlymethod
>     def as_view(cls, **initkwargs):
>         def view(request, *args, **kwargs):
>             self = cls(**initkwargs)
>             if hasattr(self, 'get') and not hasattr(self, 'head'):
>                 self.head = self.get
>             self.request = request
>             self.args = args
>             self.kwargs = kwargs
>             return self.dispatch(request, *args, **kwargs) # 主要
>         return view #主要
>     
>     def dispatch(self, request, *args, **kwargs):
>         if request.method.lower() in self.http_method_names:  #主要
>             handler = getattr(self, request.method.lower(),  self.http_method_not_allowed)  #主要
>         else:
>             handler = self.http_method_not_allowed
>         return handler(request, *args, **kwargs)  #主要
> ~~~

![Django类视图](配置图片\Django类视图.png)

> ~~~python
> 1.类视图继承自view类
> 2.view里面有一个as_view()方法
> 3.as_view会调用view里面的dispatch方法
> 4.dispatch方法则根据传递过来的值,去判断请求方式.并把方法返回给view
> 5.as_view再把view给返回
> ~~~

### 类视图使用装饰器

+ 作用:

  > ~~~
  > 给类中每一个视图方法都可以额外添加功能
  > ~~~

+ 方式一

  > ~~~python
  > # 假定装饰器都已经定义好了.装饰器的名字是:my_decorate
  > urlpatterns = [
  > url(r'^register/$', my_decorator(views.RegisterView.as_view())),
  > ]   
  > ~~~

  > ~~~
  > #深度理解：
  > 	1.需要明白装饰器本质是函数，所以可以使用函数调用的方式使用装饰
  > 	2.参数一致的问题：
  > 	DemoView.as_view() ---> 返回：handler(request, *args, **kwargs)
  >     my_decorate装饰器内层函数：     wrapper(request, *args, **kwargs)，参数一致所有可以这么使用
  > ~~~

+ 方式二

  > ~~~python
  > #1. 为全部请求方法添加装饰器
  > @method_decorator(my_decorator, name='dispatch')
  > 
  > #2. 为特定请求方法添加装饰器
  > @method_decorator(my_decorator, name='get')
  > # PS:
  > """    
  > @method_decorator(my_decorator)
  >     def get(self, request)
  > """
  > ~~~

+ 总结:
  + 方式一写法虽然简单,但是可读性比较差
  + 方式一没有办法给单一请求方式装饰

# Mixin扩展类

> ~~~
> 总结：
> # 作用 : 类多继承 为类视图补充拓展功能， 实现代码复用。
> 
> 步骤：
> 1.使用面向对象多继承的特性，定义扩展类。 [定义Mixin扩展类]
> 2.在父类中定义想要向类视图补充的方法。 [实现拓展功能]
> 3.类视图继承这些扩展父类。 [多继承]
> ~~~

# 中间件

+ 作用

  > ~~~
  > 在Django处理视图的阶段 对请求对象&响应对象 进行拦截处理，补充额外功能，处理共性的功能。
  > ~~~

+ 中间件的定义

  > ~~~python
  > def my_middleware(get_response):
  >     # 此处编写的代码仅在Django第一次配置和初始化的时候执行一次。
  >     def middleware(request):
  >         # 此处编写的代码会在每个请求处理视图前被调用。
  >         response = get_response(request)
  >         # 此处编写的代码会在每个请求处理视图之后被调用。
  >         return response
  >     return middleware
  > ~~~

+ 中间件的使用

  > ~~~python
  > MIDDLEWARE = [ 'django.middleware.clickjacking.XFrameOptionsMiddleware',
  > 			   'users.middleware.my_middleware',  # 在setting中添加自己的中间件]
  > ~~~

+ 中间件执行顺序

  > ~~~
  > 中间件初始化的时候从下至上执行的.
  > ~~~

  > 在请求视图被处理**前**，中间件**由上至下**依次执行
  >
  > 在请求视图被处理**后**，中间件**由下至上**依次执行

  ![ä¸­é´ä»¶æ§è¡æµç¨](/Python29Django/django/images/%E4%B8%AD%E9%97%B4%E4%BB%B6%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B.png)

# Django自带模板

### 模板的使用流程

+ 1.在工程中创建模板目录templates
+ 2.在settings.py配置文件中修改TEMPLATES配置项的DIRS值：模板文件夹的路径
+ 3.在views文件中返回模板：  return render(request对象, 模板文件路径, 模板数据字典)

![Django模板的使用](D:\常用资料\笔记\2.7Typora_note\配置图片\Django模板的使用.png)

### 模板语法

+ **模板变量**

  > ~~~python
  > 变量名必须由字母、数字、下划线（不能以下划线开头）和点组成。
  > #基本语法
  > {{变量}}
  > ~~~

### 模板语句

+ **for循环：**

  > ~~~
  > {% for item in 列表 %}
  > 
  > 循环逻辑
  > {{forloop.counter}}表示当前是第几次循环，从1开始
  > {%empty%} 列表为空或不存在时执行此逻辑
  > 
  > {% endfor %}
  > ~~~

+ **if条件：**

  > ~~~
  > {% if ... %}
  > 逻辑1
  > {% elif ... %}
  > 逻辑2
  > {% else %}
  > 逻辑3
  > {% endif %}
  > ~~~

+ **比较运算符**

  > ~~~python
  > ==
  > !=
  > <
  > >
  > <=
  > >=
  > 注意：运算符左右两侧不能紧挨变量或常量，必须有空格。
  > {% if a == 1 %}  # 正确
  > ~~~

+ **逻辑运算符**

  > ~~~
  > and
  > or
  > not
  > ~~~

+ **例子**

  ~~~html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
      <h1>{{ city }}</h1>
      <h1>{{ adict }}</h1>
      <h1>{{ adict.name }}</h1>  注意字典的取值方法
      <h1>{{ alist }}</h1>  
      <h1>{{ alist.0 }}</h1>  注意列表的取值方法
  </body>
  </html>
  ~~~

  ~~~html
  {% if my_int >= 18 %}
      成年人
  
  {% elif my_int > 10 and my_int < 18 %}
      青少年
  
  {% else %}
      小屁孩
  
  {% endif %}
  ~~~

### 过滤器

+ 语法

  > ~~~
  > 变量|过滤器:参数
  > ~~~

+ 系统常见过滤器

  > ~~~
  > safe length default date
  > ~~~

+ 例子

  ~~~html
  // safe: 表示告诉模板，my_h1是安全的，可以渲染显示
  {{ my_h1 | safe }}
  
  
  {#length: 显示字符串的长度#}
  {{ my_str }}
  {{ my_str | length }}
  
  
  {#default：如果变量不存在，使用默认值#}
  {{ aa|default:"数据不存在"}
  ~~~

  

### 自定义过滤器

![Django模板自定义过滤器](配置图片\Django模板自定义过滤器.png)

### 模板继承

+ **作用**

  > 模板继承和类的继承含义是一样的，主要是为了提高代码重用，减轻开发人员的工作量。

+ **父模板**

  > 如果发现在多个模板中某些内容相同，那就应该把这段内容定义到父模板中。

  父 `block`

  > 标签block：用于在父模板中预留区域，留给子模板填充差异性的内容，名字不能相同

  > ~~~html
  > {% block 名称 %}
  > 预留区域，可以编写默认内容，也可以没有默认内容
  > {% endblock  名称 %}
  > ~~~

+ **子模板**

  继承

  > ```
  > {% extends "父模板路径"%}
  > ```

  子  `block`

  > 1.子模板可以继承父模板中的block里面的内容
  >
  > 2.子模板也可以重新定义block里面的内容
  >
  > 3.子模板既可以继承,也可以增加自己的东西

  > ```html
  > {% block 名称 %}
  > 实际填充内容
  > {{ block.super }}用于获取父模板中block的内容
  > {% endblock 名称 %}
  > ```

### 总结

+ 变量 过滤器一般直接使用{{}}
+ 其他的使用{% %}结构

# Django的ORM

### ORM

+ **作用**

  > ~~~python
  > 1.将定义数据库 `模型类 `--> 数据库表
  > 2.将定义数据库模型类中的 `属性` ---> 数据库表`字段`
  > 3.将模型对象的操作(save,delete,get) ---> 对应sql语句，并将执行结果提交到数据库
  > ~~~

+ **优点**

  > ~~~python
  > 1.只需要面向对象编程, 不需要面向数据库编写代码.
  > 2.实现了数据模型与数据库的解耦, 屏蔽了不同数据库操作上的差异.
  > ~~~

+ **确定**

  > ~~~python
  > 1.相比较直接使用SQL语句操作数据库,有性能损失.
  > 
  > 根据对象的操作转换成SQL语句,根据查询的结果转化成对象, 在映射过程中有性能损失.
  > ~~~

### 配置

+ **安装Mysql数据库驱动**

  > ~~~shell
  > pip install PyMySQL
  > ~~~

+ **将pymysql转换成MySQLdb**

  > ~~~python
  > # 在Django的工程同名子目录的__init__.py文件中添加如下语句
  > from pymysql import install_as_MySQLdb
  > install_as_MySQLdb()
  > ~~~

  > ~~~python
  > # 拓展
  > 
  > 一些框架默认仍然用的是MySQLdb，但是python3已经不支持MySQLdb，取而代之的是pymysql，因此运行的时候会报 
  > ImportError: No module named ‘MySQLdb’
  > 
  > 
  > 解决方案：将pymysql转换成MySQLdb
  > 
  > pymysql.install_as_MySQLdb()
  > ~~~

+ **修改setting数据库配置**

  > ~~~python
  > DATABASES = {
  >     'default': {
  >         'ENGINE': 'django.db.backends.mysql',
  >         'HOST': '127.0.0.1',  # 数据库主机
  >         'PORT': 3306,  # 数据库端口
  >         'USER': 'root',  # 数据库用户名
  >         'PASSWORD': 'mysql',  # 数据库用户密码
  >         'NAME': 'django_demo'  # 数据库名字
  >     }
  > }
  > ~~~

+ **在MySQL中创建数据库**

  > ~~~mysql
  > create database django_demo default charset=utf8;
  > ~~~

### 定义模型类

> ```python
> 属性=models.字段类型(选项)
> ```

![Djngo模型类定义](D:\常用资料\笔记\2.7Typora_note\配置图片\Djngo模型类定义.png)

+ **数据库表名**

+  **关于主键**

  > **django会为表创建自动增长的主键列**，每个模型只能有一个主键列，如果使用选项设置某属性为主键列后django不会再创建自动增长的主键列。
  >
  > 默认创建的主键列属性为id，可以使用pk代替，pk全拼为primary key

+ **属性命名限制**

  - 不能是python的保留关键字。

  - 不允许使用连续的下划线，这是由django的查询方式决定的。

  - 定义属性时需要指定字段类型，通过字段类型的参数指定选项，语法如下：

    ~~~
    属性=models.字段类型(选项)
    ~~~

    

+ **字段类型**

  | 类型             | 说明                                                         |
  | :--------------- | :----------------------------------------------------------- |
  | AutoField        | 自动增长的IntegerField，通常不用指定，不指定时Django会自动创建属性名为id的自动增长属性 |
  | BooleanField     | 布尔字段，值为True或False                                    |
  | NullBooleanField | 支持Null、True、False三种值                                  |
  | CharField        | 字符串，参数max_length表示最大字符个数                       |
  | TextField        | 大文本字段，一般超过4000个字符时使用                         |
  | IntegerField     | 整数                                                         |
  | DecimalField     | 十进制浮点数， 参数max_digits表示总位数， 参数decimal_places表示小数位数 |
  | FloatField       | 浮点数                                                       |
  | DateField        | 日期， 参数auto_now表示每次保存对象时，自动设置该字段为当前时间，用于"最后一次修改"的时间戳，它总是使用当前日期，默认为False； 参数auto_now_add表示当对象第一次被创建时自动设置当前时间，用于创建的时间戳，它总是使用当前日期，默认为False; 参数auto_now_add和auto_now是相互排斥的，组合将会发生错误 |
  | TimeField        | 时间，参数同DateField                                        |
  | DateTimeField    | 日期时间，参数同DateField                                    |
  | FileField        | 上传文件字段                                                 |
  | ImageField       | 继承于FileField，对上传的内容进行校验，确保是有效的图片      |

+ **选项**

  | 选项        | 说明                                                         |
  | :---------- | ------------------------------------------------------------ |
  | null        | 如果为True，表示允许为空，默认值是False                      |
  | blank       | 如果为True，则该字段允许为空白，默认值是False                |
  | db_column   | 字段的名称，如果未指定，则使用属性的名称                     |
  | db_index    | 若值为True, 则在表中会为此字段创建索引，默认值是False        |
  | default     | 默认                                                         |
  | primary_key | 若为True，则该字段会成为模型的主键字段，默认值是False，一般作为AutoField的选项使用 |
  | unique      | 如果为True, 这个字段在表中必须有唯一值，默认值是False        |

+ **外键**

  > ~~~python
  > 在设置外键时，需要通过on_delete选项指明主表删除数据时，对于外键引用表数据如何处理，
  > # 例子
  > hbook = models.ForeignKey(BookInfo, on_delete=models.CASCADE, verbose_name='图书')
  > ~~~

  | 外键类型        | 说明                                                         |
  | --------------- | ------------------------------------------------------------ |
  | **CASCADE**     | 级联，删除主表数据时连通一起删除外键表中数据                 |
  | **PROTECT**     | 保护，通过抛出**ProtectedError**异常，来阻止删除主表中被外键应用的数据 |
  | **SET_NULL**    | 设置为NULL，仅在该字段null=True允许为null时可用              |
  | **SET_DEFAULT** | 设置为默认值，仅在该字段设置了默认值时可用                   |
  | **SET()**       |                                                              |
  | **DO_NOTHING**  | 不做任何操作，如果数据库前置指明级联性，此选项会抛出**IntegrityError**异常 |

+ **迁移**

  + 生成迁移文件

  ~~~
  python manage.py makemigrations
  ~~~

  + 同步到数据库中

  ~~~
  python manage.py migrate
  ~~~

### 外键

> ~~~
> 1.外键一般定义在多的一方
> 2.django会自动为外键中一的一方设置一个模型类_set的方法，方便查出所有的外键对象
> 3.对于多的一方会生成一个模型类_id的属性，用于保存关联的主键的id
> 模型类.外键_id  返回关联主键的id
> 多模型类.外键  返回一的对象
> ~~~

# ORM数据操作

### 增删改

+ **增**

  > ~~~python
  > #  模型类.objects.create()
  > HeroInfo.objects.create(
  >     hname='沙悟净',
  >     hgender=0,
  >     hbook=book)
  > ~~~

+ **删**

  > ~~~python
  > # 模型类.objects.filter().delete()
  > HeroInfo.objects.filter(id=2).delete()
  > ~~~

+ **改**

  > ~~~python
  > # 使用模型类.objects.filter().update()
  > HeroInfo.objects.filter(id=2).update(name="唐僧")
  > ~~~

+ **通过模型类增删改**

  > ~~~python
  > # 增
  > book = BookInfo(
  >     btitle='西游记')
  > book.save()
  > # 删
  > #hero为模型类对象 
  > hero.delete()
  > # 改
  > hero.hname = '猪悟能'
  > hero.save()
  > ~~~

### 查

+ **基本查询** 

  | 方法  | 功能                         | 例子                       |
  | ----- | ---------------------------- | -------------------------- |
  | get   | 查询单一结果。               | BookInfo.objects.get(id=1) |
  | all   | 查询多个结果, 返回列表数据。 | BookInfo.objects.all()     |
  | count | 查询结果数量统计             | BookInfo.objects.count()   |

+ **过滤查询**

  > 语法: **属性名称__比较运算符=值**

  | 过滤器       | 功能           | 例子                       |
  | ------------ | -------------- | -------------------------- |
  | filter       | 过滤出多个结果 | BookInfo.objects.filter()  |
  | exclude      | 取反           | BookInfo.objects.exclude() |
  | **过滤条件** | **功能**       | **例子**                   |
  | contains     | 包含           | btitle__contains='传'      |
  | endswith     | 结尾           | btitle__endswith='部'      |
  | startswith   | 开始           | btitle__startswith='部'    |
  | isnull       | 判空           | btitle__isnull=False       |
  | in           | 属于           | id__in=[1,3,5]             |
  | gt           | 大于           | id__gt=3                   |
  | gte          | 大于等于       | id__gte=3                  |

  ~~~python
  #查询书名包含'传'的图书。
  BookInfo.objects.filter(btitle__contains='传')
  #查询书名不为空的图书。 
  BookInfo.objects.filter(btitle__isnull=False)
  #查询编号大于3的图书
  BookInfo.objects.filter(id__gt=3)
  #查询编号不等于3的图书 
  BookInfo.objects.exclude(id=3)
  ~~~

#### F和Q对象

+ **F对象**

  > 对比两个字段属性('变量')使用F对象

  > ~~~python
  > # 查询 阅读量 大于等于 评论量的 图书
  > BookInfo.objects.filter(bread__gte=F("bcomment"))
  > # 其中评论量是一个时刻改变的'变量'
  > 
  > # 查询 阅读量 大于2倍 评论量的 图书
  > BookInfo.objects.filter(bread__gt=F("bcomment") * 2)
  > ~~~

+ **Q对象**

  > 逻辑查询使用Q对象

  > ~~~python
  > # 查询阅读量大于20，并且编号小于3的图书
  > BookInfo.objects.filter(Q(bread_gt=20) & Q(id__lt=3))
  > 
  > # 查询阅读量大于20，或编号小于3的图书，只能使用Q对象实现
  > BookInfo.objects.filter(Q(bread__gt=20) | Q(id__lt=3) )
  > 
  > # 查询编号不等于3的图书 
  > BookInfo.objects.filter(~Q(id=3)) # 也可用exclude
  > ~~~

#### 聚合函数&排序

| 聚合函数 | 功能     | 例子                                      |
| -------- | -------- | ----------------------------------------- |
| Avg      | 平均     | BookInfo.objects.aggregate(Sum("bread"))  |
| Count    | 数量     | aggregate(Count("bread"))                 |
| Max      | 最大     | 基本同上                                  |
| Min      | 最小     |                                           |
| Sum      | 总数     |                                           |
| **排序** | **功能** | **例子**                                  |
| order_by | 升序排序 | BookInfo.objects.all().order_by('bread')  |
|          | 降序排序 | BookInfo.objects.all().order_by('-bread') |

#### 关联查询

+ 一查多

  > ~~~python
  > 一模型类.objects.filter(多的模型类小写__属性__比较运算符=值)
  > ~~~

  ~~~python
  # 以一方为条件查多
  book = BookInfo.objects.get(id=1)
  book.heroinfo_set.all()
  
  # 以一方为条件查多
  HeroInfo.objects.filter(hbook__btitle='天⻰⼋部')
  ~~~

  

+ 多查一

  > ~~~python
  > # 以多方为条件查一
  > hero = HeroInfo.objects.get(id=1)
  > hero.hbook
  > 
  > # 以多方为条件查一 
  > BookInfo.objects.filter(heroinfo__hcomment__contains='降⻰')
  > ~~~
  ~~~python
  # 案列：查询 书名为“天龙八部”的所有 英雄
  HeroInfo.objects.filter(hbook__btitle="天龙八部")
  
  # 案列：询图 书阅读量大于30的所有 英雄
  HeroInfo.objects.filter(hbook__bread__gt=30)
  ~~~

#### 查询集 QuerySet

+ **连续过滤**

  > HeroInfo.objects.all().filter(hname__isnull=False).filter(hgender=1)

+ **判断查询结果**

  > ~~~python
  > heros = HeroInfo.objects.exclude(hname__isnull=False)
  > heros.exists()
  > # 判断有没有查询到结果
  > ~~~

+ **查询集切片**

  > ~~~Python
  > # 不支持负数，左开右闭
  > qs = BookInfo.objects.all()[0:2]
  >~~~
  > 

#### 两大特性

+ **惰性执行**

  > ~~~python
  > books = BookInfo.objects.all()
  > # 不会执行sql语句 
  > # 只有访问books数据的时候，才执行sql语句
  > # !!! 只有查询集QuerySet才会有惰性执行
  > ~~~

+ **缓存**

  > ~~~python
  > book = BookInfo.objects.all()
  > [bk for bk in book] --> 发送sql语句
  > [bk for bk in book] --> 使用缓存
  > [bk for bk in book] ....
  > # 第一次会直接从数据库查询.但是第二次他就直接读取缓存
  > ~~~

#### 管理器Manager

### Admin站点

#### Admin站点配置

+ **在settings.py中设置语言和时区**

  > ```python
  > LANGUAGE_CODE = 'zh-hans' # 使用中国语言
  > TIME_ZONE = 'Asia/Shanghai' # 使用中国上海时间
  > ```

+ **创建超级管理员**

  > ~~~
  > python manage.py createsuperuser
  > ~~~

+ **Appconfig配置**

  > ~~~python
  > from django.apps import AppConfig
  > class BooktestConfig(AppConfig):
  > 	# AppConfig.name 属性表示这个配置类是加载到哪个应用的
  >     name = 'booktest'
  >     # AppConfig.verbose_name 此名字在Django提供的Admin管理站点中会显示
  >     verbose_name = '图书管理'
  > ~~~

+ **注册模型类**

  > ~~~python
  > # 打开子应用里面的/admin.py文件
  > from django.contrib import admin
  > from booktest.models import BookInfo,HeroInfo
  > # 模型类需要在应用中的admin.py文件里注册，才能在后台管理中看到
  > admin.site.register(BookInfo)
  > admin.site.register(HeroInfo)
  > ~~~

+ **调整站点信息**

  > ~~~
  > admin.site.site_header 设置网站页头
  > admin.site.site_title 设置页面标题
  > admin.site.index_title 设置首页标语
  > ~~~

![admin基本代码结构](D:\常用资料\笔记\2.7Typora_note\配置图片\admin基本代码结构.png)

+ 详细关于admin编辑需要自己去查阅



# Git管理

| 常用命令                                           | 功能                                                |
| -------------------------------------------------- | --------------------------------------------------- |
| git init                                           | 初始化一个git仓库                                   |
| git status                                         | 查看当前处于状态                                    |
| git add .                                          | 将工作区代码提交到暂存区                            |
| git commit -m "提交的备注"                         | 将`暂存区`代码提交到`本地仓库`                      |
| git push                                           | 将本地仓库推送至远程仓库                            |
| git pull                                           | 拉取远程仓库最新代码                                |
| git log/reflog                                     | 查看日志/详细日志                                   |
| git clone 远程仓库地址（https）                    | 克隆项目到本地                                      |
| git checkout -b 分支名称                           | 创建分支并切换分支                                  |
| git checkout 分支名称                              | 切换分支                                            |
| git checkout master<br />git merge dev             | 合并分支                                            |
| **不常用命令**                                     | **功能**                                            |
| git reset --hard HEAD^<br/>git reset --hard 版本号 | 版本回退（本地仓库）<br />几个^表示向前回退几个版本 |
| git reset HEAD -- 文件名<br/>git checkout 文件名   | 从暂存区撤销                                        |
| git checkout 文件名                                | 检出 （工作区撤销）                                 |
| git checkout master                                | 切换分支                                            |
| git branch                                         | 查看当前分支                                        |
| git rm                                             | 删除                                                |
| git tag -a 标签名 -m '标签描述'                    | 创建标签                                            |
| git push -u origin 标签名                          | 推送标签到远程仓库                                  |



### Git介绍

+ **为什么要进行源代码管理**
  + 方便多人协同开发
  + 方便版本控制

+ **Git操作流程**

  > ~~~
  > Git服务器 --> 本地仓库 --> 客户端 --> 本地仓库 --> Git服务器
  > ~~~

![GIT操作图解](配置图片\GIT操作图解.png)

+ **工作区和暂存区**

  ![工作区暂存区和仓库区](D:\常用资料\笔记\2.7Typora_note\配置图片\工作区暂存区和仓库区.png)

  +  **工作区**

    >  可以对文件进行增删改操作

  + **暂存区**

    > 操作完成小阶段的存储

  + **仓库区**

    > 个人开发的小阶段的完成
    > 可以进行版本控制

### Git的基本使用

+ **安装git**

  ```
    sudo apt-get install git
  ```

+ **创建项目**

  ~~~python
  # 创建项目文件夹test
  # 进入项目内 cd Desktop/test/
  git init
  # 初始化好之后,项目里面会有一个.git文件夹
  ~~~

+ **配置个人信息**

  ~~~shell
  git config user.name '张三'
  git config user.email 'zhangsan@163.com'
  # 配置信息存到 .git/config里面
  ~~~

  

+ **文件推送到仓库**

  ~~~shell
  # 添加修改过的文件到暂存区
  git add . 
  # 将暂存区文件提交到仓库区
  git commit -m '版本描述'
  ~~~

  ~~~shell
  # 修改过的文件直接推送到仓库
  git commit -am "版本描述"
  ~~~


# CSRF

> ~~~
> 跨域请求伪造
> ~~~

![CSRF攻击过程](D:\常用资料\笔记\2.7Typora_note\配置图片\CSRF攻击过程.png)

**解决方案**

> ~~~
> 每次c/s交互后在cookie以及表单体中设置一个token.
> 每次交过过来的数据,两者的token是否相等.
> 由于攻击者无法获取实时token所以无法通过后端认证
> ~~~

# DRF

# RESTful

> ```
> 一种网络接口数据交互的规范
> ```

### 前后端分离

- **分离**:后端只负责`数据处理`,前端负责页面处理
- **不分离**:后端负责`数据处理`,也负责`页面渲染`

### 接口

- **接口**

  > ```
  > 规定了如何发请求.
  > ```

- **网络接口的具体描述**

  - 请求方式: GET、POST、PUT、DELETE
  - 协议、域名、端口、**路径**: http://127.0.0.1:8000/book/
  - 请求参数: 
    - 路径参数: http://127.0.0.1:8000/book/100/
    - 查询字符串参数: http://127.0.0.1:8000/book/?name=weiwei&age=18
    - 请求头参数
    - 请求体参数: 表单
  - 响应数据: json格式

### RESTful序列化和反序列化

- **序列化**

  > ```
  > 对象到字典再到json
  > ```

- **反序列化**

  > ```
  > json到字典再到对象
  > ```

# 定义序列化器类



> ```python
> # 创建BookInfo序列化器
> class BookInfoSerializer(serializers.Serializer):
> """图书数据序列化器"""
> id = serializers.IntegerField(label='ID', read_only=True)
> btitle = serializers.CharField(label='名称', max_length=20)
> bpub_date = serializers.DateField(label='发布日期', required=True)
> bread = serializers.IntegerField(label='阅读量', required=False)
> bcomment = serializers.IntegerField(label='评论量', required=False)
> image = serializers.ImageField(label='图片', required=False)
> 
> # many=True告诉序列化器，heroinfo_set是多条数据对象
> # 序列化成主键
> # heroinfo_set = serializers.PrimaryKeyRelatedField(many=True, read_only=True)
> # 默认会添加read_only=True
> # 序列化成关联对象__str__返回的结果
> # 如果是多条数据对象也需要many=True
> # heroinfo_set = serializers.StringRelatedField(many=True)
> # heroinfo_set = HeroInfoSerializer1(many=True)
> ```

### 字段与选项

**常用字段类型**：

| 字段                    | 字段构造方式                                                 |
| ----------------------- | ------------------------------------------------------------ |
| **BooleanField**        | BooleanField()                                               |
| **NullBooleanField**    | NullBooleanField()                                           |
| **CharField**           | CharField(max_length=None, min_length=None, allow_blank=False, trim_whitespace=True) |
| **EmailField**          | EmailField(max_length=None, min_length=None, allow_blank=False) |
| **RegexField**          | RegexField(regex, max_length=None, min_length=None, allow_blank=False) |
| **SlugField**           | SlugField(max*length=50, min_length=None, allow_blank=False) 正则字段，验证正则模式 [a-zA-Z0-9*-]+ |
| **URLField**            | URLField(max_length=200, min_length=None, allow_blank=False) |
| **UUIDField**           | UUIDField(format='hex_verbose')  format:  1) `'hex_verbose'` 如`"5ce0e9a5-5ffa-654b-cee0-1238041fb31a"`  2） `'hex'` 如 `"5ce0e9a55ffa654bcee01238041fb31a"`  3）`'int'` - 如: `"123456789012312313134124512351145145114"`  4）`'urn'` 如: `"urn:uuid:5ce0e9a5-5ffa-654b-cee0-1238041fb31a"` |
| **IPAddressField**      | IPAddressField(protocol='both', unpack_ipv4=False, **options) |
| **IntegerField**        | IntegerField(max_value=None, min_value=None)                 |
| **FloatField**          | FloatField(max_value=None, min_value=None)                   |
| **DecimalField**        | DecimalField(max_digits, decimal_places, coerce_to_string=None, max_value=None, min_value=None) max_digits: 最多位数 decimal_palces: 小数点位置 |
| **DateTimeField**       | DateTimeField(format=api_settings.DATETIME_FORMAT, input_formats=None) |
| **DateField**           | DateField(format=api_settings.DATE_FORMAT, input_formats=None) |
| **TimeField**           | TimeField(format=api_settings.TIME_FORMAT, input_formats=None) |
| **DurationField**       | DurationField()                                              |
| **ChoiceField**         | ChoiceField(choices) choices与Django的用法相同               |
| **MultipleChoiceField** | MultipleChoiceField(choices)                                 |
| **FileField**           | FileField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ImageField**          | ImageField(max_length=None, allow_empty_file=False, use_url=UPLOADED_FILES_USE_URL) |
| **ListField**           | ListField(child=, min_length=None, max_length=None)          |
| **DictField**           | DictField(child=)                                            |

**选项参数：**

| 参数名称            | 作用             |
| ------------------- | ---------------- |
| **max_length**      | 最大长度         |
| **min_lenght**      | 最小长度         |
| **allow_blank**     | 是否允许为空     |
| **trim_whitespace** | 是否截断空白字符 |
| **max_value**       | 最小值           |
| **min_value**       | 最大值           |

**通用参数**：

| 参数名称           | 说明                                          |
| ------------------ | --------------------------------------------- |
| **read_only**      | 表明该字段仅用于序列化输出，默认False         |
| **write_only**     | 表明该字段仅用于反序列化输入，默认False       |
| **required**       | 表明该字段在反序列化时必须输入，默认True      |
| **default**        | 反序列化时使用的默认值                        |
| **allow_null**     | 表明该字段是否允许传入None，默认False         |
| **validators**     | 该字段使用的验证器                            |
| **error_messages** | 包含错误编号与错误信息的字典                  |
| **label**          | 用于HTML展示API页面时，显示的字段名称         |
| **help_text**      | 用于HTML展示API页面时，显示的字段帮助提示信息 |

### 序列化器的验证

- **第一种验证**

  > ```
  > 此方式复用性较强,可对多个字段进行统一验证
  > ```

  ![序列化器第一种验证方式](配置图片\序列化器第一种验证方式.png)

- **第二种验证**

  > ```
  > 复用性较差,每个字段都需要定义一个函数
  > ```

  ![序列化器第二种验证方式](配置图片\序列化器第二种验证方式.png)



- **第三种验证**

  > ```
  > 综合性强,能在一个函数内,对该对象内所有字段进行验证
  > ```

  ![序列化器第三种验证方式](配置图片\序列化器第三种验证方式.png)

### 序列化器和反序列化器

![序列化和反序列化](配置图片\序列化和反序列化.png)

### 关联对象嵌套序列化

#### PrimaryKeyRelatedField

```python
class HeroInfoSerializer(serializers.Serializer):
   .。。。。。。。。。。。。。。。。。。。。。。。。
# 此字段将被序列化为关联对象的主键
hbook = serializers.PrimaryKeyRelatedField(label='图书', read_only=True)
# 指明字段时需要包含read_only=True或者queryset参数：

```

**效果**

```
from booktest.serializers import HeroInfoSerializer
from booktest.models import HeroInfo
hero = HeroInfo.objects.get(id=6)
serializer = HeroInfoSerializer(hero)
serializer.data
# {'id': 6, 'hname': '乔峰', 'hgender': 1, 'hcomment': '降龙十八掌', 'hbook': 2}

```



#### StringRelatedField

```python
hbook = serializers.StringRelatedField(label='图书')

```

**效果**

```
{'id': 6, 'hname': '乔峰', 'hgender': 1, 'hcomment': '降龙十八掌', 'hbook': '天龙八部'}

```



**使用关联对象的序列化器**

```
# 使用关联对象的序列化器 BookInfoSerializer为另个一序列化器
hbook = BookInfoSerializer()

```

```
{'id': 6, 'hname': '乔峰', 'hgender': 1, 'hcomment': '降龙十八掌', 'hbook': OrderedDict([('id', 2), ('btitle', '天龙八部')te', '1986-07-24'), ('bread', 36), ('bcomment', 40), ('image', None)])}

```

#### 其他

**many**

> 不管在定义序列化器还是使用时，只要时查询集都需要加many=True
>
> ```python
> heroinfo_set = serializers.PrimaryKeyRelatedField(read_only=True, many=True)  # 新增
> /////////////////////////////////////////
> serializer = BookInfoSerializer(book_qs, many=True)
> 
> ```

# 视图

### View

> ```
> 对不同的请求，进行了分发
> dispatch
> 
> ```

```python
class BookView(View):
    # 获得单一数据
    # /books/(?P<pk>\d+)/
    # 请求参数：pk
    # 响应数据：json
    def get(self, request, pk):
        # 1、获得单一对象
        book = BookInfo.objects.get(pk=pk)
        # 2、把对象，组成成字典
        book_dict = {
            'btitle': book.btitle,
            'bpub_date': book.bpub_date,
            'bread': book.bread,
            'bcomment': book.bcomment,
            'is_delete': book.is_delete,
            'image': book.image.url
        }
        # 3、把字典组成只json返回
        return JsonResponse(book_dict)

```

```
class View(object):
    """
    Intentionally simple parent class for all views. Only implements
    dispatch-by-method and simple sanity checking.
    """

    http_method_names = ['get', 'post', 'put', 'patch', 'delete', 'head', 'options', 'trace']

    def __init__(self, **kwargs):
        """
        Constructor. Called in the URLconf; can contain helpful extra
        keyword arguments, and other things.
        """
        # Go through keyword arguments, and either save their values to our
        # instance, or raise an error.
        for key, value in six.iteritems(kwargs):
            setattr(self, key, value)

    @classonlymethod
    def as_view(cls, **initkwargs):
        """
        Main entry point for a request-response process.
        """
        for key in initkwargs:
            if key in cls.http_method_names:
                raise TypeError("You tried to pass in the %s method name as a "
                                "keyword argument to %s(). Don't do that."
                                % (key, cls.__name__))
            if not hasattr(cls, key):
                raise TypeError("%s() received an invalid keyword %r. as_view "
                                "only accepts arguments that are already "
                                "attributes of the class." % (cls.__name__, key))

        def view(request, *args, **kwargs):
            self = cls(**initkwargs)
            if hasattr(self, 'get') and not hasattr(self, 'head'):
                self.head = self.get
            self.request = request
            self.args = args
            self.kwargs = kwargs
            return self.dispatch(request, *args, **kwargs)
        view.view_class = cls
        view.view_initkwargs = initkwargs

        # take name and docstring from class
        update_wrapper(view, cls, updated=())

        # and possible attributes set by decorators
        # like csrf_exempt from dispatch
        update_wrapper(view, cls.dispatch, assigned=())
        return view


```



### APIView

> ```
> 继承自View
> 请求
>  把请求过来的数据处理成字典,类型为QueryDict
>  查询字符串获取
>      request.query_params
>  请求体内获取
>      request.data
> 响应
> 	提供了Response
> 	bs.data就可以获得字典
> 其它
> 	提供了身份认证，权限管理，流量控制
> 
> ```

```python
class BookAPIView(APIView):

    # 获得单一书本信息
    # GET
    # /books/(?P<pk>\d+)/
    # 请求参数：pk
    # 响应数据：json
    def get(self, request, pk):
        # 1、获得单一的书本对象
        book = BookInfo.objects.get(pk=pk)
        # 2、构建序列化器对象
        bs = BookInfoModelSerializer(book)
        # 3、序列化返回
        return Response(bs.data, status=HTTP_200_OK)

```

**源码**

```python
class APIView(View):

    # The following policies may be set at either globally, or per-view.
    renderer_classes = api_settings.DEFAULT_RENDERER_CLASSES
    parser_classes = api_settings.DEFAULT_PARSER_CLASSES
    authentication_classes = api_settings.DEFAULT_AUTHENTICATION_CLASSES
    throttle_classes = api_settings.DEFAULT_THROTTLE_CLASSES
    permission_classes = api_settings.DEFAULT_PERMISSION_CLASSES
    content_negotiation_class = api_settings.DEFAULT_CONTENT_NEGOTIATION_CLASS
    metadata_class = api_settings.DEFAULT_METADATA_CLASS
    versioning_class = api_settings.DEFAULT_VERSIONING_CLASS

    # Allow dependency injection of other settings to make testing easier.
    settings = api_settings

    schema = DefaultSchema()

    @classmethod
    def as_view(cls, **initkwargs):
        """
        Store the original class on the view function.

        This allows us to discover information about the view when we do URL
        reverse lookups.  Used for breadcrumb generation.
        """
        if isinstance(getattr(cls, 'queryset', None), models.query.QuerySet):
            def force_evaluation():
                raise RuntimeError(
                    'Do not evaluate the `.queryset` attribute directly, '
                    'as the result will be cached and reused between requests. '
                    'Use `.all()` or call `.get_queryset()` instead.'
                )
            cls.queryset._fetch_all = force_evaluation

        view = super().as_view(**initkwargs)
        view.cls = cls
        view.initkwargs = initkwargs

        # Note: session based authentication is explicitly CSRF validated,
        # all other authentication is CSRF exempt.
        return csrf_exempt(view)

```





### View跟APIView的区别

![APIView跟View的区别](配置图片\APIView跟View的区别.png)

### GenericAPIView

> ```
> 继承自APIView
> 对数据获取，以及序列化器使用方面做了进一步封装
> 
> ```

```python
class BooksAPIView(GenericAPIView):
    # 指明当前视图，默认所处理的数据集
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoModelSerializer
    def get(self, request):
        book_queryset = self.get_queryset()
        bs = self.get_serializer(instance=book_queryset, many=True)
        return Response(bs.data)
    def post(self, request):
        bs = self.get_serializer(data=request.data)
        bs.is_valid(raise_exception=True)
        bs.save()
        return Response(bs.data)

```

**源码**

```python
class GenericAPIView(views.APIView):
    """
    Base class for all other generic views.
    """
    # You'll need to either set these attributes,
    # or override `get_queryset()`/`get_serializer_class()`.
    # If you are overriding a view method, it is important that you call
    # `get_queryset()` instead of accessing the `queryset` property directly,
    # as `queryset` will get evaluated only once, and those results are cached
    # for all subsequent requests.
    queryset = None
    serializer_class = None

    # If you want to use object lookups other than pk, set 'lookup_field'.
    # For more complex lookup requirements override `get_object()`.
    lookup_field = 'pk'
    lookup_url_kwarg = None

    # 查询集过滤类
    filter_backends = api_settings.DEFAULT_FILTER_BACKENDS

    # 查询集分页控制类
    pagination_class = api_settings.DEFAULT_PAGINATION_CLASS

    def get_queryset(self):
        """列表视图与详情视图通用，返回查询集"""
    def get_object(self):
        """获取单一的数据对象"""
    def get_serializer(self, *args, **kwargs):
        """返回序列化器对象，被其他视图或扩展类使用"""
    def get_serializer_class(self):
        """返回序列化器类"""
    def get_serializer_context(self):
    def filter_queryset(self, queryset):

```



### Mixin类

> ```
> 一共五个Mixin类都继承object。分别对应增，删，改，查一个，查多个
> 对数据获取以及甚至响应都做了进一步封装。但是需要配合GenericAPIView类使用
> 
> ```

```python
class BooksAPIView(ListModelMixin, CreateModelMixin, GenericAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoModelSerializer

    # 获得多条数据
    # GET
    # /books/
    def get(self, request):
        return self.list(request)

    # 新建单一数据
    # POST
    # /books/
    def post(self, request):
        return self.create(request)

```

**源码**

```python
class ListModelMixin:
    """
    List a queryset.
    """
    def list(self, request, *args, **kwargs):
        queryset = self.filter_queryset(self.get_queryset())

        page = self.paginate_queryset(queryset)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)

        serializer = self.get_serializer(queryset, many=True)
        return Response(serializer.data)


```



### Mixin子类

> ```
> CreateAPIView
> ListAPIView
> RetireveAPIView
> DestoryAPIView
> UpdateAPIView
> RetrieveUpdateAPIView
> RetrieveUpdateDestoryAPIView
> 基本本质只是同时继承了ModelMixin，GenericAPIView，好写继承类的时候简便一点。没有增设新功能
> 
> ```

**源码**

```python
class CreateAPIView(mixins.CreateModelMixin,
                    GenericAPIView):
    """
    Concrete view for creating a model instance.
    """
    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)

```



### ModelViewSet

> ```
> 只是同时继承了五个Mixin以及GenericViewSet类
> 
> 
> ```

**源码**

```python
class ModelViewSet(mixins.CreateModelMixin,
                   mixins.RetrieveModelMixin,
                   mixins.UpdateModelMixin,
                   mixins.DestroyModelMixin,
                   mixins.ListModelMixin,
                   GenericViewSet):
    """
    A viewset that provides default `create()`, `retrieve()`, `update()`,
    `partial_update()`, `destroy()` and `list()` actions.
    """
    pass

```



### GenericViewSet 

```
继承自ViewSetMixin，GenericAPIView

```

**源码**

```python
class GenericViewSet(ViewSetMixin, generics.GenericAPIView):
    """
    The GenericViewSet class does not provide any actions by default,
    but does include the base set of generic view behavior, such as
    the `get_object` and `get_queryset` methods.
    """
    pass

```



### ViewSetMixin

> ```
> 不继承View
> 重写了as_view()方法
> 
> ```

**源码**

```python
class ViewSetMixin:
    """
    This is the magic.

    Overrides `.as_view()` so that it takes an `actions` keyword that performs
    the binding of HTTP methods to actions on the Resource.

    For example, to create a concrete view binding the 'GET' and 'POST' methods
    to the 'list' and 'create' actions...

    view = MyViewSet.as_view({'get': 'list', 'post': 'create'})
    """

    @classonlymethod
    def as_view(cls, actions=None, **initkwargs):

```

### ViewSet

> ```
> 继承自ViewSetMixin，以及APIView
> 
> ```

**源码**

```python
class ViewSet(ViewSetMixin, views.APIView):
    """
    The base ViewSet class does not provide any actions by default.
    """
    pass


```

### 视图关系图

<img src="配置图片\DRF视图关系.jpg" alt="DRF视图关系" style="zoom:200%;" />

### 视图集

```
视图集都基本APIView的基本功能，同时增加了对路由的处理。

methods: 该action支持的请求方式，列表传递
detail: 表示是action中要处理的是否是视图资源的对象（即是否通过url路径获取主键）
            True 表示使用通过URL获取的主键对应的数据对象
            False 表示不使用URL获取主键

```

**action属性**

```
def get_serializer_class(self):
    if self.action == 'create':
        return OrderCommitSerializer
    else:
        return OrderDataSerializer

```

**action装饰器**

```python
from rest_framework import mixins
from rest_framework.viewsets import GenericViewSet
from rest_framework.decorators import action

class BookInfoViewSet(mixins.ListModelMixin, mixins.RetrieveModelMixin, GenericViewSet):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer

    # detail为False 表示不需要处理具体的BookInfo对象
    #处理以下路由:
    #url(r'^books/latest/$', views.BookInfoViewSet.as_view({'get': 'latest'})),
    @action(methods=['get'], detail=False)
    def latest(self, request):
        """
        返回最新的图书信息
        """
        book = BookInfo.objects.latest('id')
        serializer = self.get_serializer(book)
        return Response(serializer.data)

    # detail为True，表示要处理具体与pk主键对应的BookInfo对象
    #处理以下路由:
    # url(r'^books/(?P<pk>\d+)/read/$', views.BookInfoViewSet.as_view({'put': 'read'})),
    @action(methods=['put'], detail=True)
    def read(self, request, pk):
        """
        修改图书的阅读量数据
        """
        book = self.get_object()
        book.bread = request.data.get('read')
        book.save()
        serializer = self.get_serializer(book)
        return Response(serializer.data)

```

```
urlpatterns = [
    url(r'^books/$', views.BookInfoViewSet.as_view({'get': 'list'})),
    url(r'^books/latest/$', views.BookInfoViewSet.as_view({'get': 'latest'})),
    url(r'^books/(?P<pk>\d+)/$', views.BookInfoViewSet.as_view({'get': 'retrieve'})),
    url(r'^books/(?P<pk>\d+)/read/$', views.BookInfoViewSet.as_view({'put': 'read'})),
]

```

### DRF路由

> ```
> REST framework提供了两个router
> 
> ```

- **SimpleRouter**
- **DefaultRouter**

**使用方法**

```
from rest_framework import routers

router = routers.SimpleRouter()
router.register(r'books', BookInfoViewSet, base_name='book')

```

**添加到路由的方式**

```python
urlpatterns = [
    ...
]
urlpatterns += router.urls
#DefaultRouter与SimpleRouter的区别是，DefaultRouter会多附带一个默认的API根视图，返回一个包含所有列表视图的超链接响应数据。

```

**视图集中包含附加action的**

> ```
> @action(methods=['get'], detail=False)
>  def latest(self, request):
>      ...
> 
>  @action(methods=['put'], detail=True)
>  def read(self, request, pk):
>      ...
> 
> ```
>
> 此视图集会形成的路由：
>
> ^books/latest/$    name: book-latest
> ^books/{pk}/read/$  name: book-read

# 其他功能

### 认证Authentication

**配置文件中设置全局认证**

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.BasicAuthentication',   # 基本认证
        'rest_framework.authentication.SessionAuthentication',  # session认证
    )
}

```

在视图中通过设置authentication_classess属性来局部认证

```python
from rest_framework.authentication import SessionAuthentication, BasicAuthentication
from rest_framework.views import APIView

class ExampleView(APIView):
    authentication_classes = (SessionAuthentication, BasicAuthentication)
    ...

```

### 权限Permissions

配置文件中设置全局权限管理

```python
REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    )
}

```

在视图中通过设置permission_classes属性来局部权限管理

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework.views import APIView

class ExampleView(APIView):
    permission_classes = (IsAuthenticated,)
    ...

```

> ```
> AllowAny 允许所有用户
> IsAuthenticated 仅通过认证的用户
> IsAdminUser 仅管理员用户
> IsAuthenticatedOrReadOnly 认证的用户可以完全操作，否则只能get读取
> 
> ```
>
> - 在执行视图的dispatch()方法前，会先进行视图访问权限的判断
> - 在通过get_object()获取具体对象时，会进行对象访问权限的判断

### 限流Throttling

**以在配置文件中，使用`DEFAULT_THROTTLE_CLASSES` 和 `DEFAULT_THROTTLE_RATES`进行全局配置，**

> ```
> REST_FRAMEWORK = {
>  'DEFAULT_THROTTLE_CLASSES': (
>      'rest_framework.throttling.AnonRateThrottle',
>      'rest_framework.throttling.UserRateThrottle'
>  ),
>  'DEFAULT_THROTTLE_RATES': {
>      'anon': '100/day',
>      'user': '1000/day'
>  }
> }
> 
> ```
>
> `DEFAULT_THROTTLE_RATES` 可以使用 `second`, `minute`, `hour` 或`day`来指明周期。

**也可以在具体视图中通过throttle_classess属性来配置，如**

> ```
> from rest_framework.throttling import UserRateThrottle
> from rest_framework.views import APIView
> 
> class ExampleView(APIView):
>  throttle_classes = (UserRateThrottle,)
>  ...
> 
> ```
>
> 1） AnonRateThrottle
>
> 限制所有匿名未认证用户，使用IP区分用户。
>
> 使用`DEFAULT_THROTTLE_RATES['anon']` 来设置频次
>
> 2）UserRateThrottle
>
> 限制认证用户，使用User id 来区分。
>
> 使用`DEFAULT_THROTTLE_RATES['user']` 来设置频次
>
> 3）ScopedRateThrottle
>
> 限制用户对于每个视图的访问频次，使用ip或user id

**实例**

> ```python
> class ContactListView(APIView):
>  # 自定义指定限流规则
>  throttle_scope = 'contacts'
>  ...
> 
> class ContactDetailView(APIView):
>  # 自定义指定限流规则
>  throttle_scope = 'contacts'
>  ...
> 
> class UploadView(APIView):
> 	# 自定义指定限流规则
>  throttle_scope = 'uploads'
>  ...
> 
> ```
>
> REST_FRAMEWORK = {
>  'DEFAULT_THROTTLE_CLASSES': (
>      'rest_framework.throttling.ScopedRateThrottle',
>  ),
>  'DEFAULT_THROTTLE_RATES': {
>      'contacts': '1000/day',
>      'uploads': '20/day'
>  }
> }

### 过滤Filtering

对于列表数据可能需要根据字段进行过滤，我们可以通过添加django-fitlter扩展来增强支持。

```shel
pip insall django-filter

```

在配置文件中增加过滤后端的设置：

```python
INSTALLED_APPS = [
    ...
    'django_filters',  # 需要注册应用，
]

REST_FRAMEWORK = {
    'DEFAULT_FILTER_BACKENDS': ('django_filters.rest_framework.DjangoFilterBackend',)
}

```

在视图中添加filter_fields属性，指定可以过滤的字段

```python
class BookListView(ListAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
    filter_fields = ('btitle', 'bread')

# 127.0.0.1:8000/books/?btitle=西游记

```

### 排序

在类视图中设置filter_backends，使用`rest_framework.filters.OrderingFilter`过滤器，REST framework会在请求的查询字符串参数中检查是否包含了ordering参数，如果包含了ordering参数，则按照ordering参数指明的排序字段对数据集进行排序。

前端可以传递的ordering参数的可选字段值需要在ordering_fields中指明。

示例：

```python
class BookListView(ListAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
    filter_backends = [OrderingFilter]
    ordering_fields = ('id', 'bread', 'bpub_date')

# 127.0.0.1:8000/books/?ordering=-bread

```

### 分页Pagination

REST framework提供了分页的支持。

我们可以在配置文件中设置全局的分页方式，如：

```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS':  'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 100  # 每页数目
}

```

也可通过自定义Pagination类，来为视图添加不同分页行为。在视图中通过`pagination_class`属性来指明。

```python
class LargeResultsSetPagination(PageNumberPagination):
    page_size = 1000
    page_size_query_param = 'page_size'
    max_page_size = 10000
class BookDetailView(RetrieveAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
    pagination_class = LargeResultsSetPagination

```

**注意：如果在视图内关闭分页功能，只需在视图内设置**

```python
pagination_class = None

```

**可选分页器**

1） **PageNumberPagination**

前端访问网址形式：

```http
GET  http://api.example.org/books/?page=4

```

可以在子类中定义的属性：

- page_size 每页数目
- page_query_param 前端发送的页数关键字名，默认为"page"
- page_size_query_param 前端发送的每页数目关键字名，默认为None
- max_page_size 前端最多能设置的每页数量

```python
from rest_framework.pagination import PageNumberPagination

class StandardPageNumberPagination(PageNumberPagination):
    page_size_query_param = 'page_size'
    max_page_size = 10

class BookListView(ListAPIView):
    queryset = BookInfo.objects.all().order_by('id')
    serializer_class = BookInfoSerializer
    pagination_class = StandardPageNumberPagination

# 127.0.0.1/books/?page=1&page_size=2

```

> ```
> 如果需要改变分页后返回的数据结果，需要改写PageNumberPagination类下面的get_paginated_response防范
> 
> ```
>
> ```
> class Mypage(PageNumberPagination):
>  page_size = 3
>  page_query_param = "page"
>  page_size_query_param = "pagesize"
>  max_page_size = 10
> 
> 
>  def get_paginated_response(self,data):
>      return Response({
>          "counts": self.page.paginator.count,
>          "lists": data,
>          "page": self.page.number,
>          "pages": self.page.paginator.num_pages,
>          "pagesize": self.page_size,
>      })
> 
> ```

2）**LimitOffsetPagination**

### 异常处理 Exceptions

REST framework提供了异常处理，我们可以自定义异常处理函数。

```python
from rest_framework.views import exception_handler

def custom_exception_handler(exc, context):
    # 先调用REST framework默认的异常处理方法获得标准错误响应对象
    response = exception_handler(exc, context)

    # 在此处补充自定义的异常处理
    if response is not None:
        response.data['status_code'] = response.status_code

    return response

```

在配置文件中声明自定义的异常处理

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'my_project.my_app.utils.custom_exception_handler'
}

```

如果未声明，会采用默认的方式，如下

```python
REST_FRAMEWORK = {
    'EXCEPTION_HANDLER': 'rest_framework.views.exception_handler'
}

```

例如：

补充上处理关于数据库的异常

```python
from rest_framework.views import exception_handler as drf_exception_handler
from rest_framework import status
from django.db import DatabaseError

def exception_handler(exc, context):
    response = drf_exception_handler(exc, context)

    if response is None:
        view = context['view']
        if isinstance(exc, DatabaseError):
            print('[%s]: %s' % (view, exc))
            response = Response({'detail': '服务器内部错误'}, status=status.HTTP_507_INSUFFICIENT_STORAGE)

    return response

```

### REST framework定义的异常

- APIException 所有异常的父类
- ParseError 解析错误
- AuthenticationFailed 认证失败
- NotAuthenticated 尚未认证
- PermissionDenied 权限决绝
- NotFound 未找到
- MethodNotAllowed 请求方式不支持
- NotAcceptable 要获取的数据格式不支持
- Throttled 超过限流次数
- ValidationError 校验失败

### 自动生成接口文档

**大集合**

```
from rest_framework.viewsets import ModelViewSet,ViewSet
from rest_framework.response import Response
from rest_framework.decorators import action


from .models import *
from .serializers import *



from rest_framework.authentication import SessionAuthentication,BaseAuthentication
from rest_framework.permissions import IsAuthenticated,IsAdminUser,AllowAny,IsAuthenticatedOrReadOnly
from rest_framework.filters import OrderingFilter
from rest_framework.pagination import PageNumberPagination,LimitOffsetPagination

from django_filters import FilterSet



class MyPage(PageNumberPagination):
    # 自定义分页器
    page_size = 3
    page_query_param = "page" # /books/?page=1
    page_size_query_param = "pagesize" # /books/?page=1&pagesize=2
    max_page_size = 10


class MyLimit(LimitOffsetPagination):
    offset_query_param = "offset" # books/?offset=3
    limit_query_param = "limit" # books/?offset=3&limit=2
    max_limit = 10
    # default_limit = 3



# 自定义规则
class MyFilterSet(FilterSet):
    # 针对哪个模型类
    # 指明字段
    class Meta:
        model = BookInfo
        # fields = ['bread', 'bcomment']
        fields = {
            # key: 过滤依据字段； value：过滤规则
            "btitle": ['exact', 'contains', 'startswith', 'endswith']
        }



# 原则：一类资源BookInfo，我们应该尽量用一个视图类去处理
class BooksAPIView(ModelViewSet):
    """
    这个视图接口是处理书本信息

    list: 获取多条书本
    create: 新建一本书
    """


    queryset = BookInfo.objects.all()
    serializer_class = BookInfoModelSerializer

    # 局部的身份认证配置：当前视图接收请求的时候会根据authentication_classes指明的认证器去认证
    # 局部配置会覆盖全局
    # authentication_classes = [SessionAuthentication, BasicAuthentication]

    # 局部配置权限
    # permission_classes = [IsAdminUser]

    # 局部配置限流
    # throttle_classes = [AnonRateThrottle, UserRateThrottle]

    # 自定义限流规则
    # throttle_scope = 'upload'

    # 指明过滤的依据
    # filterset_fields = ['bread', 'bcomment']

    # 自定义规则
    # filterset_class = MyFilterSet

    # 指明过滤后端
    # filter_backends = [OrderingFilter]
    # ordering_fields = ['bread', 'bcomment']


    # 分页局部配置
    pagination_class = MyLimit


    # 返回最新发布的书本信息
    # GET
    # /books/last/
    @action(methods=['get'], detail=False)# , url_path="aaa")
    def last(self, request):

        # 在一个视图中如何获取，此次请求的用户对象呢？
        # request.user: 此次请求的用户对象，如果身份认证未通过为None


        a = 10/0 # 抛出ZeroDivisionError，内建异常

        # 1、过滤最新发布的书对象
        book = BookInfo.objects.latest("bpub_date")
        # 2、构建序列化器对象
        bs = self.get_serializer(book)
        # 3、序列化返回
        return Response(bs.data)

    # 修改阅读量
    # PATCH
    # /books/(?P<pk>\d+)/read/
    @action(methods=['patch'], detail=True)
    def read(self, request, pk):
        # 修改阅读量
        return Response("OK")

```

```

from rest_framework.viewsets import ModelViewSet,ViewSet
from rest_framework.response import Response
from rest_framework.decorators import action


from .models import *
from .serializers import *



from rest_framework.authentication import SessionAuthentication,BaseAuthentication
from rest_framework.permissions import IsAuthenticated,IsAdminUser,AllowAny,IsAuthenticatedOrReadOnly
from rest_framework.filters import OrderingFilter
from rest_framework.pagination import PageNumberPagination,LimitOffsetPagination

from django_filters import FilterSet



class MyPage(PageNumberPagination):
    # 自定义分页器
    page_size = 3
    page_query_param = "page" # /books/?page=1
    page_size_query_param = "pagesize" # /books/?page=1&pagesize=2
    max_page_size = 10


class MyLimit(LimitOffsetPagination):
    offset_query_param = "offset" # books/?offset=3
    limit_query_param = "limit" # books/?offset=3&limit=2
    max_limit = 10
    # default_limit = 3



# 自定义规则
class MyFilterSet(FilterSet):
    # 针对哪个模型类
    # 指明字段
    class Meta:
        model = BookInfo
        # fields = ['bread', 'bcomment']
        fields = {
            # key: 过滤依据字段； value：过滤规则
            "btitle": ['exact', 'contains', 'startswith', 'endswith']
        }



# 原则：一类资源BookInfo，我们应该尽量用一个视图类去处理
class BooksAPIView(ModelViewSet):
    """
    这个视图接口是处理书本信息

    list: 获取多条书本
    create: 新建一本书
    """


    queryset = BookInfo.objects.all()
    serializer_class = BookInfoModelSerializer

    # 局部的身份认证配置：当前视图接收请求的时候会根据authentication_classes指明的认证器去认证
    # 局部配置会覆盖全局
    # authentication_classes = [SessionAuthentication, BasicAuthentication]

    # 局部配置权限
    # permission_classes = [IsAdminUser]

    # 局部配置限流
    # throttle_classes = [AnonRateThrottle, UserRateThrottle]

    # 自定义限流规则
    # throttle_scope = 'upload'

    # 指明过滤的依据
    # filterset_fields = ['bread', 'bcomment']

    # 自定义规则
    # filterset_class = MyFilterSet

    # 指明过滤后端
    # filter_backends = [OrderingFilter]
    # ordering_fields = ['bread', 'bcomment']


    # 分页局部配置
    pagination_class = MyLimit


    # 返回最新发布的书本信息
    # GET
    # /books/last/
    @action(methods=['get'], detail=False)# , url_path="aaa")
    def last(self, request):

        # 在一个视图中如何获取，此次请求的用户对象呢？
        # request.user: 此次请求的用户对象，如果身份认证未通过为None


        a = 10/0 # 抛出ZeroDivisionError，内建异常

        # 1、过滤最新发布的书对象
        book = BookInfo.objects.latest("bpub_date")
        # 2、构建序列化器对象
        bs = self.get_serializer(book)
        # 3、序列化返回
        return Response(bs.data)

    # 修改阅读量
    # PATCH
    # /books/(?P<pk>\d+)/read/
    @action(methods=['patch'], detail=True)
    def read(self, request, pk):
        # 修改阅读量
        return Response("OK")

```




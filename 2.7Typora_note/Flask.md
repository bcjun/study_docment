- Werkzeug（路由模块）
- 模板引擎则使用 Jinja2

- Flask-SQLalchemy：操作数据库；
- **Flask-RESTful** ：开发REST API的工具；

# 第一个Flask程序[重点]

> **一个简单的Flask程序**
>
> ~~~python
> 步骤：
> #1.导入Flask类
> 
> #2.创建app对象：
> 	app = Flask(__name__)
>   
> #3.使用装饰器绑定路由和视图函数：
> 	@app.route('/')
> 	def index():
>     	return 'Hello World'
>     
> #4.Flask应用程序实例的 run 方法 启动 WEB 服务器:
> 	 app.run()
>   
> # flask路由列表
>   app.url_map
>     [
>         <Rule '/' (HEAD, GET, OPTIONS) -> helloworld>,
>         # flask自带访问静态文件的路由
>         <Rule '/static/<filename>' (HEAD, GET, OPTIONS) -> static>
>  ]
>     
> PS:
>     # 注意：response中django是不能直接返回字符串的，而flask可以直接返回字符串
> ~~~
>
> **Flask初始化参数**
>
> ~~~python
> 总结：
> # 初始化app对象的时候，可以指明对应初始化参数实现特定作用
> 1.__name__（必须传）: (重点)
> 认为当前文件所有在目录就是项目目录，会在这个目录下寻找静态文件路径（图片，js,css），模板文件路径(html)
> 
> 2.static_url_path(可以不传,知道作用就好):
> 静态文件访问路径前缀，，默认为：/ 
> 
> 3.static_folder(可以不传,知道作用就好):
> 静态文件存取的文件夹，，默认为: static
> 
> 4.template_folder
> 模板文件存取的文件夹，可以不传，默认为: templates
> PS:
> 	Flask程序相关配置加载方式（掌握）
> ~~~
>
> **Flask程序相关配置加载方式（掌握）**
>
> ~~~~
> # 方法一：从配置类中加载
>     1.自定义配置类：
>         class Config(object):
>             DEBUG=True
>     2.从配置类中加载配置信息：
>         app.config.from_object(类名)
>         
>    优点: 继承 复用
>    缺点: 敏感数据暴露 
>    
> # 方法二：从py文件中加载
> 
>     1.在工程目录下创建一个setting.py配置文件，设置对应配置变量
>     2.flask应用从读取setting.p配置文件
>     app.config.from_pyfile("setting.py")
>     
>    优点: 敏感数据不易暴露 
>    缺点: 文件路径固定 使用不灵活
>    
> 
> # 方法三：从环境变量中加载
> 		
>   	 shell 脚本语言
>     1.设置环境变量
>     export PROJECT="setting.py"
> 
>     2.根据环境变量名称，读取环境变量
>     app.config.from_envvar("PROJECT", silent=True)
> 
>    优点: 敏感数据不易暴露 
>    缺点: 不方便 切记每次要设置环境变量
> 
> # 补充：
> 	silent=True ：安静的处理，即使环境变量不存在，也不会报错
> 	silent=False ： 当环境变量不存在时会报错
> 	
> ~~~~
>
> **访问路由**
>
> ~~~
> # 修改路由：
> 1.修改@app.route('路由地址')的参数达到修改访问url：
> 
> # 获取路由信息的属性[元祖]
> app.url_app
>     
> # 获取路由规则    
> rule_list = app.url_map.iter_rules()
> 
> for rule in rule_list
>   # 视图函数名称
>   rule.endpoint
>   # 视图函数对应的路由路径
>   rule.rule
> 
> PS:
> 	app.url_map
>     [
>         <Rule '/' (HEAD, GET, OPTIONS) -> helloworld>,
>         # flask自带访问静态文件的路由
>         <Rule '/static/<filename>' (HEAD, GET, OPTIONS) -> static>
>  ]
> ~~~
>
> **指定接受的请求方式**
>
> ~~~
> app.route(路径,methods=["POST","GET"])
> ~~~

# 使用工厂方法创建app对象

> ~~~python
> # 1.传入配置类
> # 2.创建app对象
> # 3.app加载配置类中的配置信息
> # 3.app加载环境变量中的配置信息，会覆盖配置类中同名的配置信息，达到隐藏敏感信息的作用
> # 4.返回app对象
> 
> def create_flask_app(config):
> 	
> 		app = Flask(__name__)
>     
>    app.config.from_object(config)
>     
>    # 从环境变量中加载配置信息，会覆盖配置类中同名的配置信息
>    app.config.from_envvar("PROJECT_SETTING", silent=True)
> 		return app
> # 从工厂方法中获取app对象
> app = create_flask_app(配置类名称)
> ~~~
>
> 新版Flask运行方式
>
> ~~~python
> #1.运行前需要将具体需要运行的文件，使用环境变量的形式指明
> export FLASK_APP=flask文件名称 
> 
> #3.设置运行模式：(非必须,可以不指明，默认是production生产模块）
> export FLASK_ENV=production  运行在生产模式，未指明则默认为此方式
> export FLASK_ENV=development 运行在开发模式
> 
> #2.使用命令启动：默认运行在127.0.0.1:5000端口
> flask run   
> 
> # 指明ip和端口
> flask run -h 0.0.0.0 -p 8000
> 
> # 注意：FLASK_APP  FLASK_ENV  production development 固定写法 不能出错
> 
> PS:
>     # run函数参数[了解]
>     # 旧版本运行方案： app.run(host,port)
>     通过修改app.run()函数的host，port参数，指定运行的ip地址和端口号。
> 
> ~~~

# blueprint蓝图

> **蓝图的作用及步骤**
>
> ~~~
> blueprint蓝图作用：分模块开发
> 蓝图基本使用步骤：
> 1.创建蓝图对象 模块的应用对象，理解成：单独某个模块中的app对象。
> 2.使用蓝图对象绑定视图函数和路由。
> 3.调用app的register_blueprint方法注册蓝图对象
> ~~~
>
> **blueprint结构**
>
> ![Blueprint](D:\笔记\2.7Typora_note\配置图片\Blueprint.png)
>
> 1. **创建一个蓝图对象**
>
>    ```python
>    user_bp=Blueprint('user',__name__)
>    ```
>
> 2. **在这个蓝图对象上进行操作,注册路由,指定静态文件夹,注册模版过滤器**
>
>    ```python
>     @user_bp.route('/')
>     def user_profile():
>         return 'user_profile'
>    ```
>
> 3. **在应用对象上注册这个蓝图对象**
>
>    ```python
>     app.register_blueprint(user_bp)
>    ```

# 数据获取

### URL路径参数（转换器语法）

> **默认方式**
>
> ~~~
> # 默认采用string过滤
> @app.route('/users/<user_id>')
> def user_info(user_id):
>     print(type(user_id))
>     return 'hello user {}'.format(user_id)
> ~~~
>
> **指定转换器**
>
> ```
> # 有过滤功能
> @app.route('/user/<string:user_id>')
> def user_string(user_id):    
> 	print(type(user_id))    
>     return 'user id: {}'.format(user_id)
> ```
>
> **源码**
>
> ```python
> DEFAULT_CONVERTERS = {
>     'default':          UnicodeConverter,
>     'string':           UnicodeConverter,
>     'any':              AnyConverter,
>     'path':             PathConverter,
>     'int':              IntegerConverter,
>     'float':            FloatConverter,
>     'uuid':             UUIDConverter,
> }
> PS:
>     里面所有的转换器都继承自from werkzeug.routing import BaseConverter 
>     所以我们只需要自定义一个类继承自BaseConverter，然后添加到这个字典里面就可以了==>实际上我们不是直接添加到这个字典中去，Flask有拷贝这个字典，所以我们是添加到副本里面去的
> ```
>
> ~~~python
> class BaseConverter(object):
>     """Base class for all converters."""
> 
>     regex = "[^/]+"
>     weight = 100
> 
>     def __init__(self, map):
>         self.map = map
> 
>     def to_python(self, value):
>         return value
> 
>     def to_url(self, value):
>         if isinstance(value, (bytes, bytearray)):
>             return _fast_url_quote(value)
>         return _fast_url_quote(text_type(value).encode(self.map.charset))
> 
> 
> class UnicodeConverter(BaseConverter):
> ~~~

**自定义转换器**

> 1.创建转换器类，保存匹配时的正则表达式
>
> ~~~
> # 注意regex名字固定
> from werkzeug.routing import BaseConverter
> 
> class MobileConverter(BaseConverter):
>     """
>     手机号格式
>     """
>     regex = r'1[3-9]\d{9}'
> ~~~
>
> 2.将自定义的转换器告知Flask应用
>
> ```python
> app = Flask(__name__)
> 
> # 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: mobile
> app.url_map.converters['mobile'] = MobileConverter
> ```
>
> 3.在使用转换器的地方定义使用
>
> ```python
> @app.route('/sms_codes/<mobile:mob_num>')
> def send_sms_code(mob_num):
>     return 'send sms code to {}'.format(mob_num)
> ```

### 请求对象request参获取参数

| 属性    | 说明                           | 类型           |
| :------ | :----------------------------- | :------------- |
| data    | 记录请求的数据，并转换为字符串 | *              |
| form    | 记录请求中的表单数据           | MultiDict      |
| args    | 记录请求中的查询参数           | MultiDict      |
| cookies | 记录请求中的cookie信息         | Dict           |
| headers | 记录请求中的报文头             | EnvironHeaders |
| method  | 记录请求使用的HTTP方法         | GET/POST       |
| url     | 记录请求的URL地址              | string         |
| files   | 记录请求上传的文件             | *              |
| json    | 记录请求json数据，并转换成json | dict           |

> >PS:传过来的都是字典

**例子**

> ~~~python
> # 需求：提取查询字符串参数
> # 127.0.0.1:5000/articles?article_id=1
> @app.route('/articles')
> def get_article_id():
> 
>     # 使用request请求对象中args属性提取查询字符串参数 --- dict
>     param_dict = request.args
>     article_id = param_dict.get("article_id")
> 
>     return 'article_id: {}'.format(article_id)
> 
> 
> # 需求：提取前端发送json格式数据：{"name": "curry"}
> @app.route('/user', methods=["post"])
> def get_user_name():
> 
>     # 使用request对象中json属性提取前端发送的json格式数据 --dict
> 
>     # 注意：请求方式必须大写
>     if request.method == "POST":
>         param_dict = request.json
>         name = param_dict.get("name")
>         return "user name: {}".format(name)
>     else:
>         return "method not allow"
> 
> 
>     # 需求：提取前端发送的文件类型数据（图片） {'pic': 图片的数据}
> @app.route('/upload', methods=["post"])
> def get_pic():
> 
>     # 使用request对象中files属性提取前端发送的图片数据 ---文件类型数据
>     pic_file = request.files.get("pic")
> 
>     # 读取文件二进制数据
>     # pic_file.read()
> 
>     # 将图片数据存储到本地
>     pic_file.save("./1.png")
> 
>     # TODO:将图片数据存储到云平台
> 
>     return "upload file success"
> 
> ~~~



# 处理响应

### 返回模板

> ~~~
> # 渲染模版函数
> # 参数1：模板名称
> # 参数n：传入模板的参数
> render_template("index.html", my_str=mstr, my_int=mint)
> ~~~

### 重定向

> ~~~
> 总结：
> 	概念：当你访问某一url路由的时候，不是给你引导到当前url对应的网页而是跳转到了另一个url对应的网页。
> 	
> # 重定向函数：
> redirect(url地址)
> ~~~

### 返回JSON格式数据（重点）

> ~~~
> 总结：
> 当需要给客户端返回json类型的数据的时候，可以借助jsonify函数将python字典转换成json字符串
> 语法格式：
> 	jsonify(字典)
> 作用：
> 	1.将字典转换成json字符串
> 	2.将返回值包装成resonse对象
> 	3.将数据类型设置成application/json格式
> ~~~

### 自定义状态码和响应头

> ~~~
> # 方法1： 元祖方式
>  返回响应格式： (response, status, headers) 
> 
> # 方法2：make_response方式
> 
>     resp = make_response('响应体')
>     # 设置自定义响应头
>     resp.headers["Itcast"] = "Python"
>     # 设置状态码
>     resp.status = “404 not found”
> ~~~

# cookie与session

> **cookie**
>
> ~~~
> 总结：
> cookie作用：
> 	状态保持，以键值对方式保存用户信息到`浏览器`。
> 	
> 	
> 特点：
>     1.Cookie是由服务器端生成，发送给客户端浏览器。
>     2.浏览器会将Cookie的key/value保存。
>     3.下次请求同一网站时就发送该Cookie给服务器（前提是浏览器设置为启用cookie）
>     4.Cookie基于域名安全，不同域名的Cookie是不能互相访问的
>     5.cookie里面的数据都是字符串,每个域名最多只能保存20条cookie
> 
> 1.设置cookie:
>  response.set_cookie(key, value, max_age=过期时长)
> 2.获取cookie:
>  request.cookies.get(key, "")
> 3.删除cookie:
>  response.delete_cookie(key)
> ~~~
>
> **session**
>
> ~~~
> Session作用：
> 	状态保持，以键值对方式保存用户信息到`服务器` --> redis 。
> 
> 特点：
>     1.在服务器端进行状态保持的方案就是Session
>     2.以字典的方式将数据保存到服务器端。
> 	3.Session依赖于Cookie。(最终会返回session_id,需要借助cookie告知浏览器)
>  
> 1.设置session:
>  session["key"] = "value"
> 2.获取Session:
>  session.get("key", "")
> 3.删除Session:
>  session.pop("key", "")
>  
> 注意点：使用session的时候要设置 `SECRET_KEY` 加密字符串，到时候session_id需要进行混淆加密处理
> ~~~

# 异常捕获

> **@app.errorhandler(错误状态码&异常类)**
>
> ~~~
> 主动抛出异常语法: abort(HTTP的错误状态码)
> 
> 捕获错误状态码&异常：@app.errorhandler(错误状态码&异常类)
> 
> ~~~
>
> **例子**
>
> ~~~
> # 需求：捕获异常 统一引导到定制的页面，好处：提高用户体验  http://err.www.mi.com/04.html
> @app.errorhandler(404)
> def handler_404(e):
>     """
>     一旦不获取到404异常 该方法就会被调用
>     :param e:
>     :return:
>     """
>     print(e)
> 
>     # 注意：需要接受异常字符串 或者使用 _ 忽略该参数
>     return redirect("http://err.www.mi.com/04.html")
> ~~~

# 请求勾子[重要]

> ~~~
> 2.当请求发送过来的时候有四个时机：
> 	1.第一次请求之前				before_first_request
> 	2.每次请求之前				 before_request
> 	3.每次请求之后				 after_request
> 	4.每次请求之后是否有错误		  teardown_request
> ~~~
>
> **例子**
>
> ~~~
> @app.after_request
> def after_request(response):
>     """
>     每一次`请求之后`主动调用 注意：需要接受视图函数返回的响应response  [重要]
> 
>     作用：对响应对象进行统一拦截处理
>     response.set_cookie()
>     response.headers["key"] = "value"
>     :return:
>     """
> 
>     print("3.每一次请求之后（视图函数执行之后）：after_request")
> 
>     return response
> 
> ~~~
>
> ~~~
> @app.before_request
> def before_request():
>     """
>     每一次请求之前主动调用该函数 【重点】
>     作用：
>     jwt用户验证认证： request.headers --> token --> 用户信息
> 
>     request.url --> 黑名单 --> 拦截 --> 返回响应即可（提前返回响应再进入视图函数执行返回响应）
>     :return:
>     """
> 
>     print("2.每一次请求之前：before_request")
> ~~~

# 请求上下文&应用上下文

> ~~~
> 上下文定义：
> 根据之前代码所做的操作以及下文即将要执行的逻辑，可以决定在当前时刻下可以使用到的变量，或者可以完成的事情。
> 
> Flask中有两种上下文：
> 1.请求上下文： request，session
> 2.应用上下文： current_app(app的别名)，g(全局的一个临时变量)
> 注意：不同的请求，会有不同的g对象全局变量，超出请求超出范围不能使用
> 
> PS:
>     # with app.app_context(): 手动开启应用上下文 （常用）
>     # with app.request_context(environ): 手动开启请求上下文
>     # 一般终端测试的时候需要手动去开启
> ~~~
>
> **例子**
>
> > 请求上下文
>
> ~~~python
> from flask import Flask, request,session
> 
> app = Flask(__name__)
> 
> 
> # 注意：设置session加密字符串
> app.secret_key = "as;ldkas;ldkl;*"
> 
> 
> # 请求上下文：request, session
> 
> # RuntimeError: Working outside of request context. 超出了请求上下文的范围
> # 没有上下文 不能操作 属性 变量
> # print(request.remote_addr)
> 
> 
> # with app.app_context(): 手动开启应用上下文 （常用）
> 
> # with app.request_context(environ): 手动开启请求上下文
> 
> @app.route('/')
> def hello_world():
> 
>     # 视图函数中具备上下文
> 
>     print(request.method)
> 
>     session["name"] = "james"
> 
>     print(session.get("name"))
> 
>     return 'Hello World!'
> 
> 
> if __name__ == '__main__':
>     app.run()
> ~~~
>
> > **current_app**
>
> ~~~python
> from flask import Flask, current_app
> 
> app1 = Flask(__name__)
> app2 = Flask(__name__)
> 
> app1.redis_cli = "app1 redis_cli"
> app2.redis_cli = "app2 redis_cli"
> 
> 
> # 应用上下文：current_app 代表的是当前运行的app对象,在别的模块current_app.属性名
> 
> # Detected multiple Flask applications in module "demo6_current_app". Use "FLASK_APP=demo6_current_app:name"
> # 项目中有多个app对象 需要使用 FLASK_APP=demo6_current_app:app1  指明具体运行的是那个app对象
> @app1.route('/')
> def hello_world():
> 
>     #  current_app代表的是app1
>     #  app1 redis_cli
>     print(current_app.redis_cli)
>     return 'Hello World!'
> 
> 
> @app2.route('/app2')
> def hello_world():
> 
>     #  current_app代表的是app2
>     #  app2 redis_cli
>     print(current_app.redis_cli)
>     return 'Hello World!'
> 
> ~~~
>
> > **g变量**
>
> ~~~python
> from flask import Flask, g
> from demo import get_user
> 
> app = Flask(__name__)
> 
> # 应用上下文：g
> # 作用：全局的`临时`变量 在`同一次请求`的多个函数之间参数存储和参数传递, 下一次请求的时候g变量中数据就会被清空
> 
> 
> @app.route('/')
> def hello_world():
> 
>     g.user_name = "xiaoming"
> 
>     get_user()
> 
>     return 'Hello World!'
> 
> 
> @app.route('/index')
> def index():
> 
>     # _AppCtxGlobals' object has no attribute 'user_name'
>     # 新的请求，g变量的值就会被清空  g变量是一个临时的变量
>     print(g.user_name)
> 
>     return 'index'
> ~~~

# Flask-RESTful

> **Flask-RESTful的基本使用**

**蓝图中使用Flask-RESTful**

> ~~~
> # 需求：将蓝图对象包装成restful api风格
> from flask import Flask, Blueprint
> from flask_restful import Api, Resource
> 
> # 1.创建app对象
> app = Flask(__name__)
> 
> # 2.创建蓝图对象
> user_bp = Blueprint("user", __name__)
> 
> # 3.将 `蓝图对象` 包装成api对象
> api = Api(user_bp)
> 
> 
> # 4.自定义类视图实现业务逻辑
> class UserResource(Resource):
> 
>     def get(self):
> 
>         return "get message"
> 
>     def post(self):
> 
>         return "new resource"
> 
> 
> # 5.通过app对象注册蓝图  目的：注册路由信息 app.url_map
> app.register_blueprint(user_bp)
> 
> # 6.给类视图添加路由信息  endpoint类似于django中namespace
> api.add_resource(UserResource, "/", endpoint="user route")
> ~~~
>
> **给类视图中的方法添加装饰器**
>
> ~~~python
> from flask import Flask
> from flask_restful import Api, Resource
> 
> # 1.创建app对象
> app = Flask(__name__)
> 
> # 2.将app对象包装成api对象，具备restful风格
> api = Api(app)
> 
> 
> # 自定义装饰器
> def my_decorator1(func):
> 
>     print("init my_decorator1")
> 
>     def wrapper(*args, **kwargs):
>         print("dispatch my_decorator1")
>         return func(*args, **kwargs)
>     return wrapper
> 
> 
> def my_decorator2(func):
> 
>     print("init my_decorator2")
> 
>     def wrapper(*args, **kwargs):
>         print("dispatch my_decorator2")
>         return func(*args, **kwargs)
>     return wrapper
> 
> 
> # 3.自定义类视图实现业务逻辑
> class HelloworldResource(Resource):
> 
>     # 方法1：给类中 `所有方法` 添加装饰器
>     # method_decorators = [my_decorator1]
> 
>     # 方法2：给类中指定方法添加装饰器
>     method_decorators = {
>         "get": [my_decorator1, my_decorator2],
>         "post": [my_decorator2],
>     }
> 
>     def get(self):
>         """
>         处理get请求的业务逻辑
>         """
>         return "get message"
> 
>     def post(self):
>         """
>         处理post请求的业务逻辑
>         """
>         return "new resource"
> # 4.给类视图添加路由信息
> # 参数1：类视图名称  参数2：路由地址
> api.add_resource(HelloworldResource, "/")
> # api.add_resource(HelloworldResource, "/", endpoint="路由名称")
> # django中写法： url("/", HelloworldResource.as_view() )
> ~~~

### Flask-RESTful中请求处理

> ~~~python
> from flask import Flask
> from flask_restful import Api, Resource, inputs
> from flask_restful.reqparse import RequestParser, Argument
> 
> # 1.创建app对象
> app = Flask(__name__)
> 
> # 2.将app对象包装成api对象，具备restful风格
> api = Api(app)
> 
> 
> """
> 需求：使用flask-restful 处理请求
> 
> 127.0.0.1:5000/user?name=laowang
> 
> # 1.提取参数
> name  = request.args.get("name")
> 
> # 2.校验参数
> if name is None:
>     ...
> else:
>     ...
> 
> """
> import re
> 
> 
> def regex_mobile(mobile_str):
>     """
>     手机号码正则校验
>     :param mobile_str:
>     :return:
>     """
>     if re.match(r'^1[3-9]\d{9}$', mobile_str):
>         # 正则匹配成功
>         return mobile_str
>     else:
>         # 匹配失败
>         return "invalid mobile number"
> 
> 
> 
> # 3.自定义类视图实现业务逻辑
> class HelloworldResource(Resource):
> 
>     def get(self):
>         """
>         处理get请求的业务逻辑
>         :return:
>         """
>         # 需求：
>         # 127.0.0.1:5000/user?name=laowang&like="basketball"&like="football"&height=180&mobile=1851112222&age=18
> 
>         # 1.构建解析器对象
>         parser = RequestParser()
> 
>         # 2.添加需要解析的字段
>         # 参数1：需要解析的字段名称  参数2：参数是否必传  参数3：参数错误的提示信息
>         parser.add_argument("name", required=True, type=str, help="missing  name")
> 
>         # action = "store" 在一键多值的情况下默认值保留第一个键值对信息
>         # action = "append" 一键多值保留所有数据
>         parser.add_argument("like", required=True, action="append")
> 
>         # type 指明参数类型
>         # 1. python标准类型 str int float ...
>         # 2. inputs模块中定义的类型 int_range ...
> 
>         # parser.add_argument("height", required=True, type=float)
>         parser.add_argument("height", required=True, type=inputs.int_range(120, 250))
> 
>         # 3. 参数类型可以是自定义的`函数名称`
>         parser.add_argument("mobile", required=True, type=regex_mobile)
> 
>         # location="json" 要求前端传递参数的位置 从json字符串中传递
>         # 注意点：location和required=True需要同时指明
>         parser.add_argument("age", required=True, type=int, location="json")
> 
>         # 3.开启解析，获取解析结果
>         ret = parser.parse_args()
> 
>         # 4.从解析结果字典中提取数据
>         name = ret.get("name")
>         like = ret.get("like")
>         height = ret.get("height")
>         mobile = ret.get("mobile")
>         age = ret.get("age")
> 
>         return "get message name:{}  like:{}  height:{}  mobile:{} age:{}".format(name, like, height, mobile, age)
> 
>     def post(self):
>         """
>         处理post请求的业务逻辑
>         :return:
>         """
>         return "new resource"
> 
> 
> # 4.给类视图添加路由信息
> # 参数1：类视图名称  参数2：路由地址
> api.add_resource(HelloworldResource, "/user")
> 
> ~~~

### Flask-RESTful中响应对象处理

> ~~~python
> from flask import Flask
> from flask_restful import Api, Resource, marshal_with, fields
> 
> 
> # from flask_restful.representations import json
> 
> # 1.创建app对象
> app = Flask(__name__)
> 
> # 2.将app对象包装成api对象，具备restful风格
> api = Api(app)
> 
> 
> class Person(object):
> 
>     def __init__(self, name, id):
>         self.name = name
>         self.id = id
> 
> # 1.提前声明返回数据的格式
> person_feild = {
>     # 字段名称   字段类型
>     "name":  fields.String,
>     "id": fields.Integer
> }
> 
> 
> # 3.自定义类视图实现业务逻辑
> class HelloworldResource(Resource):
> 
>     def get(self):
>         """
>         处理get请求的业务逻辑
>         :return:
>         """
> 
>         # 需求：返回具备restful-api风格的响应对象 json字符串
> 
>         my_dict = {
>             "name": "Harden",
>             "team": "Rocket"
>         }
> 
>         # alt + 回车键 output_json
> 
>         # 在flask_restful底层已经将字典转换成json字符串的过程封装好了，我们只需要返回字典即可
>         # 在底层output_json内部已将封装完毕
>         return my_dict
> 
>     # 2.使用marshal_with装饰器标明需要返回`对象`
>     @marshal_with(person_feild)
>     def post(self):
>         """
>         处理post请求的业务逻辑
>         :return:
>         """
> 
>         # 构建类的对象
>         person = Person("xiaoming", 1)
> 
>         # 方案1：将对象手动转换成字典 (推荐)
>         person_dict = {
>             "name": person.name,
>             "id": person.id
>         }
> 
>         # return person_dict
> 
> 
>         # Object of type Person is not JSON serializable
>         # 对象类型数据不能直接json序列化
>         # 使用marshal_with提前声明序列化格式就能直接返回对象
>         return person
> 
> 
> # 4.给类视图添加路由信息
> # 参数1：类视图名称  参数2：路由地址
> api.add_resource(HelloworldResource, "/")
> 
> ~~~

### 定制返回的JSON格式

> ~~~python
> from flask import Flask
> from flask_restful import Api, Resource
> from flask_restful.representations import json
> 
> # 1.创建app对象
> app = Flask(__name__)
> 
> # 2.将app对象包装成api对象，具备restful风格
> api = Api(app)
> 
> """
> 需求：json格式的定制化处理
> 
> # 原本json格式
> {
>     "name": "Harden",
>     "team": "Rocket"
> }
> 
> 
> # 定制化后的格式：
> {
>     message: "OK",
>     data: {
>         "name": "Harden",
>         "team": "Rocket"
>     }
> }
> 
> 
> 方案1：改源代码  （不推荐）
> 
> 方案2：@api.representation("application/json")
> 
> """
> 
> 
> # 3.自定义类视图实现业务逻辑
> class HelloworldResource(Resource):
> 
>     def get(self):
>         """
>         处理get请求的业务逻辑
>         :return:
>         """
>         my_dict = {
>                 "name": "Harden",
>                 "team": "Rocket"
>         }
> 
>         return my_dict
> 
>     def post(self):
>         """
>         处理post请求的业务逻辑
>         :return:
>         """
>         return "new resource"
> 
>     def delete(self):
>         pass
> 
>     def put(self):
>         pass
> 
>     def patch(self):
>         pass
> 
> 
> from flask import make_response, current_app
> from flask_restful.utils import PY3
> from json import dumps
> 
> 
> # 复制底层源码，使用representation("application/json")装饰器拦截底层返回的数据，进行定制化处理
> @api.representation("application/json")
> def output_json(data, code, headers=None):
>     """Makes a Flask response with a JSON encoded body"""
> 
>     # 此处为自己添加***************
>     if 'message' not in data:
>         data = {
>             'message': 'OK',
>             'data': data
>         }
>     # **************************
> 
>     settings = current_app.config.get('RESTFUL_JSON', {})
> 
>     # If we're in debug mode, and the indent is not set, we set it to a
>     # reasonable value here.  Note that this won't override any existing value
>     # that was set.  We also set the "sort_keys" value.
>     if current_app.debug:
>         settings.setdefault('indent', 4)
>         settings.setdefault('sort_keys', not PY3)
> 
>     # always end the json dumps with a new line
>     # see https://github.com/mitsuhiko/flask/pull/1262
>     dumped = dumps(data, **settings) + "\n"
> 
>     resp = make_response(dumped, code)
>     resp.headers.extend(headers or {})
>     return resp
> 
> 
> 
> # 4.给类视图添加路由信息
> # 参数1：类视图名称  参数2：路由地址
> api.add_resource(HelloworldResource, "/")
> # api.add_resource(HelloworldResource, "/", endpoint="路由名称")
> # django中写法： url("/", HelloworldResource.as_view() )
> ~~~

### PS:装饰器的第二种使用方法

> ~~~python
> from functools import wraps
> 
> 
> def login_required(func):
> 
>     @wraps(func)
>     def wrapper(*args, **kwargs):
>         # 1.装饰器额外补充的业务逻辑
>         print("调用了")
> 
>     return wrapper
> 
> 
> @login_required
> def index():
> 
>     print("index")
> 
> 
> @login_required
> def user():
>     print("user")
> 
> 
> """
> # 问题：装饰器能改变被装饰的视图函数名称，已经doc文件
> # 解决方案：使用functools 中 @wraps(func)
> 
> 
> 使用装饰器的2种方法：
> 
> 方法一（语法糖）：
> 
> @login_required
> def index():
>     pass
> 
> 方法二： 装饰器本质其实相当于是函数
> 
> login_required(index)
> 
> """
> 
> if __name__ == '__main__':
> 
>     print(index.__name__)
>     print(user.__name__)
> ~~~

# ORM

### ORM的方式选择

> 先创建模型类，再迁移到数据库中
>
> ~~~
> 优点：简单快捷，定义一次模型类即可，不用写sql
> 缺点：不能尽善尽美的控制创建表的所有细节问题，表结构发生变化的时候，也会难免发生迁移错误
> ~~~
>
> 先用原生SQL创建数据库表，再编写模型类作映射
>
> ~~~
> 优点：可以很好的控制数据库表结构的任何细节，避免发生迁移错误
> 缺点：可能编写工作多（编写sql与模型类，似乎有些牵强）
> ~~~

### SQLAlchemy

> **连接SQLAlchemy**
>
> ~~~
> mysql://user:password@localhost/mydatabase
> SQLALCHEMY_TRACK_MODIFICATIONS 在Flask中是否追踪数据修改
> 
> SQLALCHEMY_ECHO 显示生成的SQL语句，可用于调试
> ~~~
>
> **例子**
>
> ~~~
> from flask import Flask
> 
> app = Flask(__name__)
> 
> class Config(object):
>     SQLALCHEMY_DATABASE_URI = 'mysql://root:mysql@127.0.0.1:3306/toutiao'
>     SQLALCHEMY_TRACK_MODIFICATIONS = False
>     SQLALCHEMY_ECHO = True
> 
> app.config.from_object(Config)
> ~~~

| 名字                      | 备注                                                         |
| :------------------------ | :----------------------------------------------------------- |
| SQLALCHEMY_DATABASE_URI   | 用于连接的数据库 URI 。例如:sqlite:////tmp/test.dbmysql://username:password@server/db |
| SQLALCHEMY_BINDS          | 一个映射 binds 到连接 URI 的字典。更多 binds 的信息见[*用 Binds 操作多个数据库*](http://docs.jinkan.org/docs/flask-sqlalchemy/binds.html#binds)。 |
| SQLALCHEMY_ECHO           | 如果设置为Ture， SQLAlchemy 会记录所有 发给 stderr 的语句，这对调试有用。(打印sql语句) |
| SQLALCHEMY_RECORD_QUERIES | 可以用于显式地禁用或启用查询记录。查询记录 在调试或测试模式自动启用。更多信息见get_debug_queries()。 |
| SQLALCHEMY_NATIVE_UNICODE | 可以用于显式禁用原生 unicode 支持。当使用 不合适的指定无编码的数据库默认值时，这对于 一些数据库适配器是必须的（比如 Ubuntu 上 某些版本的 PostgreSQL ）。 |
| SQLALCHEMY_POOL_SIZE      | 数据库连接池的大小。默认是引擎默认值（通常 是 5 ）           |
| SQLALCHEMY_POOL_TIMEOUT   | 设定连接池的连接超时时间。默认是 10 。                       |
| SQLALCHEMY_POOL_RECYCLE   | 多少秒后自动回收连接。这对 MySQL 是必要的， 它默认移除闲置多于 8 小时的连接。注意如果 使用了 MySQL ， Flask-SQLALchemy 自动设定 这个值为 2 小时。 |

### 模型类字段与选项

**字段类型**

| 类型名       | python中类型      | 说明                                                |
| :----------- | :---------------- | :-------------------------------------------------- |
| Integer      | int               | 普通整数，一般是32位                                |
| SmallInteger | int               | 取值范围小的整数，一般是16位                        |
| BigInteger   | int或long         | 不限制精度的整数                                    |
| Float        | float             | 浮点数                                              |
| Numeric      | decimal.Decimal   | 普通整数，一般是32位                                |
| String       | str               | 变长字符串                                          |
| Text         | str               | 变长字符串，对较长或不限长度的字符串做了优化        |
| Unicode      | unicode           | 变长Unicode字符串                                   |
| UnicodeText  | unicode           | 变长Unicode字符串，对较长或不限长度的字符串做了优化 |
| Boolean      | bool              | 布尔值                                              |
| Date         | datetime.date     | 时间                                                |
| Time         | datetime.datetime | 日期和时间                                          |
| LargeBinary  | str               | 二进制文件                                          |

**列选项**

| 选项名      | 说明                                              |
| :---------- | :------------------------------------------------ |
| primary_key | 如果为True，代表表的主键                          |
| unique      | 如果为True，代表这列不允许出现重复的值            |
| index       | 如果为True，为这列创建索引，提高查询效率          |
| nullable    | 如果为True，允许有空值，如果为False，不允许有空值 |
| default     | 为这列定义默认值                                  |

**关系选项**

| 选项名         | 说明                                                         |
| :------------- | :----------------------------------------------------------- |
| backref        | 在关系的另一模型中添加反向引用                               |
| primary join   | 明确指定两个模型之间使用的联结条件                           |
| uselist        | 如果为False，不使用列表，而使用标量值                        |
| order_by       | 指定关系中记录的排序方式                                     |
| secondary      | 指定多对多关系中关系表的名字                                 |
| secondary join | 在SQLAlchemy中无法自行决定时，指定多对多关系中的二级联结条件 |

### 模型类的基本操作

> ~~~
> 总结：
> 	插入、修改、删除都是由数据库会话 `db.session` 管理
>     
>     将数据写入数据库两个步骤：
>     	`1.要先将数据添加到会话中
>        2.调用commit()方法提交会话。
>     
>     插入: 
>         db.session.add(对象)
>         db.session.commit()
>         
>         # 如果commit提交失败，可以回滚
>         db.session.rollback()
>     
>    	修改:
>         user.name = "curry"
>         db.session.commit()
>         
>     删除:
>         db.session.delete(对象)
>         db.session.commit()
>         
>         
>         
>       查询所有用户数据
>       User.query.all()
> 
>       查询1号用户  只能填id
>       User.query.get(1)
> 
> ~~~

### 数据库查询

~~~
查询所有用户数据
User.query.all()

查询1号用户  只能填id
User.query.get(1)


# 查询有多少个用户
User.query.count()


# 查询id为4的用户[3种方式]

User.query.get(4)


# filter_by 精确查询 id是来源于User表所以不需要指明

User.query.filter_by(id=4).first()

# filter 全局查询 User.id 必须指明id来源于那张表

User.query.filter(User.id==4).first()


# 查询名字以`张`开头的所有数据[startswith开始/endswith结尾/contains包含]
User.query.filter(User.name.startswith("张")).all()


# 查询手机号码为18516952650 同时  名字以 `号` 结尾的  [两种]

# 多个条件以逗号隔开表示条件并列 常用
User.query.filter(User.mobile=="18516952650", User.name.endswith("号")).first()


from sqlalchemy import and_,or_,not_
User.query.filter(and_(User.mobile=="18516952650", User.name.endswith("号"))).first()



# 查询手机号码为13021074747 或者  名字以 `号` 结尾的
# or_(条件1， 条件2)

User.query.filter(or_(User.mobile=="13021074747", User.name.endswith("号"))).all()

# 查询手机号码不是为13911111111  --- 非 

User.query.filter(User.mobile !="13911111111").all()

User.query.filter(not_(User.mobile =="13911111111")).all()

# offset 偏移，起始位置
User.query.offset(3).all()

# 只取前三条数据  limit 获取限制数据
User.query.limit(3).all()


# 根据id排序  order_by 降序排序
User.query.order_by(User.id.desc()).all()
# desc()：降序

# 分页查询
# 查询第一页的每一页10条用户数据
# 参数1：第几页数据 参数2：每一页多少条数据
p = User.query.paginate(1,10)

# 获取当前页码的所有数据
p.items

# 总页码
p.pages

# 当前页码
p.page

~~~

### 数据库查询优化

~~~
# 问题：
# 查询所有字段
user = User.query.filter_by(id=1).first()  

相当于: select * from User  #  消耗性能
    
# 查询特定字段:
from sqlalchemy.orm import load_only

User.query.options(load_only(User.name, User.mobile)).filter_by(id=1).first() # 查询特定字段

~~~


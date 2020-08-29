# Flask入门

### WEB工作原理

1. C/S和B/S架构

2. B/S工作原理

   客户端(浏览器) <=> WEB服务器(nginx/apache) <=> Python(Flask) <=> 数据库(MySQL)

### Flask框架

1. 简介

   是一个非常小的python web框架，被称为微型框架，只提供了一个强健的核心，其它的功能都要使用扩展来实现。意味着可以根据自己的项目需求量身打造。

2. 组成

   1. 调试、路由、WSGI系统
   2. 模板引擎(Jinja2，Flask的核心人员开发)

3. 安装

   `pip install flask`

4. 最小的Flask程序

   ```python
   # 导入Flask类库
   from flask import Flask
   # 创建应用实例
   app = Flask(__name__)
   # 创建视图函数
   @app.route('/')
   def index():
       return 'Hello Flask !'
   # 启动应用实例
   if __name__ == '__main__':
       app.run()
   ```

   > 默认访问地址：http://127.0.0.1:5000

5. 启动参数

   | 参数     | 说明                                                       |
   | -------- | ---------------------------------------------------------- |
   | debug    | 是否开启调试模式，开启后有错误提示，代码修改后可以重新启动 |
   | threaded | 开启多线程，默认是不开启的                                 |
   | port     | 指定端口号                                                 |
   | host     | 指定主机，设置为'0.0.0.0'后可以通过ip地址进行访问          |

7. 视图函数

   1. 不带参数的视图函数，见上

   2. 带参数的视图函数，如下：

      ```python
      # 带参数的路由，不指定参数类型，默认是字符串
      @app.route('/hello/<username>')
      def hello(username):
          return 'Hello %s !' % username
          
      # 参数可以指定类型：int、float、path(/不再是分隔符)
      @app.route('/user/<int:uid>')
      def user(uid):
          return '%d号用户详细信息' % uid

      @app.route('/path/<path:p>')
      def path(p):
          return p
      ```

   3. 总结说明

      1. 若指定参数，需要将参数外添加<>，且路由参数名称要与视图函数参数一致
      2. 参数可以指定类型，如：int、float、path，不指定默认为字符串
      3. 路由中的最后的'/'最好还是加上

8. 请求(request)

   ```python
   @app.route('/request/')
   def req():
       # 完整的请求URL
       #return request.url
       # 基本路由信息，不包含get参数
       #return request.base_url
       # 只包含主机和端口
       #return request.host_url
       # 只包含装饰器中的路由地址
       #return request.path
       # 请求方法类型
       #return request.method
       # 客户端IP地址
       #return request.remote_addr
       # 所有的请求参数(GET)
       #return request.args['page']
       # 请求头信息
       return request.headers['User-Agent']
   ```

9. 响应(response)

   ```python
   @app.route('/response/')
   def response():
       # 只要返回字符串就可以，默认状态码为200
       #return 'ok'
       # 可以指定状态码
       #return 'page not found', 404
       # 先使用专门的函数构造一个响应，然后返回，可以在构造时指定状态码
       resp = make_response('我是提前构造好的响应', 404)
       return resp
   ```

10. flask-script扩展



1. 安装：`pip install flask-script`

2. 说明：

   在项目测试完成后，上线时最好不要改动任何代码。只能通过终端的方式进行启动，通过传递不同的参数，完成特定的启动方式。很遗憾flask默认不支持命令行启动，然而幸运(^_^)的是有一个第三方库flask-script帮我们实现了这个功能。简单来说，它就是一个flask终端启动的命令行解析器。

3. 使用：

   ```python
   # 导入类库
   from flask_script import Manager
   # 创建对象
   manager = Manager(app)
   # 启动应用实例
   if __name__ == '__main__':
       #app.run(debug=True, threaded=True, port=8000, host='0.0.0.0')
       manager.run()
   ```

4. 启动参数

   ```
   -?，--help			# 查看帮助信息
   --threaded			# 开启多线程
   -d					# 开启调试模式
   -r					# 自动加载
   -h，--host			# 指定主机
   -p，--port			# 指定端口
   ```

### 蓝本(blueprint)

1. 说明：

   代码越来越复杂时，将所有的视图函数放在一个文件中是很不合理的，如果能够根据功能不同将特定的视图函数分类存放，蓝本就是为了解决这个问题而出现的

2. 使用：

   ```python
   # 导入类库
   from flask import Blueprint
   # 创建对象
   user = Blueprint('user', __name__)

   @user.route('/register/')
   def register():
       return '欢迎注册'
     
   # 蓝本注册，不注册时蓝本处于休眠状态  
   app.register_blueprint(user, url_prefix='/user')
   ```

   


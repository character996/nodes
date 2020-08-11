# django知识

## 项目创建运行

### 创建虚拟环境

在目录下执行python3 -m venv \<DIR>命令，创建虚拟环境。进入虚拟环境>，需要source <DIR\>/bin/activate激活，退出虚拟环境是 deactivate 命令。

### 创建Django项目  

>django-admin startproject mysite

### 运行python项目

>python manage.py runserver

开发中的服务器会对每一次请求载入一次代码，修改代码时无需重复启动服务，但是添加新文件动作需要重新启动。

创建的项目与应用的区别:应用是专门做某件事，项目是一个网站的配置应用集合。

创建应用

>python manage.py startapp polls

建立url   include函数，需要建立应用的url连接

建立路径  path函数

检查INSTALLED_APPS设置，为需要建立数据库的应用建立数据库

### 定义模型和元数据

数据迁移
>python manage.py sqlmigrate polls

可以输出数据库执行的操作set

### 改变模型需要三步

1. 编辑 models.py 文件，改变模型
2. 运行 python manage.py makemigrations 为模型的改变生成迁移文件
3. 运行 python manage.py migrate 来应用数据库迁移

manage.py 会设置 DJANGO_SETTINGS_MODULE 环境变量，这个变量会让 Django 根据 mysite/settings.py 文件来设置 Python 包的导入路径

### django设置

django设置在/venv/lib/python3.5/site-packages/django/conf/global_settings.py

时区设置  USE_TZ为True，Django使用系统默认时区，将USE_TZ = True, TIME_ZONE = 'Asia/Shanghai'，还有settings文件，USE_TZ=True后，数据库中存储的是UTC时间
render函数返回一个HttpResponse对象

get_object_or_404() 找一个对象，如果不存在就404，第一个参数是models，第二个是条件

### 通用视图：常见的模式抽象化

1. 改良URLconf
2. 改良视图
3. 在url中显示

基于类的视图,继承自view类，RedirectView 用于简单的 HTTP 重定向，TemplateView 扩展基类来使它能渲染模板。通用视图可以在urlconf中直接使用，也可以在views文件里定义。
ListView类视图、DetailView视图。

### 自动化测试

编写测试程序，python manage.py test polls寻找polls应用里的测试程序，在test.py文件中找到一个Testcase的子类，创建测试数据库，找到类中以test开头的测试方法，执行测试，使用assertls()方法来判断测试是否成功。

### 测试视图

通过模拟用户使用浏览器访问应用来检测。Client工具可以模拟用户和视图层代码的交互。
测试时，对于每个模型和视图都建立单独的TestClass，每个测试方法只测试一个功能，测试方法的名字可以描述他的功能。

### 静态文件

服务端生成的HTML、图片、脚本、样式表
django.contrib.staticfiles将各个应用中的静态文件统一收集起来，便于分发。
自定义后台表单，编辑admin.py
修改属性定制页面显示数据的格式

## MTV模式

### M模型：描述数据，一个模型类对应一个数据库表

定义 继承自models.Modes，设置表字段类型，字段参数，Meta属性。

#### 模型的查询

Models.objects.方法，filter、exclude返回查寻集，查寻集条件表示方法__双下划线 字段名__字段查询谓词，提供下标操作，get方法返回单条数，排序操作order_by。

#### 关系操作

一对一：OneToOneField，一对多：ForeignKey，一对多中，主模型可以通过 子模型名_set访问关联的子模型，多对多关系: ManyToManyfield

#### 模型类继承

抽象类继承、多表继承、代理模型继承

### V视图：处理HTTP请求、python程序、HTML模板

url映射、反向映射(模板文件中用{% url %}标签，程序中用reverse函数)

视图函数：处理HTTP请求，返回HTTP response，

直接返回HTTP页面：return HttpResponse()

返回模板渲染文件：return render()

返回HTTP错误：可以在此定义错误视图

### T模板：文本文件

{{变量内容}}，过滤器(管道符号 | )、流程控制、模板继承({% extend “base.html” %},父模板中的block内容可以被重写)

模板系统中统一使用点符号来访问变量的属性。首先对对象使用字典查找，然后属性查找，然后列表查找。
利用name去掉模板中的硬编码URL

不同应用中可能会有重名的views，防止模板文件引用时混乱，可以在urls.py添加app_name,然后在模板文件中 {% url 'polls:detail' question.id %}

### 表单：做一些输入文本、选择选项、操作对象或空间的动作，发送到服务端

表单必须指定：响应用户输入的url地址、数据请求使用的HTTP方法

Django表单系统有Form类，表单类的字段会映射到HTML表单中的\<input\>元素

渲染表单需要：在视图中获取、传递给模板、模板变量将它变为HTML标记，

form在模板语言中可以解包为HTML标记，通过POST方法提交启用了csrf的表单时，需要使用模板标签 csrf_token

表单用来编辑或添加Django模型时，用ModelForm类

表单绑定，表单对象被实例化后赋予过数据内容，会处于绑定(bound)状态,才可以进行表单数据验证，is_bound属性可以查看，验证过的数据会放到form.cleaned_data字典中。

Django可以自动渲染表单对象，可以指定形式{{ form.as_table | as_p | as_ul }}

继承ModelForm的form类，在表单提交后，可以clean方法验证字段，save方法将数据提交到数据库，Meta中的属性 model、fields、widges

需要自定义表单数据验证时，可以通过重载Form子类的clean()函数，表单函数has_changed判断数据是否修改，changed_data时修改过字段名列表

form应用widgets   widget = 部件名 或 widgets={字段名：部件名}，表单小部件在forms.widgets、admin.widgets

继承表单类，修改表单元素在页面中的显示效果，可以通过Meta类下的widgets、labels、erroe_messages来设置。

表单资源作为静态定义  在内部定义Media类

模板四个元素：变量{{}}、标签{% %}、过滤器{{  | }}、注释{# #}({% comment %} 提供多行注释）

过滤器 控制输出

Paginator类对结果集分页，分清Paginator类对象和Page类对象

static要在setting.py中配置。

### 从已有数据库中导入数据，创建模型

用python manage.py inspectdb查看
django使用pymysql库时，会有mysqlclient版本问题，可以修改源码，也可以使用mysqlclient库。

已有的数据表建立models.py会将列名变成属性名，列名不要定义为中文！

#### 分页显示数据时，不用专门弄一个url来显示page，可以将page作为参数，request.GET.get(‘page’)得到page的值

django在生成迁移文件后，如果删除此迁移文件，查看一下django_migrations表，看表中的迁移文件是否删除。

### 数据库优化

查看mysql语句执行时间

查看profile是否开启，数据库默认是不开启的。变量profiling是用户变量，每次都要重新启动

查看方法： `show variables like "%pro%";`

设置开启方法： `set profiling = 1;`

执行完sql语句后，`show profiles`可以查看sql语句的执行时间

测试完毕后，关闭参数：`mysql> set profiling=0`

在sql语句前面加上 explain 可以查看查询了多少行 `explain sql`

#### 查询优化

>Teacher.objects.all().select_related('subject') 可以聚合查询
>Teacher.objects.all().only('name', 'good_count', 'bad_count') 可以查询具体想要的列
>queryset = Teacher.objects.values('subject__name').annotate(good=Avg('good_count'), bad=Avg('bad_count'))    分组查询也支持

Django的ORM框架允许我们用面向对象的方式完成关系数据库中的分组和聚合查询。

#### 索引优化

索引就是根据表中的一列或若干列按照一定顺序建立的列值与记录行之间的对应关系表，实质上是一张描述索引列的列值与原表中记录行之间一 一对应关系的有序表。

索引类型：主键、外键、unique、普通索引(index)

创建索引
>CREATE <索引名> ON <表名> (<列名> [<长度>] [ ASC | DESC]) 只能添加普通索引和unique

或者
>ALTER TABLE 表名 ADD 索引类型 (unique,primary key,fulltext,index)[索引名](字段名)

like '% %'查询会遍历整个表，索引不起作用。

### 前后端分离

前端进行页面展示，后端提供数据，前后端约定数据交互接口，前端通过HTTP请求来获取数据。后端主要提供JSON格式的数据，调用JsonResponse对象来将字典对象进行处理，封装为JSON返回数据。

在创建应用前，应该首先确定号路由的分配，将api应用中不要处理模板，只返回数据。基于类的视图,继承自view类，RedirectView 用于简单的 HTTP 重定向，TemplateView 扩展基类来使它能渲染模板。通用视图可以在urlconf中直接使用，也可以在views文件里定义。

ListView类视图、DetailView视图。基于类的视图,继承自view类，RedirectView 用于简单的 HTTP 重定向，TemplateView 扩展基类来使它能渲染模板。通用视图可以在urlconf中直接使用，也可以在views文件里定义。
ListView类视图、DetailView视图。

REST 表述性状态传递，web实现方案，软件架构风格

### django 返回json类型数据，利用ajax实现页面数据刷新

#### 将查询集转变为json数据

serializers.serialize 将queryset转化，可以指定类型，和要转化的字段，用fields声明。

json.loads()将转化后的字符串变为python对象，放入最终要传递的对象中。

在利用JsonResponse将最终要传递的数据（可以是字典，也可以是json字符串）变为json对象，return

#### 前端利用ajax实现页面刷新

jquery中的ajax

```javascript
$.ajax({
	type:http类型,
	url:要访问的url,
	data:要传递的参数,
	success:function(res){
	res是访问成功后接收到的数据
})
```

ajax通过对页面元素的操作来进行页面更新。取用json数据时，先确定是object还是json字符串，需要时可以转换。取用元素使用  . 或 [key] 都可以。

循环取用元素，使用$each()来进行循环。

### 前端相关知识

#### js相关

js使用变量需要声明，var关键字，尤其是在嵌套循环中，如果使用的标志变量相同，会引起循环混乱。

使用js利用ajax接收到的数据添加元素的方法为拼接标签字符串，可以先将所要得到的标签写出来，再将变量添加进去，提高速度。

##### 事件代理

新添加的元素，不能直接给他添加事件。如果需要添加事件,可在页面原先存在的且为他的祖先元素中添加事件，叫做事件代理。以点击事件为例

 `$('#movie_info').on('click','.show_intro', function(){}`

.show_intro为新添加的元素，#movie_info是他的祖先元素

### 缓存

缓存的工作流程：

```
given a URL, try finding that page in the cache
if the page is in the cache:
    return the cached page
else:
    generate the page
    save the generated page in the cache (for next time)
    return the generated page
```

#### django中缓存分类

可以使用 Memcached、数据库、文件系统、本地内存、用于开发模式的虚拟缓存，也可以使用自定义的缓存后台。

### 设置数据库缓存

在setting文件中设置，还有一些非必须的缓存参数，TIMEOUT、OPTIONS等

```
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.db.DatabaseCache',
        'LOCATION': 'my_cache_table', #缓存表的名字
    }
}
```

然后创建缓存表
>python manage.py createcachetable

如果只是想查看创建缓存表的sql语句，可以使用
> createcachetable --dry-run

### 设置redis缓存

1. 安装redis
2. redis设置密码
3. 安装django-redis
4. 在setting文件中配置
```
CACHES = {
    'default': {
        'BACKEND': 'django_redis.cache.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379',
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
             "PASSWORD": "*******",
        },
    },
}
```

缓存设置完成后，选择需要在哪些页面中设置缓存。

#### 整个站点

添加中间件，位置不能变化，要在最上面和最下面添加中间件。

```
MIDDLEWARE = [
    'django.middleware.cache.UpdateCacheMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.cache.FetchFromCacheMiddleware',
]
```

#### 在视图中添加缓存或者在urlconf中指定

在视图中添加是给视图函数添加装饰器

>from django.views.decorators.cache import cache_page

在urlconf中指定
>cache_page(3.60)(views.tags)

#### 在模板中添加缓存

在模板中添加缓存需要指定两个参数，过期时间和缓存片段的名称

```
{% load cache %}
{% cache 500 sidebar %}
    .. sidebar ..
{% endcache %}
```

#### 底层缓存API（待补）



## django 项目部署

Django 的主要部署平台是 WSGI，它是 Web 服务器和 Web 应用的 Python 标准。

### gunicore

Gunicorn（'Green Unicorn'）是一个UNIX下一个纯Python WSGI服务器。

#### 安装

>pip install gunicorn  

#### 运行

##### 遇到问题

debug = False 后会碰到 500 错误，查阅文档后需要设置 `ALLOWED_HOSTS`，设置之后依旧报错。（未解决）

>gunicorn showmovie.wsgi

运行后，会出现静态文件无法加载的现象，需要将静态文件收集起来。  
在 settings 文件中添加 `STATIC_ROOT = os.path.join(BASE_DIR, "static/")`，然后执行
>python manage.py collectstatic

此时运行之后，可以在浏览器中访问，静态文件可以加载出来。

### uWSGI

uWSGI是一个Web服务器，它实现了WSGI协议、uwsgi、http等协议。Nginx中HttpUwsgiModule的作用是与uWSGI服务器进行交换。  

* WSGI是一种通信协议。
* uwsgi是一种线路协议而不是通信协议，常用于在uWSGI服务器与其他网络服务器的数据通信。
* uWSGI是实现了uwsgi和WSGI两种协议的Web服务器。  

#### 安装运行

> pip install uwsgi  

##### 命令行中带参数启动

> uwsgi --http :8000 --file showmovie/wsgi.py   # 指定端口号和配置文件路径

##### 使用初始化文件启动

新建 uwsgi.ini 文件，写入
```
[uwsgi]

# Django-related settings
# the base directory (full path)
chdir           = /home/scanv/PycharmProjects/movie/movie_git
# Django's wsgi file
module          = showmovie.wsgi:application
# the virtualenv (full path)
home            = /home/scanv/PycharmProjects/test/venv
# process-related settings
# master
master          = true
# maximum number of worker processes
processes       = 1
# pid file
pidfile         = /home/scanv/PycharmProjects/movie/movie_git/uwsgi/uwsgi.pid
# The address and port of the monitor
http            = :8000
# clear environment on exit
vacuum          = true
#The process runs in the background and types the log to the specified log file
daemonize       = /home/scanv/PycharmProjects/movie/movie_git/uwsgi/uwsgi.log
```

使用初始化文件启动

> uwsgi --ini uwsgi.ini # 启动  
> uwsgi --reload uwsgi.pid # 重启  
> uwsgi --stop uwsgi.pid # 关闭

报错 `!!! no internal routing support, rebuild with pcre support !!!`，重新编译 uwsgi
> pip uninstall uwsgi  
> sudo apt-get install libpcre3 libpcre3-dev  
> pip install uwsgi --no-cache-dir #加上–no-cache-dir,可以强制下载重新编译安装的库，不然pip会直接从缓存中拿出了上次编译后的 uwsgi 文件，并没有重新编译一份。

重新启动后，访问站点可以显示内容，说明 uwsgi 部署 django 项目没有问题，接下来配置 nginx。  

#### Nginx

使用 nginx + uwsgi 部署django，它的流程是：  
>the web client <-> the web server <-> the socket <-> uwsgi <-> Django

nginx 安装配置
>apt-get install nginx  

nginx 配置  
新建 movie.conf 文件，写入  

```
server {
        listen 8000;
        server_name localhost;

        # 指定项目路径uwsgi
        location / {
            include uwsgi_params; # 导入一个Nginx模块他是用来和uWSGI进行通讯的
            uwsgi_connect_timeout 30; # 设置连接uWSGI超时时间
            uwsgi_pass unix:/home/scanv/PycharmProjects/movie/movie_git/movie.sock; # 指定uwsgi的sock文件所有动态请求就会直接丢给他
            }

        # 指定静态文件路径
        location /static/ {
            alias /home/scanv/PycharmProjects/movie/movie_git/static/;
            }

    }

```

注意 uwsgi_pass 和 uwsgi.ini 中的 socket 相同。配置完 nginx 之后，还需要更改 uwsgi.ini  

```
# http            = 127.0.0.1:8000 #http 注释掉
socket          = /home/scanv/PycharmProjects/movie/movie_git/movie.sock #设置 socket 路径

# 其余设置不用更改
```

配置完成之后，`nginx -s reload` 重新加载 nginx，此时访问 8000 端口，可以看到首页信息。  

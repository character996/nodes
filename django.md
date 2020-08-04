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

### 数据库查询优化

Teacher.objects.all().select_related('subject') 可以聚合查询

Teacher.objects.all().only('name', 'good_count', 'bad_count') 可以查询具体想要的列

queryset = Teacher.objects.values('subject__name').annotate(good=Avg('good_count'), bad=Avg('bad_count'))    分组查询也支持

Django的ORM框架允许我们用面向对象的方式完成关系数据库中的分组和聚合查询。

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

### js相关

js使用变量需要声明，var关键字，尤其是在嵌套循环中，如果使用的标志变量相同，会引起循环混乱。


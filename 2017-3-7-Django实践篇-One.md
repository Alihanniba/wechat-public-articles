>上午改了几个小BUG，下午折腾了一下午的Python，熟悉了下Django框架的开发模式

上手难度整体来说个人感觉比Laravel要简单一点，还有就是Laravel玩家对PHP语言的熟悉程度以及Django玩家对Python语言的熟悉程度。本身接触Python语言时间不长，系统的学习还只是从上周末开始，直接上框架难免基础不稳，但还是想在框架实践中慢慢熟悉这门语言吧，毕竟马上项目就要开始了。

Python环境之前就安装好了，直接：

```
pip install django
```

创建项目

```
django-admin startproject mysite
```

新建app

```
python manage.py startapp articles
```

启动服务

```
python manage.py runserver  8001
```

生成模型

```
python manage.py makemigrations
```

创建表

```
python manage.py migrate
```

因为主要是想用Django来为论坛写接口，前端并不想用内部模板，用 Dva + React + antd 的技术栈更利于开发速度及部署上线。于是翻过模板这块，直接配置Mysql数据库在Django中的配置文件及相关连接库，配置路由。后端接口无非就是增删改查几个模块，慢慢摸索的过程中，对学习能力及方向的把控稍稍有了点感觉。

圈子里有句话说：

>写一个方法，C语言要写1000行代码，Java只需要写100行，而Python可能只要20行

真的有感觉到，一个查询接口真的只需要三行就能搞定，并不是说其他语言接口不能这么简洁，只是想表示python的封装库应该算是语言生态圈最欣欣向荣的了。

```
def index(request):
    return HttpResponse(serializers.serialize("json", Articles.objects.all()))

```
<img src="https://share.alihanniba.com/mac/2017-03-07-Screen%20Shot%202017-03-07%20at%206.25.22%20PM.png" height="350" width="300" style="margin: 0 auto; display:block;" />

---
**over，明天继续**
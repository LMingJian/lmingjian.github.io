---
title: Django 网页的部署
date: 2021-08-27
author: LM
---

## 1.Django 网页的部署

Django 网页的部署需要用到 uwsgi，Nginx，以及具备 Python3 的系统环境。

## 2.配置 uwsgi

- 安装：`pip install uwsgi`
- 测试：`uwsgi --http-socket :80 --file test.py`

```python
# test.py
def application(env, start_response):
    start_response('200 OK', [('Content-Type','text/html')])
    return [b"Hello World"]
```

- 配置文件：`uwsgi.ini`

```ini
[uwsgi]
# 使用nginx连接时使用
# socket=0.0.0.0:8000
# 直接做web服务器, python manage.py runserver ip:port
http=0.0.0.0:8000
# 路径为 0.0.0.0，表本地，使用127.0.0.1可能会无法从外网访问
# 项目目录
chdir=/home/mayanan/bj18/dailyfresh
# 项目中wsgi.py文件的目录，相对于项目目录
wsgi-file=dailyfresh/wsgi.py
# 指定启动的工作进程数
processes=4
# 指定工作进程中的线程数
threads=2
master=True
# 保存启动后，主进程的pid
pidfile=uwsgi.pid
# 设置uwsgi后台运行, uwsgi.log保存日志信息
daemonize=uwsgi.log
```

- 启动：`uwsgi --ini uwsgi.ini`，uwsgi 通过 ini 文件启动后会在相同目录下生成一个 pid 文件，包含主进程的进程号
- 重载：`uwsgi --reload uwsgi.pid`
- 停止：`uwsgi --stop uwsgi.pid`

## 3.配置 Django

- 测试：`python manage.py runserver 0.0.0.0:8000`，访问宿主机 IP:8000
- 配置：`setting.py`，`DEBUG=False`，`ALLOWED_HOSTS=['*']`
- 配置：`wsgi.py`，默认情况下以配置完成

```python
import os
from django.core.wsgi import get_wsgi_application

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "mysite.settings")
application = get_wsgi_application()
```

## 4.连接 Django 与 uwsgi

- 测试：`uwsgi --ini uwsgi.ini`，ini 内容需指定项目目录与 wsgi 文件
- 访问宿主机 IP:端口，所有的请求都会经过 uwsgi 然后传递给 Django

## 5.Nginx

- Docker 安装：`docker pull nginx`，`docker run --name some-nginx --privileged=true -v /your-content:/usr/share/nginx/html:ro -d -p 8000:80 nginx`
- 检查：`nginx -v`，`/etc/init.d/nginx status`
- 启动：`/etc/init.d/nginx start`
- 关闭：`/etc/init.d/nginx stop`
- 重启：`/etc/init.d/nginx restart`

## 6.连接 Nginx 与 uwsgi

- uwsgi 配置：修改 `uwsgi.ini`，启用`socket=0.0.0.0:8080`，使用端口通信而不再作为服务器
- 修改 Nginx 配置文件：`cd /etc/nginx`，可以通过`nginx -V`查看，属性`--conf-path`

```nginx
# vim nginx.conf
.....
http{
    .......
    # include /etc/nginx/conf.d/*.conf 可能需要前往这以修改 server 块配置
    server{
        location / {
            include uwsgi_params;
            # 转交请求给uwsgi，填uwsgi服务地址
            uwsgi_pass 127.0.0.1:8000;
        }
    }
}
```

- 重启，能访问网页，但无静态文件
- 配置静态文件

```nginx
# vim nginx.conf
.....
http{
    .......
    # include /etc/nginx/conf.d/*.conf 可能需要前往这以修改 server 块配置
    server{
        location / {
            include uwsgi_params;
            # 转交请求给uwsgi，填uwsgi服务地址
            uwsgi_pass 127.0.0.1:8000;
        }
        
        location /static {
            # 静态文件路径
            alias /root/static/;
        }
    }
}
```

- Django 收集静态文件：`python manage.py collectstatic`

```python
# 指定收集静态文件的路径
STATIC_ROOT = os.path.join(BASE_DIR, "static/")
STATICFILES_FINDERS = (
 
"django.contrib.staticfiles.finders.FileSystemFinder",
 
"django.contrib.staticfiles.finders.AppDirectoriesFinder"
 
)
```

## 7.网络拓扑

通过上述配置，当用户访问网页时，请求会先经过Nginx，然后Nginx会判断是否是动态请求`/`，若是则将请求转交给 uwsgi ，由 uwsgi 再转交给 Django；若请求是静态请求 `/static`，那么 Nginx 将直接从收集的静态文件目录中将资源找到，返回给浏览器。

## 8.扩展

当用户访问网页时，有部分 html 文件可以被直接访问，对于这些，可以使用多个 Nginx 服务器分开处理，以节省 uwsgi 的负担。

第一个nginx服务，用来提交可直接打开的静态页面

```nginx
 # 配置服务提交静态页面
    server {
        listen 9999;
        server_name localhost;
 
        location /static {
            alias  /home/mayanan/bj18/dailyfresh/static/;
        }
 
        location / {
            #root   html;
            root  /home/mayanan/bj18/dailyfresh/static/;
            index  index.html index.htm;
        }
 
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
 
    }
```

第二个 nginx 服务，精确匹配`/`并转交请求给静态文件服务器 nginx，模糊匹配`/`转交请求给 uwsgi，`/static`静态文件匹配，从收集的静态文件目录中找到静态文件，直接返回给浏览器

```nginx
server {
        listen       80;
        server_name  localhost;
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
 
        # = 代表精确匹配
        location = / {
                # 传递请求给静态文件服务器的nginx
                proxy_pass http://192.168.13.128:9999;
                #root  /home/mayanan/bj18/dailyfresh/static/;
                #index  index.html index.htm;
        }
 
        location / {
                # 包含uwsgi的请求参数
                include uwsgi_params;
                # 转交请求给uwsgi
                uwsgi_pass 127.0.0.1:8080;
        }
 
        location /static {
                # 指定静态文件存放的目录
                alias /home/mayanan/bj18/dailyfresh/static/;
        }
 
        #error_page  404              /404.html;
 
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
         location = /50x.html {
            root   html;
        }
    
        # ......
 
    }
```


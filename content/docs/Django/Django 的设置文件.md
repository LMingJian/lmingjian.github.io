---
title: Django 的设置文件
date: 2024-12-25T11:08:00+08:00
author: LiangMingJian
---

# setting.py

`setting.py`是 Django 项目的配置文件，有关项目的一切设置都在该文件中进行填写，包括以下的内容。

- `BASE_DIR`：即为项目所在目录，`__file__`可以获得当前文件的路径。
- `SECRET_KEY`：一个特殊的 Django 安装的密钥，每当使用`Django-admin startproject`时会自动生成一个。
- `DUBUG`：默认值为 FALSE，当选择 TRUE 时，当我们的项目出错时可以使我们看到出错信息，但是为了防止被用户看到或者他人攻击，在项目上线后应改为 FALSE。
- `ALLOWED_HOSTS`：默认值是一个空列表，列表中的值为哪些域名可以访问我们的 Django 项目。
- `INSTALLED_APPS`：安装的 APP 列表，Django 为我们默认添加了一些自带的项目，每个新创建的 APP 都要加入这个列表才可以被使用。
- `MIDDLEWARE`：这是将要使用的中间件列表。
- `ROOT_URLCONF`：表示根 URLconf 的完整 Python 导入路径。
- `TEMPLATES`：模板文件配置。
- `WSGI_APPLICATION`：配置 Django 项目的 WSGI 服务路径。
- `DATABASES`：Django 的数据库设置，Django 默认的是 sqlite3 数据库。ENGINE 是选择对应我们选择的数据库的引擎，NAME 是数据库名称，HOST 是连接数据库所要用到的主机，还有 PORT 选择端口等许多选项。
- `AUTH_PASSWORD_VALIDATORS`：用于检查用户密码强度的验证器列表，在为空的情况下就接受任意强度的用户密码。
- `LANGUAGE_CODE`：Django 项目的语言代码，默认值为 en-us，值 zh-hans 汉语。
- `TIME_ZONE`：时区，默认值是 UTC。当 USE_TZ 为 TRUE 时，无论 TZ 设置为何值 Django 都会使用系统默认的时区，例如要使用上海的时区则需将 USE_TZ=FALSE，TIME_ZONE='Asia/Shanghai'。
- `USE_I18N`：国际化，Django 允许开发者指定要翻译的字符串，也可以让访问者进行语言选择。
- `USE_L10N`：是否选择启用数据的本地化。
- `USE_TZ`：如果开启了 Time Zone 功能，则所有的存储和内部处理，甚至包括直接 print 显示全都是UTC的。只有通过模板进行表单输入/渲染输出的时候，才会执行 UTC 本地时间的转换。
- `STATIC_URL`：静态目录的所有文件，存放 css，js 等文件。
- `STATICFILES_DIRS`：static 文件的路径。
- `MEDIA_URL`：与 STATIC_URL 类似，存放用户上传的文件。
- `SECURE_CROSS_ORIGIN_OPENER_POLICY`：跨域检测是否启用。

# 示例

```python
# 导入 os 包对文件进行操作管理
import os
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

# 随机生成的密钥
SECRET_KEY = '^#kms19!iawj2b&v3egmynpfwj8^v@2f(_1+jlw+#^vy^pg7oy'

# 调试环境
DEBUG = False

# 允许的主机路径
# 不填写或者 ALLOWED_HOSTS = ["*"] 代表允许任意主机域名
# 若域名只允许为 www.baidu.com 那么 ALLOWED_HOSTS = ["www.baidu.com"]
ALLOWED_HOSTS = []

# 注册项目应用 app，只有注册后才能进行模型同步等操作
INSTALLED_APPS = [
  'django.contrib.admin',
  'django.contrib.auth',
  'django.contrib.contenttypes',
  'django.contrib.sessions',
  'django.contrib.messages',
  'django.contrib.staticfiles',
]

# django 中间件注册
MIDDLEWARE = [
  'django.middleware.security.SecurityMiddleware',
  'django.contrib.sessions.middleware.SessionMiddleware',
  'django.middleware.common.CommonMiddleware',
  'django.middleware.csrf.CsrfViewMiddleware',
  'django.contrib.auth.middleware.AuthenticationMiddleware',
  'django.contrib.messages.middleware.MessageMiddleware',
  'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# 主路由，也就是项目的主urls(根urls)注册
ROOT_URLCONF = 'untitled.urls'

# 模版的注册，包括模板路径，需要使用的模块等
TEMPLATES = [
  {
    'BACKEND': 'django.template.backends.django.DjangoTemplates',
    'DIRS': [os.path.join(BASE_DIR, 'templates')]
    ,
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

# WSGI 应用程序定义
WSGI_APPLICATION = 'untitled.wsgi.application'

# 数据库配置
DATABASES = {
  "default": {
    "ENGINE": "django.db.backends.mysql",
    "NAME": "practice", # 需要自己手动创建数据库
    "USER": "root",
    "PASSWORD": "318",
    "HOST": "127.0.0.1",
    "POST": 3306
  }
}

# 验权配置
AUTH_PASSWORD_VALIDATORS = [
  {
    'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
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

# 语言 如中文: LANGUAGE_CODE = 'zh-hans'
LANGUAGE_CODE = 'en-us'
# 时区  如中国上海时区: TIME_ZONE = 'Asia/Shanghai'
TIME_ZONE = 'UTC'
# 国际化
USE_I18N = True
USE_L10N = True
# 系统时区
USE_TZ = True

# logging 模块配置
LOGGING = {
  'version': 1,
  'disable_existing_loggers': False,
  'handlers': {
    'console':{
      'level':'DEBUG',
      'class':'logging.StreamHandler',
    },
  },
  'loggers': {
    'django.db.backends': {
      'handlers': ['console'],
      'propagate': True,
      'level':'DEBUG',
    },
  }
}

# 不启用跨域检测
SECURE_CROSS_ORIGIN_OPENER_POLICY = 'None'

# 静态文件配置
STATIC_URL = '/static/'
"""
STATICFILES_DIRS = [
  os.path.join(BASE_DIR, 'static'),
  ]
"""
# 自定义时间显示格式 如:
"""
DATE_FORMAT = 'Y-m-d'
DATETIME_FORMAT = 'Y-m-d H:i:s'
"""
# 定义视图限制 如:
"""
MAX_CUSTOMER_NUM = 3 #数量限制
"""
```

---
title: 基于 FastAPI+Uvicorn 的 Python 后端框架介绍
date: 2026-05-21T09:17:12+08:00
author: LiangMingJian
---

# 前言

在 AI 大时代，我们经常需要通过 AI 模型来创建各种 Agent 后端，为前端用户提供各式各样的 AI 功能，比如语音分析，人脸识别和 AI 对话等。此时，使用 Python 作为后端的编程语言是一个不错的选择（Python 有着当前最完善的 AI 工具链，覆盖从数据处理到模型部署的全流程，大幅降低开发难度，加快开发速度）。

而在 Python 中，使用基于 FastAPI+Uvicorn 的后端框架可以让我们的后端项目足够现代化，高性能，以及易维护。

# FastAPI 与 Uvicorn 是什么

**FastAPI 是什么？**

FastAPI 是 Python 的一个现代化，高性能的 Web 框架，专门用于搭建 API 接口。

FastAPI 的特点是速度快（运行速度接近 Node.js 和 Go）、代码少、支持自动生成 OpenAPI 或 Schema 的标准接口文档。

在基于 FastAPI+Uvicorn 的后端框架中，FastAPI 是“大脑”，主要负责路由处理、请求验证、功能实现，响应生成等核心业务逻辑。

**Uvicorn 是什么？**

Uvicorn 是一个轻量级、高性能的 ASGI 服务器，专门用于运行 Python Web 服务。

ASGI 是异步服务器网关接口（Asynchronous Server Gateway Interface）的缩写，由 Django 团队提出，用于处理各种网络协议服务（HTTP，HTTPS，WebSocket）和 Python 后端接口应用之间的连接。ASGI 通过事件循环机制，在单个进程中同时处理数千个并发连接，异步的处理各种不同网络协议所请求的功能。‌‌‌

在基于 FastAPI+Uvicorn 的后端框架中，Uvicorn 是“手脚”，主要为 FastAPI 提供对外访问的能力（如果只运行 FastAPI 代码，你是不能通过网络访问目标服务的，只有通过 Uvicorn 这些 ASGI 服务器来运行 FastAPI 代码，才可以正常访问目标服务）。

> 简单来说，把 FastAPI 看作插头，那 ASGI 就是插头的制式（两头的，三头的），Uvicorn 就是插座。只要你的“插头”是符合“制式”的，插入对应的“插座”就能让它运行起来。

# 一个常用的后端项目结构

在日常工作中，一个常用的 FastAPI+Uvicorn 的后端项目可以按以下的文件目录创建：

```python
project/
├── app/                     # 应用目录
│   ├── __init__.py          # Package 标志
│   ├── main.py              # 应用主程序
│   ├── config.py            # 配置文件
│   ├── api/                 # API 接口路由
│   │   ├── __init__.py
│   │   ├── deps.py          # API 依赖文件
│   │   └── v1/              # API 版本
│   │       ├── __init__.py
│   │       ├── auth.py      # 认证接口实现
│   │       ├── user.py      # 用户接口实现
│   │       └── school.py    # 学校接口实现
│   ├── core/                # 核心组件
│   │   ├── __init__.py
│   │   ├── database.py      # 数据库组件
│   │   ├── exceptions.py    # 异常组件
│   │   ├── logging.py       # 日志组件
│   │   └── security.py      # 安全组件
│   ├── models/              # SQLAlchemy ORM 数据库模型组件
│   │   ├── __init__.py
│   │   ├── user.py          # 用户数据库 ORM 模型
│   │   └── school.py        # 学校数据库 ORM 模型
│   ├── schemas/             # Pydantic 数据验证模式组件
│   │   ├── __init__.py
│   │   ├── user.py          # 用户接口的请求/响应模型
│   │   ├── school.py        # 学校接口的请求/响应模型
│   │   └── response.py      # 统一格式接口的请求/响应模型
│   ├── services/            # 业务逻辑层
│   │   ├── __init__.py
│   │   ├── user_server.py   # 用户业务功能逻辑实现
│   │   └── school_server.py # 学校业务功能逻辑实现
│   ├── static/              # 静态资源文件（可选）
│   │   └── index.html       # 主页文件（可选）
│   └── utils/               # 通用工具组件
│       ├── __init__.py
│       └── tools.py         # 常用的工具
├── data/                    # 数据库文件夹
├── docs/                    # 文档文件夹
├── logs/                    # 日志文件夹
├── models/                  # AI 模型文件夹
├── test/                    # 测试用例文件夹
├── .env                     # 环境变量
├── .gitignore               # git 忽略文件
├── requirements.txt         # Python 依赖声明
├── pyproject.toml           # 依赖也可以是适用于 Uv 的 pyproject.toml
├── README.md                # 项目说明
├── run.py                   # 项目运行脚本
├── docker-compose.yml       # Docker 编排（可选）
└── Dockerfile               # Docker 镜像构建（可选）
```

针对上述框架结构，下面会逐个进行解析。

# app/main.py

`app/main.py`：这是**应用的“主函数”**，里面应当写入包括应用实例化，路由注册，应用生命周期处理等内容。

一个常规的 `app/main.py` 可以参照下面的示例进行编写：

```python
from contextlib import asynccontextmanager
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware

@asynccontextmanager
async def lifespan(app: FastAPI):  # noqa
    # 应用生命周期管理
    # yield 前的代码在应用启动时执行
    print('应用启动中')
    yield
    # yield 后的代码在应用关闭时执行
    print('应用关闭中')

# 实例化应用
app = FastAPI(
    title="FastAPI 示例服务",
    description="FastAPI 示例服务",
    version="1.0.0",
    lifespan=lifespan,
)

# 注册 CORS 中间件  
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# 导入路由
from app.api.v1.auth import router as auth_router
from app.api.v1.user import router as user_router  
from app.api.v1.school import router as school_router  

# 注册路由，prefix 是接口前缀，会自动添加到 auth_router 中定义的路由前面
app.include_router(auth_router, prefix="/api/v1/auth", tags=["认证"])
app.include_router(user_router, prefix="/api/v1/user", tags=["用户"])  
app.include_router(school_router, prefix="/api/v1/school", tags=["学校"])

# 挂载一个静态的测试主页（可选）
@app.get("/", tags=["主页"])  
async def index():  
    html_path = Path(__file__).resolve().parent / "static" / "index.html"  
    return FileResponse(html_path)

# 挂载一个简单的服务检测接口
# 当 / 不想挂载 html 时，把这里的返回替换到 / 路由下面即可
@app.get("/health", tags=["检测"])  
async def health(): 
    return {
        "code": 0,
        "message": "success",
    }
```

# app/config.py

`app/config.py`：这是**应用的配置部分**，里面通过 `pydantic_settings` 结构化的写入应用配置，同时支持通过环境变量或 `.env` 读取配置参数。

一个标准的 `app/config.py` 可以参照下面的示例进行编写：

```python
from pydantic_settings import BaseSettings, SettingsConfigDict

class Settings(BaseSettings):
    # 应用配置
    APP_NAME: str = "FastAPI Name"
    DEBUG: bool = False

    # 数据库配置
    DATABASE_URL: str = "sqlite+aiosqlite:///./data/database.db"

    # 日志配置  
    LOG_LEVEL: str = "INFO"
    LOG_ROTATION: str = "00:00"

    # 指定 .env 文件路径和编码
    model_config = SettingsConfigDict(  
        env_file=".env",
        env_file_encoding="UTF-8",
        case_sensitive=True,  # 区分大小写
        extra="ignore"        # 忽略环境中多余的配置项
    )

# 创建全局实例（项目其他文件通过变量 setting 来调用配置项）  
settings = Settings()
```

注意：当同一个配置项在多个地方定义时，`pydantic-settings` 从高到低遵循以下优先级：

1. ‌传入的参数‌：实例化 `Settings()` 时传入的参数。
2. ‌环境变量‌：操作系统的环境变量。
3. ‌.env 文件‌：在 `model_config` 中指定的 `.env` 文件中的值。
4. ‌默认值：在 `Settings` 类定义中的默认值。

# app/api/

`app/api/`：这是**应用的接口定义**，这里定义了接口的路由，接口的参数，以及接口的响应（注意，接口的业务逻辑，功能实现不建议写在此处，应当写在 `app/services`）。

一个标准的 `app/api/` 可以包含如下的文件。

```
app/api/ 
│   ├── __init__.py
│   ├── deps.py
│   └── v1/
│       ├── __init__.py
│       ├── auth.py
│       ├── user.py
│       └── school.py  
```

其中 `deps.py` 包含可以给接口定义复用的一些工具函数，比如数据库连接，获取认证，数据校验等。

一般来说，可以将下述功能编写到 `deps.py` 文件中：

```python
# 从 core 的数据库组件中导入数据库实例
from app.core.database import async_session

# 获取数据库会话
async def get_db(): 
	async with async_session() as session:  
        try:  
            yield session  
            await session.commit()  
        except Exception:  
            await session.rollback()  
            raise
```

而文件夹 `v1` 则包含对应接口路的由定义，一般来说，可以按接口的功能来命名代码文件。文件夹 `v1` 表示接口的版本。

以 `app/api/auth.py` 为例，一个标准的接口路由定义文件可以这样编写：

## POST 接口

```python
from fastapi import APIRouter, Depends, Body, Query, HTTPException, status
from sqlalchemy.ext.asyncio import AsyncSession

from app.services.auth_service import AuthService
from app.api.deps import get_db

# 定义 FastAPI 核心路由组件
router = APIRouter()

# 定义一个 POST 接口，路由 /login
@router.post("/login", tags=["登录"])
async def login( 
	# 通过 Body 规范传入参数来自请求体（JSON）
	# Body(...) 表示这是一个必填参数，description 是参数的描述
    username: str = Body(..., description="账号"),        
    password: str = Body(..., description="密码"), 
    # Body(default=None) 表示这个是一个可选参数，可为空
    school: int | None = Body(default=None, description="学校"),
    # 依赖注入，FastAPI 会在调用这个接口前自动执行这个函数
    db: AsyncSession = Depends(get_db),
) -> dict:

	# 业务功能实现
	auth_service = AuthService(db)
	user_obj = await auth_service.login(username, password)

	# 响应返回
	return { 
	    "code": 0,  
	    "message": "success",  
	    "data": {  
	        "id": user_obj.id,  
	        "name": user_obj.name,  
	        "token": user_obj.token,   
	    },
	}
```

## GET 接口

```python
...

# 定义一个 GET 接口，路由 /keys	
@router.get("/keys", tags=["认证"]) 
async def list_keys(
	# 通过 Query 规范传入参数来自请求参数（即路由 ? 后面跟随的参数）
	# Query(0) 表示默认值 0，ge=0 表示必须参数范围必须大于等于 0
    page: int = Query(0, ge=0, description="页数"),  
    page_nums: int = Query(100, ge=1, le=1000, description="每页的数量"),
    # 依赖注入
    db: AsyncSession = Depends(get_db),  
) -> dict:  

	# 业务功能实现
	auth_service = AuthService(db)
    api_keys = await auth_service.get_keys(page, page_nums)
  
    # 响应
    return {  
        "code": 0,  
        "message": "success",  
        "data": [  
            {  
                "id": key.id,
                "name": key.name,  
                "is_active": key.is_active,
            }  
            for key in api_keys  # 列表推导式
        ],  
    }
```

## DELETE 接口

```python
...

# 定义一个 DELETE 接口，路由 /keys/{key_id}
@router.delete("/keys/{key_id}", tags=["认证"])  
async def delete_api_key(
	# 当不使用 Query, Body 时，传入参数会作为路径参数使用
    key_id: int,
    # 依赖注入
    db: AsyncSession = Depends(get_db),  
) -> dict:  

	# 业务功能实现
	auth_service = AuthService(db)  
    deleted = await auth_service.delete_key(key_id)  
  
    if not deleted:
	    # 抛出错误响应
        raise HTTPException(
	        # 状态码使用 404 
            status_code=status.HTTP_404_NOT_FOUND,
            detail={  
                "code": "NOT_FOUND",  
                "message": "API Key 不存在",  
                "data": None,  
            }, 
        )  
	
	# 返回正确的响应
    return {  
        "code": 0,  
        "message": "success",  
        "data": None,  
    }
```

# app/core/

`app/core/` 是**应用的服务器工具组件**，除业务功能外，作用于全局服务的工具都应写在该模块，包括但不限于数据库，日志，安全认证。

比如最主要的数据库组件 `database.py`，可以参照以下的示例进行编写：

```python
from pathlib import Path
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine, async_sessionmaker

from app.config import settings
from app.models.base import Base

# 检查数据库目录是否存在，当前项目使用数据库 SQLite（小型，简单）
_db_path = settings.DATABASE_URL.replace("sqlite+aiosqlite:///", "")  
_db_dir = Path(_db_path).parent  
_db_dir.mkdir(parents=True, exist_ok=True)

# 通过 SQLAlchemy 创建异步数据库引擎  
engine = create_async_engine(
	settings.DATABASE_URL,  # 数据库地址，格式应当为：数据库名称+引擎:///，不同的数据库有不同的格式
	echo=True,              # 将所有生成的 SQL 语句输出到控制台，设置为 False 可以关闭
	pool_size=10,           # 连接池中固定的连接数
	max_overflow=20,        # 允许超出 pool_size 的最大额外连接数
	pool_timeout=30,        # 获取连接的超时时间（秒）
	pool_recycle=1800,      # 连接回收时间（秒），用于防止数据库断开空闲连接
)
  
# 通过 SQLAlchemy 创建异步会话工厂
# 看向 app/api/deps.py，get_db() 方法通过 async_sessionmaker() 实例化一个数据库会话对象 
async_session = async_sessionmaker(  
    engine,                 # 数据库引擎              
    class_=AsyncSession,    # 会话类型
    expire_on_commit=False, # Commit 更新会话后是否重新查询数据库，一般不重新查询
)

# 初始化数据库，如果不存在数据表，则根据 Model 配置，创建所有数据库表
async def init_db() -> None:  
    async with engine.begin() as conn:
	    # Base.metadata.create_all 创建所有数据库表，其介绍查看 app/models 一节
        await conn.run_sync(Base.metadata.create_all) 

# 关闭数据库连接
# 显然，init_db() 和 close_db() 这两个方法应当在 main.py 中的 lifespan() 使用
# 在应用开启时执行和应用关闭时执行
async def close_db() -> None:  
    await engine.dispose()
```

# app/models/

`app/models/` 是**应用的数据库模型**，这里编写着数据库表的 ORM 模型，通过统一格式的模型，利用 SQLAlchemy 无损的迁移至不同的数据库。

在 SQLAlchemy 中，用户需要基于 Alembic 的命名约定来定义表结构。通过将配置了 `NAMING_CONVENTION` 的 `MetaData` 对象绑定到声明性基类 `Base` 上来完成这个操作。

对应的代码如下，在 `app/models/base.py` 中加入以下代码：

```python
from sqlalchemy import MetaData
from sqlalchemy.orm import DeclarativeBase

# 命名约定（用于 SQLAlchemy 数据库迁移）  
NAMING_CONVENTION = {  
    "ix": "ix_%(column_0_label)s",  
    "uq": "uq_%(table_name)s_%(column_0_name)s",  
    "ck": "ck_%(table_name)s_%(constraint_name)s",  
    "fk": "fk_%(table_name)s_%(column_0_name)s_%(referred_table_name)s",  
    "pk": "pk_%(table_name)s",  
}  

metadata = MetaData(naming_convention=NAMING_CONVENTION)  

# 继承 DeclarativeBase 声明性基类并绑定 metadata 
class Base(DeclarativeBase):    
	metadata = metadata
```

当声明性基类 `Base` 在被用户自定义的模型类继承后，在数据库表初始化（执行 `Base.metadata.create_all`）时，SQLAlchemy 会自动使用绑定的 `metadata` 来生成对应的表名，主键、外键、唯一约束等。

基于上述定义，对于用户自定义的数据库表，可以参照下面的示例进行编写（`app/models/user.py`）：

```python
from datetime import datetime

from sqlalchemy import Integer, String, Boolean, DateTime  
from sqlalchemy.orm import Mapped, mapped_column, relationship

from app.models.base import Base

# User 表模型
class User(Base):

	# 定义表名
    __tablename__ = 'users'

    # 定义主键 id
    # primary_key 主键；autoincrement 自增；
    # 因为 primary_key 所以自动生成约束名 pk_users
    id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)

    # 定义字段 username
    # unique 唯一字段；nullable 非空；index 生成索引
    # 同样自动生成约束名 uq_users_username
    username: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)

    # 定义字段 password_hash
    password_hash: Mapped[str] = mapped_column(String(255), nullable=False)

    # 定义字段 created_time
    # server_default 默认值；func.now() SQLAlchemy 提供的当前时间方法
    created_time: Mapped[datetime] = mapped_column(DateTime, server_default=func.now())

    # 定义字段 updated_time
    # onupdate 更新时；
    updated_time: Mapped[datetime] = mapped_column(DateTime, server_default=func.now(), onupdate=func.now())
    
    # 定义字段 is_active
    is_active: Mapped[bool] = mapped_column(Boolean, server_default=True)
    
    # 提示信息
    def __repr__(self):
        return f"<User(id={self.id}, username='{self.username}')>"

```

最后，SQLAlchemy 的 `Base` 类默认提供一个构造函数，它本身就支持‌**关键字参数初始化**‌。

你可以这样创建对象 `new_user = User(username="abc", password_hash="123456", is_active=True)`，同时创建对应的数据库数据。

以及，ORM 的核心思想是‌**对象状态追踪**‌。因此你只需要像操作普通 Python 对象一样修改属性，SQLAlchemy 就会自动检测变化并在 `commit` 时生成 `UPDATE` 语句，更新数据。

你可以这样修改数据 `user.username = "abcde"`。

当然，如果你想在模型中添加些辅助方法，比如密码加密，Api Key 自动生成，数据自动生成填入等，这些还是支持写入的。

你可以在 User 中添加一个密码加密的方法，在写入密码时调用这个方法。

```python
class User(Base):
	...
	
	def set_password(self, password):
		sha256_hash = hashlib.sha256()
		sha256_hash.update(password.encode('UTF-8'))
		self.password_hash = sha256_hash.hexdigest()
	...

...
user.set_password('123456')
...
```

# app/schemas/

`app/models/` 是**应用接口的请求/响应格式模型**，通过 pydantic 的使用，可以在这里定义接口的请求参数格式，响应参数格式。

当用户在这里写入参数格式后，用户便可以在 `app/api/` 定义接口时，直接调用这里的参数格式类，而不是使用 `Body, Query` 这些方法一个一个写入参数。将参数统一进行管理，更好的进行维护。

比如，原有接口是通过下述代码定义的：

```python
@router.post("/login", tags=["登录"])
async def login( 
    username: str = Body(..., description="账号"),        
    password: str = Body(..., description="密码"), 
    school: int | None = Body(default=None, description="学校"),
    db: AsyncSession = Depends(get_db),
) -> dict:

	auth_service = AuthService(db)
	user_obj = await auth_service.login(username, password)

	return { 
	    "code": 0,  
	    "message": "success",  
	    "data": {  
	        "id": user_obj.id,  
	        "name": user_obj.name,  
	        "token": user_obj.token,   
	    },
	}
```

现在，在 `app/schemas/auth.py` 中定义如下的一个请求参数模型和响应参数模型：

```python
from pydantic import BaseModel, Field
from typing import Generic, TypeVar

# 请求参数模型
class LoginRequest(BaseModel):
	# 与在接口中定义类似，只是将 Body 这些替换为 Field
	username: str = Field(..., description="账号") 
	password: str = Field(..., description="密码") 
	school: Optional[int] = Field(default=None, description="学校ID")

# 定义一个类型变量，代表任意 Pydantic 模型
PYDANTIC = TypeVar('PYDANTIC', bound=BaseModel)

# 基础响应模型
class Rseponse(BaseModel):
	code: int = 0
	message: str = "success"
	# data 数据可以使用其他参数模型，构造更复杂的响应，支持 None
	data: PYDANTIC | None = None

# 响应参数模型
class LoginResponse(BaseModel):  
    id: int
    name: str
    token: str
```

通过构造上述的参数模型，在 `app/api/` 接口定义时，便可以使用如下代码，调用参数模型：

```python
# 使用 response_model 指定 Pydantic 响应参数模型
@router.post("/login", response_model=Response[LoginResponse], tags=["登录"])
async def login( 
	# 使用 Pydantic 模型接收 Body 数据
    request: LoginRequest,
    db: AsyncSession = Depends(get_db),
) -> dict:

	auth_service = AuthService(db)
	# 从 request 中获取参数
	user_obj = await auth_service.login(request.username, request.password)

	return Response(
		code=0,
		message='success',
		data=LoginResponse(
			id=user_obj.id,
			name=user_obj.name,
			token=user_obj.token
		)
	)
```

# app/services/

`app/services/` 是**应用接口的功能实现**，接口要完成什么工作，实现什么功能，都应该在这里写入。

接口参数从 `app/api/` 中接收，然后在 `app/api` 中调用 `app/services/` 中对应功能的实现。

比如，一个简单的登录功能可以这样实现：

```python
class AuthService:

	# 初始化一个数据库会话，用来操作数据库
    def __init__(self, db: AsyncSession) -> None:  
        self.db = db  
  
    # 登录实现
    async def login(self, name: str, password: str) -> dict:  
	    # 使用 SQLAlchemy 编写 ORM 查询语句，查询数据
	    stmt = select(User).where(User.username == name)
	    result = await self.db.execute(stmt) 
	    user = result.scalar_one_or_none()
	    
	    if not user: 
		    raise ValueError("用户名或密码错误") 
		
		if password != user.password: 
			raise ValueError("用户名或密码错误") 
			 
		return { 
			"id": user.id,
			"name": user.username,
			"token": '------------------' 
		}
```

显然，在 `app/services/` 中，数据库操作是重要的组成部分，对应增删查改，对应的 ORM 语句如下所示：

> 下述中的 `db` 变量是一个 `AsyncSession`，参考 `app/api/deps.py` 中的 `get_db()` 方法实例化。

##  ORM 增

```python
# 实例化数据模型
new = User(username='username', password_hash='password_hash')
# 使用 try 语句保护数据执行
try:
	# 添加到会话，暂存修改
    db.add(new)
    # 提交事务 (实际执行 INSERT 语句)
    await db.commit()   
    # 更新会话，获取新插入的数据
    await db.refresh(new)
    # 返回插入的用户数据
    return new
except Exception as e:
    # 发生错误时回滚，防止脏数据
    await db.rollback()
    # 抛出错误
    raise e
```

## ORM 查

**基础的查询**

```python
# 构建 select 语句
stmt = select(User)
# 执行查询
result = await db.execute(stmt)
# 获取所有的对象，使用 scalars 方法获取单列数据
users = result.scalars().all() 
```

**条件查询**

```python
# 单个条件
stmt = select(User).where(User.username == "abc")
# AND 条件
stmt = select(User).where(User.is_active == True, User.age > 18)
stmt = select(User).where(
	and_(User.is_active == True, User.age > 18)
)
# OR 条件；like 模糊查询；% 通配符
stmt = select(User).where(
    or_(User.username == "abc", User.email.like("%@gmail.com"))
)

# 执行查询
result = await db.execute(stmt)
# first 只获取第一条匹配的数据
user = result.scalars().first()
```

**指定字段查询**

```python
# 只查询 id 和 username
stmt = select(User.id, User.username).where(User.is_active == True)
# 执行查询
result = await db.execute(stmt)
# 因为是多列，所以这里不能使用 scalars() 获取，只能 all() 返回全部
rows = result.all() 
# 通过遍历列数据进行访问
for row in rows:
    print(row.id, row.username)
```

**排序与分页**

```python
stmt = (select(User)
	.where(User.is_active == True)
    .order_by(User.created_time.desc())  # 降序，正序是 asc()
    .limit(10)                           # 限制
    .offset(0)                           # 偏移，从第几个数据开始
)

result = await db.execute(stmt)
users = result.scalars().all()
```

**关联查询 JOIN**

关联查询中有两个方法 `joinedload` 和 `selectinload`，分别适用于不同的连接关系。

`joinedload` 适用于一对一或多对一的关系，比如一个用户只有一个手机号码（一对一），多个地址对应一个用户（多对一）。

`joinedload` 只会执行一次查询，通过关键字 JOIN 一次性获取。

```python
# 查找所有用户的信息，包括另外一个表中对应的手机信息
# 这里的 phone_number 是表模型 Phone 在 User 的关系字段
stmt = select(User).options(joinedload(User.phone_number))
result = await db.execute(stmt)
# unique() 对数据合并去重（为避免误在一对多的情况下使用，添加）
#（可选，在一对一，多对一的情况下，不会重复）
users = result.scalars().unique().all()

# 查找所有地址的信息，包括地址对应的用户信息
stmt = select(Address).options(joinedload(Address.user))
result = await db.execute(stmt)
users = result.scalars().unique().all()
```

`selectinload` 适用于一对多或多对多的关系，比如一个用户对应多个地址（一对多）；一个用户对应多个分组，一个分组又可以对应多个用户（多对多）。

> 显然，一对多和多对一是相互的，用户和地址是一对多，那地址和用户便是多对一。

`selectinload` 会执行两次查询。与 `joinedload` 不同，`selectinload` 会先查询 `User` ，然后再通过关键字 IN 连接查询。虽然从代码层看起来一样，但 `selectinload` 和 `joinedload` 是执行逻辑是不同的。

```python
# 查找所有用户信息，包括对应的地址信息
stmt = select(User).options(selectinload(User.addresses))
result = await db.execute(stmt)
users = result.scalars().all()

# 查找所有用户信息，包括对应的分组信息
stmt = select(User).options(selectinload(User.group))
result = await db.execute(stmt)
users = result.scalars().all()
```

## ORM 改

```python
# 先查找对象再修改数据
stmt = select(User).where(User.id == 1)
result = await db.execute(stmt)
user = result.scalars().first()
# 执行修改
user.username = 'abcdef'
try:
	await db.commit()
	await db.refresh(user)
except Exception as e: 
	await db.rollback()

# 通过执行 UPDATE 修改数据
# 注意，这种情况下，无法对待修改的数据进行处理
stmt = update(User).where(User.id == 1).values(username='abcdef')
await db.execute(stmt)
await db.commit()
```

## ORM 删

```python
# 先查找对象再删除数据
stmt = select(User).where(User.id == 1)
result = await db.execute(stmt)
user = result.scalars().first()
# 执行删除
try:
	db.delete(user)
	await db.commit()
	await db.refresh(user)
except Exception as e: 
	await db.rollback()

# 通过执行 DELETE 删除数据
stmt = delete(User).where(User.id == 1)
await db.execute(stmt)
await db.commit()
```

# app/static/

应用的静态资源。

# app/utils/

应用的工具。

# .env

环境文件。应当遵循如下格式：

```ini
# 应用配置  
DEBUG=True  
  
# 数据库配置  
DATABASE_URL=sqlite+aiosqlite:///./data/database.db
```

# run.py

应用的启动脚本。

```python
import uvicorn  
  
if __name__ == "__main__":  
    uvicorn.run(
	    # ASGI 应用的路径，格式为模块路径:应用实例名
	    # 即 app/main.py 中名字为 app 的 FastAPI 实例
        "app.main:app",  
        host="0.0.0.0",
        port=8000,
        # 热重载，部署时务必关闭  
        reload=True,  
        # 工作进程数，热重载的时候会无效，部署时填写 CPU 核心数
        # workers=4,  
    )
```

当然，如果不通过这个脚本，也可以通过在终端执行命令运行应用。

```shell
uvicorn app.main:app --host 0.0.0.0 --port 8000 --workers 4
```

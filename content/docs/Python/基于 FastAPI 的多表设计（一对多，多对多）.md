---
title: 基于 FastAPI 的多表设计（一对多，多对多）
date: 2026-05-22T13:49:12+08:00
author: LiangMingJian
---

# 概述

在数据库表设计中，关系模型是一个很重要的设计部分。多表关系的构建可以让我们的数据库查询更加高效，表之间的结果更加完整，且可控。

在数据库表设计中，关系模型主要分为以下三种：**一对一，一对多，多对多**。

# 一对一

一对一指两个表中的一个记录仅与另一个表中的一个记录关联。比如，一个人只能有一个身份证号，而一个身份证号也只能属于一个人。

在 SQL 数据库设计时，可以通过在两个表中数据量较少的一方设置一个唯一的外键 FOREIGN KEY 来实现这一个关系。

```SQL
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
 
CREATE TABLE Ids (
    id INT PRIMARY KEY,
    card_id VARCHAR(20),
    user_id INT UNIQUE,
    FOREIGN KEY (user_id) REFERENCES Users(id)
);
```

在 FastAPI 的 Models 设计时，可以通过构建以下的代码，来实现这个关系。

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

class Users(Base):

    __tablename__ = 'Users'
    
    id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)
    name: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)

class Ids(Base):

	__tablename__ = "Ids" 
	
	id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True) 
	card_id: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)
	# 创建唯一（unique=True）外键（ForeignKey） user_id；nullable 非空外键，避免出现孤立数据
	user_id：Mapped[int] = mapped_column(ForeignKey("Users.id"), unique=True, nullable=False)

```

# 一对多

一对多指一个记录与另一个表中的多个记录关联。比如，一个人有多个地址。

在 SQL 数据库设计时，可以通过在【**多方**】设置一个外键 FOREIGN KEY 指向【**一方**】的主键来实现这一个关系。

```SQL
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
 
CREATE TABLE Addresses (
    id INT PRIMARY KEY,
    address_name VARCHAR(20),
    user_id INT,
    FOREIGN KEY (user_id) REFERENCES Users(id)
);
```

从上面不难看出，与一对一关系不同，外键 `user_id` 在一对多关系时，是不唯一的（没有 `UNIQUE`）。

因此，在 FastAPI 的 Models 设计时，也可以通过类似操作实现这个关系。

注意，这样操作只是在数据库层创建了外键，实现了一对多，但在 Python 的 ORM 操作中，你还不能通过在获取 `Addresses` 表数据后，通过点式 `.user.id` 来便捷的获取 `Users` 表的数据。

你还需要在两个模型中添加 `relationship`，让 ORM 操作时可以双向同步。即，可以使用 `Addresses.user.id`，通过 `Addresses` 表的数据来获取 `Users` 表的数据，也可以反过来使用 `Users.address.id` 通过 `Users` 表的数据，来获取 `Addresses` 表的数据。

```python
class Users(Base):

    __tablename__ = 'Users'
    
    id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)
    name: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)
    
    # 定义 Addresses 数据，back_populates 需要与 Addresses 中对应的数据一致
    address = Mapped[list["Addresses"]] = relationship("Addresses", back_populates="user")

class Addresses(Base):

	__tablename__ = "Addresses" 
	
	id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True) 
	card_id: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)
	
	# 创建唯一（unique=True）外键（ForeignKey） user_id；nullable 非空外键，避免出现孤立数据
	user_id：Mapped[int] = mapped_column(ForeignKey("Users.id"), unique=True, nullable=False)
	
	# 定义 User 数据，back_populates 需要与 Users 中对应的数据一致
	user: Mapped["Users"] = relationship("Users", back_populates="address")

```

注意！不要觉得写了 `relationship` 就可以不定义 `ForeignKey`。务必记得 `ForeignKey` 才是数据层的物理连接定义，`relationship` 只是 Python 代码的对象映射，只有 `relationship` 而没有 `ForeignKey`，SQLAlchemy 无法判断这两个表是怎么联系的，会抛出错误。

> 可能有时候你会听到多对一的关系，但其实多对一和一对多是相互的，用户和地址是一对多，那地址和用户便是多对一，这两者只是说法上的不同而已。

# 多对多

多对多指一个表中的数据既可以与另一个表中的多个数据关联，另一个表的数据也可以和这个表的多个数据关联。比如，一个学生可以选择多门课程，同时一门课程也可以被多个学生进行选择。

在 SQL 数据库设计时，往往需要通过新建第三张表来实现这个一关系模型，这个第三张表可以称为关联表。关联表中包含两个外键，这两个外键分别指向对应表中的主键。

```SQL
CREATE TABLE Users (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
 
CREATE TABLE Courses (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);

CREATE TABLE UsersRelateCourses (
    UserID INT,
    CourseID INT,
    PRIMARY KEY (UserID, CourseID),
    FOREIGN KEY (UserID) REFERENCES Users(id),
    FOREIGN KEY (CourseID) REFERENCES Courses(id)
);
```

与 SQL 设计方法类似，FastAPI 在设计多对多关系时同样需要新建第三张关联表，在这个表中加入外键，然后通过 `relationship` 方法中的 `secondary` 指定多对多关系。

```Python
class Users(Base):

    __tablename__ = 'Users'
    
    id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)
    name: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)
    
    # 通过 secondary 指定中间表，back_populates 对应连接属性
    course = Mapped[list["Courses"]] = relationship(secondary="UsersRelateCourses", back_populates="user")

class Courses(Base):

	__tablename__ = "Courses" 
	
    id: Mapped[int] = mapped_column(Integer, primary_key=True, autoincrement=True)
    name: Mapped[str] = mapped_column(String(50), unique=True, nullable=False, index=True)
    
    # 通过 secondary 指定中间表，back_populates 对应连接属性
    user = Mapped[list["Users"]] = relationship(secondary="UsersRelateCourses", back_populates="course")
    
class UsersRelateCourses(Base):

	__tablename__ = "UsersRelateCourses"
	
	# 定义关联表，连接属性
	user_id: Mapped[int] = mapped_column(ForeignKey('Users.id'), primary_key=True) 
	course_id: Mapped[int] = mapped_column(ForeignKey('Courses.id'), primary_key=True)
```

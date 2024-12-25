---
title: Python 的格式化输出
date: 2024-12-25T16:11:09+08:00
author: LiangMingJian
---

# 标准输出

```python
print("ASDDF")
```

# 使用占位符输出

```python
name = input("name：")
age = int(input("age："))
Job = input("Job:")
salary = float(input("salary:"))
info = """
    -----------info of %s-----------
    name:%s
    age:%d
    Job:%s
    salary:%f
    -------------------------------
""" %(name, name, age, Job, salary)
print(info)
# 其中 %s 是占位符
```

# 使用 format 输出

```python
name = input("name：")
age = int(input("age："))
job = input("Job:")
salary = float(input("salary:"))
info2 = '''
    =================info of {_name}====================
    name:{_name}
    age:{_age}
    job:{_job}
    salary:{_salary}
    ===================================================
'''.format(_name=name, _age=age, _job=job, _salary=salary)
info3 = f'''
    =================info of {name}====================
    name:{name}
    age:{age}
    job:{job}
    salary:{salary}
    ===================================================
'''
print(info2)
# 格式：.format(_name=name,_age=age,_job=job,_salary=salary)
# 也可以在字符串前加上 f 快捷的使用
```


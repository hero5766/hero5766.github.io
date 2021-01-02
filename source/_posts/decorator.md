---
title: 装饰器
author: hero576
tags:
  - python
categories:
  - programme
date: 2021-1-2 15:58:23
---

> 

<!--more-->

# 简介
- 装饰器是python的一种语法糖，可以简洁的在函数加入操作。

# 函数中返回函数[^1]
[^1]: [链接](https://www.runoob.com/w3cnote/python-func-decorators.html)

```py
def decorator(a_func):
    def wrapTheFunction():
        print("I am doing some boring work before executing a_func()")
        a_func()
        print("I am doing some boring work after executing a_func()")
    return wrapTheFunction
def function():
    print("I am the function which needs some decoration to remove my foul smell")
function = decorator(function)
function()
```

- @符号是一个简短的方式来生成一个被装饰的函数

```py
def decorator(a_func):
    def wrapTheFunction():
        print("I am doing some boring work before executing a_func()")
        a_func()
        print("I am doing some boring work after executing a_func()")
    return wrapTheFunction
@decorator
def function():
    print("I am the function which needs some decoration to remove my foul smell")
function()
```

- 我们运行如下代码会存在一个问题：
```py
print(function.__name__)
# Output: wrapTheFunction
```

- 这并不是我们想要的！Ouput输出应该是"function"。这里的函数被warpTheFunction替代了。它重写了我们函数的名字和注释文档(docstring)。幸运的是Python提供给我们一个简单的函数来解决这个问题，那就是functools.wraps。我们修改上一个例子来使用functools.wraps：

```py
from functools import wraps
def decorator(a_func):
    @wraps(a_func)
    def wrapTheFunction():
        print("I am doing some boring work before executing a_func()")
        a_func()
        print("I am doing some boring work after executing a_func()")
    return wrapTheFunction
@decorator
def function():
    """Hey yo! Decorate me!"""
    print("I am the function which needs some decoration to "
          "remove my foul smell")
print(function.__name__)
# Output: function
```

# 带参数的装饰器
- 在函数中嵌入装饰器：以日志为例，创建一个包裹函数，能让我们指定一个用于输出的日志文件。
```py
from functools import wraps
def logit(logfile='out.log'):
    def logging_decorator(func):
        @wraps(func)
        def wrapped_function(*args, **kwargs):
            log_string = func.__name__ + " was called"
            print(log_string)
            # 打开logfile，并写入内容
            with open(logfile, 'a') as opened_file:
                # 现在将日志打到指定的logfile
                opened_file.write(log_string + '\n')
            return func(*args, **kwargs)
        return wrapped_function
    return logging_decorator
@logit()
def myfunc1():
    pass
myfunc1()
# Output: myfunc1 was called
# 现在一个叫做 out.log 的文件出现了，里面的内容就是上面的字符串
@logit(logfile='func2.log')
def myfunc2():
    pass
myfunc2()
# Output: myfunc2 was called
# 现在一个叫做 func2.log 的文件出现了，里面的内容就是上面的字符串
```

# 装饰器类
- 现在我们有了能用于正式环境的logit装饰器，但当我们的应用的某些部分还比较脆弱时，异常也许是需要更紧急关注的事情。比方说有时你只想打日志到一个文件。而有时你想把引起你注意的问题发送到一个email，同时也保留日志，留个记录。这是一个使用继承的场景，但目前为止我们只看到过用来构建装饰器的函数。
```py
from functools import wraps
class logit(object):
    def __init__(self, logfile='out.log'):
        self.logfile = logfile
    def __call__(self, func):
        @wraps(func)
        def wrapped_function(*args, **kwargs):
            log_string = func.__name__ + " was called"
            print(log_string)
            # 打开logfile并写入
            with open(self.logfile, 'a') as opened_file:
                # 现在将日志打到指定的文件
                opened_file.write(log_string + '\n')
            # 现在，发送一个通知
            self.notify()
            return func(*args, **kwargs)
        return wrapped_function
    def notify(self):
        # logit只打日志，不做别的
        pass
```

- 这个实现有一个附加优势，在于比嵌套函数的方式更加整洁，而且包裹一个函数还是使用跟以前一样的语法：

```py
@logit()
def myfunc1():
    pass
```

# 例子
## 实现重试策略[^2]
[^2]: [链接](https://blog.csdn.net/weixin_42731853/article/details/111351730)



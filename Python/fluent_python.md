# Fluent Python

[TOC]

## 序列



## 装饰器

### functools.lru_cache

* <u>空间换时间</u>
* functors.lru_cache常用于 <u>递归</u> 算法
* `lru`是”Least Recent Used” 的缩写
- - - -
* e.g.
```python
import functools

@functools.lrc_cache()
def fibonacci(n):
	if n < 2:
		return n
	return fibonacci(n-2) + fibonacci(n-1)
```



### singledispatch：单分配泛函数

> dispatch：派遣  
- - - -
* 类似于重载
```python
from functools import singledispatch
from collections import abc # 使用抽象类

@singledispatch
def f(n):
	print("default")

@f.register(str)
def _(string):
	print("String")

@f.register(abc.Integral)
def _(integral):
	print("integral")
```

> 注：   
> 1. `abc.Integral`是int的抽象超类
> 2. 应该优先使用`abc`


# functools.lru_cache
- - - -
* ~空间换时间~
* `functors.lru_cache`常用于 ~递归~ 算法
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



#python/fluent_python
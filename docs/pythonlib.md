Python 第三方库
===

Python的第三方库使用Demo

时间
-----

### timeit

检测测试一段代码运行多次所使用的时间。可以用来替代使用time计算程序运行时间

```python
from timeit import timeit

# 看执行1000000次x=1的时间
timeit('x=1')

# 看x=1的执行时间，执行1次(number可以省略，默认值为1000000)
timeit('x=1', number=1)

# 看一个列表生成器的执行时间,执行1次
timeit('[i for i in range(10000)]', number=1)

# 看一个列表生成器的执行时间,执行10000次
timeit('[i for i in range(100) if i%2==0]', number=10000)
```

9.11日志
====
匿名函数(很好用)
----
```
可以把匿名函数赋值给一个变量，再利用变量来调用该函数：

>>> f = lambda x: x 
```
装饰器，在代码运行期间动态增加功能的方式
----
首先 简单定义一个能打印日志的decorator<br>
```
def log(func):
    def wrapper(*args, **kw):
        print('call %s():' % func.__name__)
        return func(*args, **kw)
    return wrapper
```
**借助python的@语法**
```
@log
def now():
    print('2015-3-25')
```
以上@其实等价于
```
now = log(now)
```

假设现在要自定义log，则decorator需要本身传入参数<br>
写在这里[question](https://github.com/justabugg/target/blob/master/python/python.md#%E7%AC%AC%E5%8D%81%E4%B8%83%E4%B8%AA-%E8%A3%85%E9%A5%B0%E5%99%A8decoorator)
要注意 最后经过装饰，new的签名变成了wrapper，所以要在wrapper后加上 **@functools.wraps(func)**<br>


利用functools.partial制造偏函数
----
在我理解偏函数就是正统函数经过一定改造，functools.partial其实就是辅助改造<br>
```
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
>>> int2('1010101')
85

```
而且固定的为{'base':2}
不知道为什么现在心里很急躁我很难受头很疼 好多东西写了一半 今天就到此为止吧 
![fuck](https://github.com/justabugg/test/blob/master/111.jpg)
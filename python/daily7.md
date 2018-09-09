9.9学习日志
====
生成器 generator
----
首先要知道产生的原因，因为list占用空间，不可能无限大，所以用生成器并不直接推算出所有内容<br>
创建方法就是把列表生成式的[]改成()<br>
```
>>> L = [x * x for x in range(10)]
>>> L
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
>>> g = (x * x for x in range(10))
>>> g
<generator object <genexpr> at 0x1022ef630>
```
理论上要打印generator成员要用next()<br>
当很多时可用for n in L输出<br>
改造fib可以推出第二种generator方式**(有yield的generator function)**<br>
(fib思路写在target)<br>
**重点是generator的执行流程，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。**<br>
```
>>> o = odd()
>>> next(o)
step 1
1
>>> next(o)
step 2
3
>>> next(o)
step 3
5
>>> next(o)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
这里还有一点 就是怎么调用generator的，先生成了o=odd（）再调用它<br>

迭代器
----
这一节内容很可能弄混 注意iterable iterator generator<br>
**iterable可迭代对象**是可以作用于for循环的对象<br>
```
>>> from collections import Iterable
>>> isinstance([], Iterable)
True
>>> isinstance({}, Iterable)
True
>>> isinstance('abc', Iterable)
True
>>> isinstance((x for x in range(10)), Iterable)
True
>>> isinstance(100, Iterable)
False

```
如list，dict，generator<br>
**生成器iterator**不但可以被for循环，还可以被next()调用并返回下一个值的对象<br>
```
>>> from collections import Iterator
>>> isinstance((x for x in range(10)), Iterator)
True
>>> isinstance([], Iterator)
False
>>> isinstance({}, Iterator)
False
>>> isinstance('abc', Iterator)
False

```
这里理一下<br>
**之所以很多可迭代对象例如list，dict，str可以被for调用，却不可以被next()调用并且返回，因为生成器对象是一个数据流，并不能提前知道序列的长度，他的计算是惰性的，需要时才会计算，iterator甚至可以表示无限的数据流，这样的特性是前者不可能拥有的**<br>
不过这些iterable可以通过一个iter（）函数变成iterator<br>
```
>>> isinstance(iter([]), Iterator)
True
>>> isinstance(iter('abc'), Iterator)
True

```
小结
===
基本完全理解 秒杀第一次看的我只会死记ITERABLE,ITERATOR<br>
![cool](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1536484829430&di=4ace37b3793a84a7f0d6656159f574b6&imgtype=0&src=http%3A%2F%2Fwx1.sinaimg.cn%2Fthumb150%2F006HodI6ly1fejwu9an5qj306404tgm2.jpg)

函数式编程，高阶函数
-----
意思是可以把函数作为参数传入另一个函数<br>
**python内建的map()函数，传入一个函数一个iterable，输出一个iterator**<br>
例<br>
```
>>> def f(x):
...     return x * x
...
>>> r = map(f, [1, 2, 3, 4, 5, 6, 7, 8, 9])
>>> list(r)
[1, 4, 9, 16, 25, 36, 49, 64, 81]
```
其中注意 输出的iterator是惰性的，所以用一个list把他计算出来了，我用next试了一下也ok<br>

**内建的reduce函数，接受两个参数再把结果继续和序列中的下个元素做累计计算**
```
reduce(f, [x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```
深入的理解写在target里了[target](https://github.com/justabugg/target/blob/master/python/python.md)<br>
今天有点累了 等会去跑个步 回来的时候要洗洗衣服啥的 如果有空就把target写了 明天再战么么哒<br>
![image](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1536501551550&di=e2559c90a1833099ae73f1e003c8975c&imgtype=0&src=http%3A%2F%2Fimg2.tuicool.com%2F7RnUjez.jpg%2521web)

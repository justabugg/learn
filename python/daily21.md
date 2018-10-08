10.8 现在就是后悔 非常后悔
===========
经历了挂掉科目二和一个国庆后 我又回来继续学习啦<br>
大概半个月的时间没摸吧 来复习一周 把python1,2的问题解决<br>

基础部分
----

#### ord()获取字符的整数表示，chr()把编码转换为对应的字符<br>
字符和数字之间转换 以后应该会有一些巧用<br>

#### bytes
在内存中unicode储存，字符可能对应若干字节，如果传输或者保存到磁盘就要把他变成以字节为单位的bytes<br>
		x=b'ABC'

#### hash算法
[大神总结](https://blog.csdn.net/asdzheng/article/details/70226007)

#### 函数参数
一个组合看懂就行
```python
def f1(a, b, c=0, *args, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'args =', args, 'kw =', kw)

def f2(a, b, c=0, *, d, **kw):
    print('a =', a, 'b =', b, 'c =', c, 'd =', d, 'kw =', kw)

>>> f1(1, 2)
a = 1 b = 2 c = 0 args = () kw = {}
>>> f1(1, 2, c=3)
a = 1 b = 2 c = 3 args = () kw = {}
>>> f1(1, 2, 3, 'a', 'b')
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {}
>>> f1(1, 2, 3, 'a', 'b', x=99)
a = 1 b = 2 c = 3 args = ('a', 'b') kw = {'x': 99}
>>> f2(1, 2, d=99, ext=None)
a = 1 b = 2 c = 0 d = 99 kw = {'ext': None}
```

高级特性
----

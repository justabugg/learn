2018.9.8
=======
今天看一下高级特性，其实就是一些增加便捷性的操作吧  
切片
----
range和切片都是range（5）是0~4 L[0:5]是L[0]~L[4]  
**其中L[:10:2]代表前10个 每俩取一个 L[::2]代表所有的 每俩取一个 [:]复制一个list**  
迭代部分
-----
 python的迭代使用for xx in yyy  
抽象程度很高因为不仅仅可以用在list或者tup，比如dict  
```d={'a':1,'b':2,"c":3}  
  for key in d:  
    print(key)  
a  
c  
b
```  
因为dict不是顺序的 字符之类的也可以迭代 用isinstace（'abc',Iterable)判断  
```for x,y in [(1,1),(2,4),(3,5)]:  
···    print(x,y)  
1 1  
2 4  
3 9 
 ```
列表生成式
-----
```
 [x * x for x in range(1, 11)]  

 [x * x for x in range(1, 11) if x % 2 == 0]  

 [m + n for m in 'ABC' for n in 'XYZ']

d = {'x': 'A', 'y': 'B', 'z': 'C' }
>>> for k, v in d.items():
...     print(k, '=', v)
...
y = B
x = A
z = C

最后把一个list中所有的字符串变成小写：

>>> L = ['Hello', 'World', 'IBM', 'Apple']
>>> [s.lower() for s in L]
['hello', 'world', 'ibm', 'apple']
```
今天满课 晚上没有跑步 没有怎么学习
![images](https://wx1.sinaimg.cn/mw1024/923642a7gy1ftgekenti7j20ku0gyn66.jpg)
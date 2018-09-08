2018.9.8
=======
今天看一下高级特性，其实就是一些增加便捷性的操作吧  
range和切片都是range（5）是0~4 L[0:5]是L[0]~L[4]  
**其中L[:10:2]代表前10个 每俩取一个 L[::2]代表所有的 每俩取一个 [:]复制一个list**  
迭代部分 python的迭代使用for xx in yyy  
抽象程度很高因为不仅仅可以用在list或者tup，比如dict  
**d={'a':1,'b':2,"c":3}  
  for key in d:  
    print(key)  
a  
c  
b**  
因为dict不是顺序的 字符之类的也可以迭代 用isinstace（'abc',Iterable)判断  
```for x,y in [(1,1),(2,4),(3,5)]:  
···    print(x,y)  
1 1  
2 4  
3 9  ```···

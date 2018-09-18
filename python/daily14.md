9.16 认真学习
========
使用元类
----------
***动态语言和静态语言最大的不同，就是函数和类的定义，不是编译时定义的，而是运行时动态创建的。***
### type()
type()创建的class和直接写是一样的，python解释器遇到class定义也是扫描一下语法，用type()写出<br>
type()可以查看一个类型或者变量的类型，也可以创建出一个新的类型：
```
>>> def fn(self, name='world'): # 先定义函数
...     print('Hello, %s.' % name)
...
>>> Hello = type('Hello', (object,), dict(hello=fn)) # 创建Hello class
>>> h = Hello()
>>> h.hello()
Hello, world.
>>> print(type(Hello))
<class 'type'>
>>> print(type(h))
<class '__main__.Hello'>
```
type()依次传入class名称，继承的父类集合，(如果只有一个，别忘了元组的单独写法),class的方法名称与函数绑定

### metaclass() 元类

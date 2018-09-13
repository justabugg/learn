9.13学习日志
=====
今天更的有点迟，甚至差点断更

获取对象信息
-------
**type返回的是class信息<br>**
```
>>> type(123)
<class 'int'>
>>> type(abs)
<class 'builtin_function_or_method'>
>>> type(a)
<class '__main__.Animal'>
```
但是如果想判断一个具体的类型，要调用一下常量<br>
```
>>> import types
>>> def fn():
...     pass
...
>>> type(fn)==types.FunctionType
True
>>> type(abs)==types.BuiltinFunctionType
True
>>> type(lambda x: x)==types.LambdaType
True
>>> type((x for x in range(10)))==types.GeneratorType
True
```
**isinstance可以告诉我们一个对象是否是某种类型**
```
>>> isinstance('a', str)
True
>>> isinstance([1, 2, 3], (list, tuple))                   #还可以看是否为多者中的一种
True
>>> isinstance((1, 2, 3), (list, tuple))
True
```
**dir()可以返回一个list，包含对象的所有属性和方法**  
```
>>> dir('ABC')
['__add__', '__class__',..., '__subclasshook__', 'capitalize', 'casefold',..., 'zfill']
```

实例属性和类属性
-------
在class里定义的话属于类属性，这样如果实例属性默认没没有时，会查找到class属性<br>
```
lass Student(object):
    def __init__(self, name):
        self.name = name

        >>> class Student(object):
...     name = 'Student'
...
>>> s = Student() # 创建实例s
>>> print(s.name) # 打印name属性，因为实例并没有name属性，所以会继续查找class的name属性
Student
>>> print(Student.name) # 打印类的name属性
Student
>>> s.name = 'Michael' # 给实例绑定name属性
>>> print(s.name) # 由于实例属性优先级比类属性高，因此，它会屏蔽掉类的name属性
Michael
>>> print(Student.name) # 但是类属性并未消失，用Student.name仍然可以访问
Student
>>> del s.name # 如果删除实例的name属性
>>> print(s.name) # 再次调用s.name，由于实例的name属性没有找到，类的name属性就显示出来了
Student
```

9.14认真学习 
=====
面对对象高级编程
======

使用__slots__
----
### 定义class后绑定实例的属性和方法<br>
```
class Student(objece):
  pass

s=Student()
s.name='zhang'                                                   #给实例绑定一个属性

def set_age(self,age):
  self.age=age

from types import MethodType
s.set_age=MethodType(set_age,s)                                  #绑定一个方法
s.set_age(20)
s.age
20

s2=Student()
s2.set_age(25)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'Student' object has no attribute 'set_age'      #但是不能给另一个实例绑定，如果想绑定

Student.set_score=set_score                                      #给类绑定方法就可以了
```
### 现在想限制实例的属性，则用__slots__
```
class Student(object):
    __slots__ = ('name', 'age') # 用tuple定义允许绑定的属性名称
```
***但是注意，子类的属性仍然不受限制，除非在子类里也定义__slots__***

使用@property
---------
平时要想定义一个封装起来的类要这样
```
class Studetn(object):
  def get_score(self):
    return self.score
  def set_score(self,value)
    if not isinstacne(value,int)
      raise ValuerError('score must be int')
    if value>100 or value<0:
      raise ValuerError('score must between 0~100')
    self.score=value

>>>s=Studetn()
>>>s.set_score(90)
>>>s.get_scores()
90
```
这样每次调用都要s.get__balbalba的很麻烦，如果可以像属性一下调用就要用@property<br>
我觉得@property就是生成一个装饰器这样重构了类里面的def<br>
```
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value

>>>s.score=99
>>>s.score
99
```
**如果想定义只读，那么之就不定义setter**
```
 @property
    def age(self):
        return 2015 - self._birth
```


多重继承
----
主要mixin

定制类
----
就像_init__这个格式的变量有特殊任务<br>
[偷个懒](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/0014319098638265527beb24f7840aa97de564ccc7f20f6000)偷个懒 来看就行了


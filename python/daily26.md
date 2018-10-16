复习第六天啦
=====
把target里疑惑看了不少就很舒服 很多方面可以深入的但是我还是想等实际应用的时候遇到再理解，死记不是方法

#### __slots__
遇到了一个好玩的给实例绑定方法的用法
```py
from type import MethodType
def add():
  pass
s.set_age=MethodType(set_age,s)
s.set_age(25)
s.age
25
```
***很奇怪，子类并不直接继承父类的slots，除非在子类也定义一个，这样就允许绑定子类＋父类的slots***

#### @property
这就是一种装饰器，可以把一个方法变成属性调用的
```py
class Student(object):

    @property
    def score(self):
        return self._score
#可以设置限定条件
    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value
```
以上，直接通过s.score=100就可以直接调用setter，就像属性调用一样，如果不设置setter，就是只读的。


#### 多重继承
参考[targe](https://github.com/justabugg/target/blob/master/python/python2.md#%E7%AC%AC%E4%B8%80%E4%B8%AA--mixin)



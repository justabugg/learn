9.15 请假
========
调整状态两天，今天下午也很忙，趁现在多学学我真的不想再水了<br>

使用枚举类
---
一开始是为了定制一个常量,直接赋值的话不仅是变量，而且还是int类型<br>
所以用enum类来实现
```
from enum import Enum

Month = Enum('Month', ('Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'))
```
**注意这里的value是默认给成员的int常量，默认从1开始**
```
>>> for name, member in Month.__members__.items():                #注意这里
...     print(name, '=>', member, ',', member.value)    
...
Jan => Month.Jan , 1
Feb => Month.Feb , 2
Mar => Month.Mar , 3
```
最灵活的是可以定义从Enum中派生的类:<br>
```
from enum import Enum,unique

@unique                                                           #unique可以检查没有重复值
class myself(Enum):
  sun=0
  mon=1
  tue=2
```
还是有点迷本节最后的习题，为什么叫避免使用字符串

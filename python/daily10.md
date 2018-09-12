9.12学习日志
====
这两天的睡眠简直是一种折磨

模块
---

### 模块
```
mycompany
├─ __init__.py
├─ abc.py
└─ xyz.py
```
其中init本身就是一个模块，模块名为mycompany<br>
例子：hello的模块<br>
```
#!/usr/bin/env python3                      #这是可以直接再mac/linux/unix运行
# -*- coding: utf-8 -*-                     #这是用utf-8编制

' a test module '                           #这是文档注释，任何模块代码的第一个字符串都是
 
__author__ = 'alex z'                       #作者名称

import sys                        

def test():
    args = sys.argv                         #sys模块有一个argv变量，用list存了命令行所有参数，至少有一个元素，第一个参数是.py的名称
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':          
    test()
```

### 作用域
------
在编写代码时很重要，有的时候作用域不对甚至会导致严重的算法错误，所以要明确，我现在知道的局部变量，global，nonlocal<br>
现在__x__一般是特殊变量，\_x 则是隐私变量<br>
```
def _private_1(name):
    return 'Hello, %s' % name

def _private_2(name):
    return 'Hi, %s' % name

def greeting(name):
    if len(name) > 3:
        return _private_1(name)
    else:
        return _private_2(name)
```
这样也算一种用法吧，等于把内部调用函数隐藏起来好看些，我记得private后面还要讲，先了解一下了<br>

2.1面向对象编程
------
**对象包含了数据和操作数据的函数**

### 类和实例
```
class Student(object):                   #定义类
  pass

brat=Student()                           #创建实例

bart.name='zhang'                        #给实例绑定属性
bart.name
'zhang'
```
也可以给在创建类的时候强制绑定属性<br>
```
class Student(object):

    def __init__(self, name, score):     #注意，第一个参数永远是self，不用输入，但是后来的都是绑定在self上
        self.name = name
        self.score = score

yuan=Student('yuan',99)                  #必须传入相应参数
``` 

### 数据封装
一开始是定义了一个新的函数去print实例里的name<br>
现在没有必要在外部访问，直接在student内部定义一个<br>
```
class Student(object):

    def __init__(self, name, score):
        self.name = name
        self.score = score

    def print_score(self):
        print('%s: %s' % (self.name, self.score))

bart.print_score()
bart simpson:59
```
这样的student只需要定义时传入参数就可以 不用管他的print是怎么实现 ，这样就封装起来了<br>
同理可以添加其他的函数来实现其他的功能哦<br>

## 访问限制
就是这里 详细一些的写了private的用法<br>
意思就是内部的self.name改为了self.\__name，这样外部就无法直接brad.name的访问了<br>
如果想访问的话则需要定义一个<br>
```
def print_score(self):
  print ('%s:%s'%(self.__name,self.__score))
```
同理 如果想修改就要加一个set函数<br>
```
def set_score（self,score):
  self.__score=score
```
这样的好处是可以增加限制条件<br>
```
def set_score(self, score):
        if 0 <= score <= 100:
            self.__score = score
        else:
            raise ValueError('bad score')
```

### 继承和多态
前部分讲的都是子类可以继承父类的全部功能<br>
```
class Animals(object):
  def run(self):
    return animals is running

class Dog (Animals):
  pass

dog=Dog()
dong.run()
animals is running
```
在子类时可以改进一些功能，在存在同样的时候。子类覆盖了父类的功能。<br>
重点是定义一个函数：
```
def rundouble(Animals):
    animals.run()
    animals.run()

rundouoble(Dog())
animals is running
animals is running

class new (Animals):                  #新增一个animals的子类
  def run(self):
    new is running
    new is running

rundouble(new()                       #照样可以运行
new is running 
new is running
```
这就体现了多态，***开闭原则：***<br>
对扩展开放：允许新增Animal子类；

对修改封闭：不需要修改依赖Animal类型的run_twice()等函数。

### 静态语言 vs 动态语言
对于静态语言（例如Java）来说，如果需要传入Animal类型，则传入的对象必须是Animal类型或者它的子类，否则，将无法调用run()方法。<br>
对于Python这样的动态语言来说，则不一定需要传入Animal类型。我们只需要保证传入的对象有一个run()方法就可以了：<br>
```
class Timer(object):
    def run(self):
        print('Start...')
```
这就是动态语言的“鸭子类型”，它并不要求严格的继承体系，一个对象只要“看起来像鸭子，走起路来像鸭子”，那它就可以被看做是鸭子。
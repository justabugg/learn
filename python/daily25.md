昨天忙了一天，做了网站，但是有在脑子里想装饰器
=====

#### 装饰器的深入运用在python3

#### 偏函数
利用functool的功能改造函数

#### 模块
顶层包名不重复底文件就不会重复，注意__init__文件

#### 使用模块
要知道模块怎么写
```py
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

' a test module ' #任何模块代码的第一个字符串都被视为模块的文档注释

__author__ = 'Michael Liao'  #使用__author__变量把作者写进去

import sys

def test():
 #sys模块有一个argv变量，用list存储了命令行的所有参数。argv至少有一个元素，因为第一个参数永远是该.py文件的名称
    args = sys.argv
    if len(args)==1:
        print('Hello, world!')
    elif len(args)==2:
        print('Hello, %s!' % args[1])
    else:
        print('Too many arguments!')

if __name__=='__main__':
    test()
```

#### 面对对象编程  数据封装，继承和多态
之前并没有具体理解oop，其中的对象包括了数据和操作数据的函数(class中的属性和method)<br>
面对过程把计算机程序是为命令的集合，函数的顺序执行，通过切分为子函数降低复杂度<br>
面对对象则把程序视为对象的集合，对象间可以接受消息并处理，执行就是一系列消息在对象中传递<br>
<br>
* 注意怎么把一个属性对外隐藏并封装一个get一个set方法<br>
* 开闭原则
* 动态语言的鸭子类型

解决一下python1里的东西，如果晚上有空继续看高级的面oop
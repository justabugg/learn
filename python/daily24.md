10.11
=====
天气好棒洗了被单洗了冬天衣服晒了被子，可惜昨晚把鞋子跑坏了今晚不能跑步啦，明晚再5KM

#### 先把昨天的问题解决，一个是f1,f2,3=count()的赋值原理<br>
[具体看这里](https://blog.csdn.net/gavin_john/article/details/49906095)  
```py
a=1
b=2
a,b=b,a
#这是一个可以互换值的巧妙办法
```
<br>
注意拓展序列解包，用起来很方便:
```py
>>>sec=[1,2,3,4]
>>>a,*b=sec
>>>a
1
>>>b
[2,3,4]
```
一个是yield和return之类的执行顺序:其实也不难，主要在交互式环境测一下就差不多了<br>
[埃氏筛法](https://github.com/justabugg/target/blob/master/python/python3.md#2.generator的yield中执行顺序cat)

#### 返回函数
闭包中引用循环变量的方法中，注意逻辑：先定义一个函数给入参数x，在主定义最后用for传入x，再用[f(x)]实现调用而将x传入最里层的函数return中<br>
涉及到挺多的知识点的，在python1和3里了


#### 装饰器
本质上来说是一个高级返回函数，使用的时候记得@decorator实际执行起来是啥样的，再一个就是记得***改签名***<br>
做一下作业:请编写一个decorator，能在函数调用的前后打印出'begin call'和'end call'的日志。
```py
def decorator(fun):
	def wrapper(*args,**kw):
		print('begin call')
            def new(*args,**kw):
            	
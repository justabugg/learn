10.25 
====
复习错误与调试
---------
* 使用try···except···finally
```py
try:
    print('try...')
    r = 10 / int('2')
    print('result:', r)
except ValueError as e:
    print('ValueError:', e)
except ZeroDivisionError as e:
    print('ZeroDivisionError:', e)
else:
    print('no error!')
finally:
    print('finally...')
print('END')
```
由以上可以看到，用不同的except捕获不同的错误，错误其实是一个class继承于BseException，所以注意这个
```py
try:
    foo()
except ValueError as e:
    print('ValueError')
except UnicodeError as e:  #UnicodeError是ValueError的子类 所以永远捕获不了错误
    print('UnicodeError')
    ```
可以跨越层次(嵌套在函数内的一个错误，可以在最外层捕获到)所以省略了很多代码减少麻烦

* logging记录错误
数据结构中算法设计的健壮性，在错误的时候不应该终止程序运行，而是继续运行并抛出错误，以便更高层次的解决
```py
import logging 
def main():
	try:
		function('0')
	except Exception as e:
		logging.exception(e)
main()
print('END')  #End将会被打印 虽然错误还是运行完
```

* 抛出错误
自己编写函数来抛出错误
```py
#首先定义自己的错误，注意父类错误
class mineError(ValueError):
	pass
def foo(s):
	n=int(s)
#判断错误条件并raise
	if n==0:
		raise mineError('invalid value:%s'%s)
	return 10/n
foo('0')
```
综合一下 一种常用的错误处理方式，捕获错误仅仅用来记录，留下错误栈继续上抛
```py
def myerror(s):
	n=int(s)
	if n==0:
		raise ValueError('wrong!:%s'%s)
	return 10/n
def bar():
	try:
		myerror('0')
	#首先捕获到错误
	except ValueError as e:
		print('wrong!!')
	#再次raise原错误栈
		raise                 
bar()
```
raise不加参数就会抛出原错误，也可以在except中raise来转换错误类型
```py
try:
	10/0
except ZeroDivisionError:
	raise ValueError('inpust error')
```

* 使用断言assert
首先是print，太麻烦所以变成了断言assert
```py
def wrong(s):
	n=int(s)
	assert n!=0,'n is 0000'
	return 10/n
foo('0')

python -o err.py#关闭断言 asser=pass
```

* 使用logging(终极武器)
```py
import logging 
#以下用来配置等级 debug,info,warning,error
logging.basicConfig(level=logging.INFO)
s='0'
n=int(s)
logging.info('n=%d'%n)
print 10/n
#将会输出一个文件为错误log
```

* pdb以及pdb.set_tarce()
启动python调试器pdb
```py
python -m pdb err.py
#l来查看代码
#n可以单步执行代码
#p ＋变量名查看变量
#q结束
```
手动敲太麻烦了所以设置一个pdb.set_trace()在可能出错的关键位置
```py
import pdb
s='0'
n=int(s)
pdb.set_trace
print(10/n)
```

补单元测试和文档测试
------
* 单元测试
```py
#引入测试模块
import unittest
#引入自己的Dict
from mydict import Dict

class TestDict(unittest.TestCase):  
#setUP()在测试前执行
    def setUp(self):
    	print('start first')

#测试属性调用和定义功能
    def test_init(self):
        d = Dict(a=1, b='test')
        self.assertEqual(d.a, 1)
        self.assertEqual(d.b, 'test')
        self.assertTrue(isinstance(d, dict))
#测试定义功能
    def test_key(self):
        d = Dict()
        d['key'] = 'value'
        self.assertEqual(d.key, 'value')
#测试属性定义
    def test_attr(self):
        d = Dict()
        d.key = 'value'
        self.assertTrue('key' in d)
        self.assertEqual(d['key'], 'value')
#测试报错/健壮性
    def test_keyerror(self):
        d = Dict()
        with self.assertRaises(KeyError):
            value = d['empty']

    def test_attrerror(self):
        d = Dict()
        with self.assertRaises(AttributeError):
            value = d.empty
#在测试最后执行
    def tearDown(self):
    	print('now i am end ')
```
运行时只会运行test开头的测试方法，unittest,TestCase其中包括两种重要的断言
```py
#期望相等/布尔值为1
self.assertEqual(abs(-1),1)
#期望抛出错误
with self.assertRaises(KeyError):
	value=d.empty
```
**注意setUp和tearDown**

运行方法一：
```py
#末尾加上
if __name__='__main__':
	unittest.main()

#当作正常脚本运行
python mydict_test.py
```
运行方法二：
```py
python -m unittest mydict_test
.....
----------------------------------------------------------------------
Ran 5 tests in 0.000s

OK
```

* 文档测试doctest
在注释中编写，会严格按照交互式命令来判断结果，错误才返回
```py
# mydict2.py
class Dict(dict):
    '''
    Simple dict but also support access as x.y style.

    >>> d1 = Dict()
    >>> d1['x'] = 100
    >>> d1.x
    100
    >>> d1.y = 200
    >>> d1['y']
    200
    >>> d2 = Dict(a=1, b=2, c='3')
    >>> d2.c
    '3'
    >>> d2['empty']
    Traceback (most recent call last):
        ...
    KeyError: 'empty'
    >>> d2.empty
    Traceback (most recent call last):
        ...
    AttributeError: 'Dict' object has no attribute 'empty'
    '''
    def __init__(self, **kw):
        super(Dict, self).__init__(**kw)

    def __getattr__(self, key):
        try:
            return self[key]
        except KeyError:
            raise AttributeError(r"'Dict' object has no attribute '%s'" % key)

    def __setattr__(self, key, value):
        self[key] = value
#模块被导入的时候不会执行哦
if __name__=='__main__':
    import doctest
    doctest.testmod()
```

近一个月的学习，大概有了基础概念，但是为了进一步学习计算机其他方面知识决定先转到c为主，等我掌握了c再回来。
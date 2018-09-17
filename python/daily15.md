9.17恶补知识
======
错误，调试，测试

1.错误处理
----------
#### 1.1传统报错是错误码，结构也挺有意思
```
def foo():
    r = some_function()
    if r==(-1):
        return (-1)
    # do something
    return r

def bar():
    r = foo()
    if r==(-1):
        print('Error')
    else:
        pass
```
<br>
#### 1.2现在用try..except..finally..的错误处理机制
```
try:
    print('try...')
    r = 10 / 0
    print('result:', r)
except ZeroDivisionError as e:     #当执行有问题的时候直接跳到这里
    print('except:', e)
finally:                           #执行完except后，有finally就执行
    print('finally...')
print('END')

try...
except: division by zero           #捕捉到错误，所以报错
finally...
END

如果改成r=10/2
try...
result: 5                          #无错，继续执行到print 跳过except
finally...
END
```

注意，所有的错误都继承自BaseException[err](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)
***不但捕获该类型，也捕获它的子类***
```
try:
    foo()
except ValueError as e:
    print('ValueError')
except UnicodeError as e:          #这个捕捉不到，因为是上面的子集错误
    print('UnicodeError')
```

***可以跨越多层调用，只要在合适的层次捕捉，可以减少很多代码***
```
def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:
        print('Error:', e)
    finally:
        print('finally...')
```

#### 1.3用logging记录错误

```
import logging

def foo(s):
    return 10 / int(s)

def bar(s):
    return foo(s) * 2

def main():
    try:
        bar('0')
    except Exception as e:                 
        logging.exception(e)                      #注意这里不一样了

main()
print('END')


$ python3 err_logging.py
ERROR:root:division by zero           
Traceback (most recent call last):
  File "err_logging.py", line 13, in main
    bar('0')
  File "err_logging.py", line 9, in bar
    return foo(s) * 2
  File "err_logging.py", line 6, in foo
    return 10 / int(s)
ZeroDivisionError: division by zero               #继续报错了
END                                               #但是继续执行下一步

```

#### 1.4抛出错误 raise Exception err
因为错误的回馈不可能是凭空产生的，错误其实也是一种class，不同的错误是该class的不同实例<br>
所以理论上我们可以自己定义一个类来抛出自己的错误<br>
```
#err_raise.py
def myerr(ValueError):                           #先定义一个错误class，注意父类
  pass

def foo(n):
  s=int(n)
  if s==0:
    raise myerr('invalid value: %s'%n)
  return 100/n
foo('0')

$ python3 err_raise.py 
Traceback (most recent call last):
  File "err_throw.py", line 11, in <module>
    foo('0')
  File "err_throw.py", line 8, in foo
    raise myerr('invalid value: %s' % s)
__main__.myerr: invalid value: 0                   #这个为抛出的错误
```

***还有一种很常见的记录并抛出错误的方式***
```
def foo(s):
    n = int(s)
    if n==0:
        raise ValueError('invalid value: %s' % s)
    return 10 / n

def bar():
    try:
        foo('0')
    except ValueError as e:
        print('ValueError!')
        raise

bar()
ValueError!                                           #记录错误，喊出来
Traceback (most recent call last):                    #以下抛出来 告诉你哪里错咯 
  File "<stdin>", line 1, in <module>
  File "<stdin>", line 3, in bar
  File "<stdin>", line 4, in foo
ValueError: invalid value: 0

```

而且可以通过except中err，转换成另一个错误
```
try:
    10 / 0
except ZeroDivisionError:
    raise ValueError('你猜你哪里错了')                  #反面例子，抛出的错误不要是一个毫不相干误导人的哦
```

小结
----
在调试的时候你可以用try..except..finally来尝试捕获错误，其中注意except exception as e，然后print出错误来，或者logging之类<br>
或者你可以用 raise 自己抛出错误，raise后可以自定义你的错误，甚至自己定义一个错误class，然后任意抛出<br>


2调试
---------
对于一个程序来说，如果出错时的各个变量数值知道，会更容易修复，首先想到的用print会导致到处都是，还得删除，于是用断言代替<br>

#### 2.1断言 assert
```
def foo(s):
    n = int(s)
    assert n != 0, 'n is zero!'
    return 10 / n

def main():
    foo('0')
```
在断言中，遇到非0布尔值就会抛出AssertionError<br>
```
$ python err.py
Traceback (most recent call last):
  ...
AssertionError: n is zero!
```
在调试时可以用-O关闭assert(注意，是大写英文字母O)
```
$ python -O err.py
Traceback (most recent call last):
  ...
ZeroDivisionError: division by zero
```


#### 2.2logging不会抛出错误，但是可以输出到文件
logging可以设定等级 info debug warning error<br>
```
import logging
logging.basicConfig(level=logging.INFO)

s = '0'
n = int(s)
logging.info('n = %d' % n)                    #报错项目
print(10 / n)

INFO:root:n = 0                               #报错等级INFO
Traceback (most recent call last):
  File "err.py", line 8, in <module>
    print(10 / n)
ZeroDivisionError: division by zero
```
还可以输出到不同地方 以后可以看看


#### 2.3 pdb
这是python的调试器pdb，单步运行，先准备程序<br>
```
#err2.py
s='0'
n=ins(s)
print(10/n)

启动
$ python -m pdb err.py
> /Users/michael/Github/learn-python3/samples/debug/err.py(2)<module>()
-> s = '0'
```
进入之后就是用代码来实现各种调试功能了，具体的调试代码可以百度，这里我先大概体验一下就好了<br>
(体验了一下，其实挺好用的，但是太多的话一步一步执行不可能这样调，不过如果定位到报错的地方然后这样调试一下还是可以接受的)

#### 2.4 pdb.set_trace()
```
# err.py
import pdb

s = '0'
n = int(s)
pdb.set_trace()                               # 运行到这里会自动暂停
print(10 / n)

$ python err.py 
> /Users/michael/Github/learn-python3/samples/debug/err.py(7)<module>()
-> print(10 / n)
(Pdb) p n                                     #查看n的值
0         
(Pdb) c                                       #继续执行
Traceback (most recent call last):
  File "err.py", line 7, in <module>
    print(10 / n)
ZeroDivisionError: division by zero
```
！这样一来效率更高一些了，以后我写小的代码可以考虑用这个调试一下<br>

单元测试
=========
室友在打牌 我去洗澡洗衣服 明儿再看嘿嘿<br>
今天效率很高美滋滋
![images](http://img.mp.itc.cn/upload/20160926/0afd6a8463bc459da835c68e410d49e2.jpeg)
复习最后一天啦 然后就可以继续学习咯
======
### 定制类
在class里定义一些特殊功能
#### 如\_\_len\_\_,\_\_str\_\_
```py
#用class实现fib
class Fib(object):
    def __init__(self):
        self.a, self.b = 0, 1 # 初始化两个计数器a，b

    def __iter__(self):
        return self # 实例本身就是迭代对象，故返回自己

    def __next__(self):
        self.a, self.b = self.b, self.a + self.b # 计算下一个值
        if self.a > 100000: # 退出循环的条件
            raise StopIteration()
        return self.a # 返回下一个值
>>> for n in Fib():
...     print(n)
...
1
1
2
3
5
...
46368
75025
```
#### \_\_getattr\_\_
```py
class Student(object):
    def __init__(self，name):
    	self.name=name
    def __getattr__(self, attr):
        if attr=='age':
            return lambda: 25
        raise AttributeError('\'Student\' object has no attribute \'%s\'' % attr)
```
这个就是一个后备参数，如果调用到name以外就会调用getattr

#### \_\_call\_\_
可以直接对实例进行调用
```py
class Student(object):
	def __init__(self,name):
		self.name=name
	def __call__(self):
		print('yoyoyo success %s',%self.name)
>>>s=student('zhang')
>>>s()
yoyoyo success zhang
```
最后就是用callable('')查看是否可以被调用

### 枚举类
哈，有人问这个意义何在，和dict一样，意义在于不可以任意增减<br>
```py
from enum import Enum,unique
@unique               #可以检查没有重复值
class my(Enum):
	A=1
	B=2
	C=3
	D=4

>>>a=my.A
>>>a
<my.A: 1>
>>>my.B.value
2
```

### 元类
首先class的定义时运行的时候动态创建的，方法就是type()函数。<br>
**用 type()创建类:**
```py
#首先创建在class内的函数
def fn(self,name='hello world'):
	print('halo,%s',% name)

hello =type('Hello',(object,),dict=(hello=fn))
```
先后传入的是:<br>
* class的名称
* 继承父类(注意元组写法)
* class内置方法名称和函数绑定

我日元类现在看起来有点恶心啊，我休息一下看一下后面的部分

### IO
* 在读配置文件(成行读取)时候
```py
for line in f.readlines():
	print(line.strip())#把末尾\n删掉
```

* 在内存中读写哦 StringIO和BytesIO
```py
from io import StringIO
f=StringIO()
f.write('ok')
print(f.getvalue())#读取写入后的str


n=StringIO('hello o ')
print(n.read())
```
***几个点***
* 关于tell(),seek()的用法，0.1.2分别对应开头，当前，末尾
* 在StringIO中定义时指针默认在0，第一次write的时候，指针在默认的0处，所以虽然write追加但是还是覆盖了第二次的时候就是追加了
* 奇怪的是用getvalue()方法一直可以输出，在写文件用file-like-object的时候就不可以了<br>
第三点的解答，要把指针重置seek(0,0)再输出就可以了，也是因为指针指向了最后
* 最后一点
```py
from io import StringIO
f=StringIO('abc')
print(f.readlines)
这样会报错因为
StingIO要么用来读要么用来写，不能同时，要用getvalue()就好了
```

* 操作文件和目录<br>
此情况是在Py文件中操作，用到了os模块<br>
在操作系统中定义的环境变量，全部保存在os.environ这个变量中,还有一部分函数放在os.path

```py
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'
# 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
# 然后创建一个目录:
>>> os.mkdir('/Users/michael/testdir')
# 删掉一个目录:
>>> os.rmdir('/Users/michael/testdir')
#拆分路径要用split
>>>os.path.split('/users/assboy/learn')
('/users/assboy','learn')
#获取拓展名
>>>os.path.splitext('/path/to/file.txt')
('/path/to/file','.txt')
#重命名
>>>os.rename('text.txt','text.py')
#删除文件
>>>os.remove('test.py')
```

* 序列化
平时的程序运行在内存中，可以更改，程序结束时才被系统全部回收，所以要及时传输到磁盘上，所以要通过序列化把变量从内存中变成可存储或者可传输的状态。<br>
python中用pickle模块来实现
```python
import pickle
d=dict(name='zhang')
pickle.dumps(d)
#还有一种直接转换并写入的
f=open('dump.txt','wb')
pickle.dump(d,f)
f.close()
```
读取时候先读到一个bytes再用pickle.loads()
```python
f=open('dump.txt','rb')
d=pickle.loads(f)
f.close()
```  
  
  
* JSON
用来在不同的编程语言中进行传输，JSON表示出来就是一个字符串<br>
用python内置的json
```py
import json
d=dict(name='zhang')
json.dump(d)
'{"name": "zhang"}'

json.loads(json_str)#这就可以把json文件反序列化为python对象了 
```
或者自定义把任何数据类型转换为JSON，需要定义一个default
```py
def studetndict(std):
    return {
        'name':std.name
    }
print(json.dumps(s,default=studetndict))
```
通常class的实例都有一个\_\_dict\_\_属性，就是一个dict用来储存实例变量(除了定义了__slots__的)<br>
所以可以这样
```py
print(json.dumps(s,default=lambda obj:obj.__dict__))
```
反序列过程
```py
def dict2student(d):
    return Student(d['name',d['age']])

```


*json模块的dumps()和loads()函数是定义得非常好的接口的典范。当我们使用时，只需要传入一个必须的参数。但是，当默认的序列化或反序列机制不满足我们的要求时，我们又可以传入更多的参数来定制序列化或反序列化的规则，既做到了接口简单易用，又做到了充分的扩展性和灵活性。*
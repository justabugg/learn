9.19 今天尝试恶补
========
我发现电脑用久了手机打字总是错 很烦

1 IO编程
====
通常运算时数据都在内存由cpu执行，涉及到数据交换（磁盘或网络等）时就需要IO接口<br>
同时要了解**同步IO** **异步IO**

1.0文件读写
-----
通常都是通过系统提供的接口读写文件中的数据<br>
#### 1.1读文件
```
f=open('/c/users/assboy/learn/python/daily1.md','r')   #打开一个文件

f.read()                                               #一次读取文件的全部内容到内存
"hellow,world"                                         #注意 是以str对象表示

f.close()                                              #当然要记得关闭文件 不然占用内存
```
在文件使用二进制啊，不是utf-8编码啊，有非法字符可以传入encoding和error参数来调用一个偏函数(？是这个意思不)<br>
```
>>> f = open('/Users/michael/test.jpg', 'rb')                                       #这是二进制rb
>>> f.read()
b'\xff\xd8\xff\xe1\x00\x18Exif\x00\x00...' # 十六进制表示的字节 

>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk')                         #这是用gbk编码的                      
>>> f.read()
'测试'
 
>>> f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')        #这是忽视乱码                          
```
但是可能出错了导致close不可以调用，所以通过try···finally实现<br>
```
try:
  f=open('/path/to/file','r')                          #打开
  print(f.read())                                      #读
finally:
  if f:
    f.close()                                          #关闭
```
这个太过于繁琐了，所以python引入了with帮助调用close<br>
```
with open('/path/to/file','r') as f:
  print(f.read())
```
最后**防止一个文件太大内存爆掉，记得read(size)或者readlines()**

#### 1.2 file-like Object


#### 1.3 写文件
```
f=open('path/to/file','w')           #这里传入标识符w，或者rw
f.write('hello world')               #写入数据
f.close
```
和之前一样，写入write时只是把文件**写入了内存缓存区**，只有在调用close的时候才**存入磁盘**<br>
```
with open('/c/users/assboy','w') as f:
  f.write('hello world')
```
**在用w写入的时候，如果写入同样的文件将会覆盖，如果希望append那就传入a**<br>

2.0 StringIO和BytesIO
-------
####  2.1 StringIO
顾名思义在内存中读写**str,只有str**<br>
```
>>>from io import StringIO
>>>f=StringIO()                     #首先定义一个
>>>f.write('hello')                 #写入二连
5
>>>f.write('world!')
6
>>>print(f.getvalue())              #getvalue用于获得写入后的str
helloworld!

f=StringIO('hello\nhi!\ngoodbye!')  #初始化StringIO
>>>while True:
···  s=f.readline()
···  if s=='':
···    break
···  print(s.strip())
···
hello
hi!
goodbye!
```

#### 2.2 BytesIO
StringIO只能操作str，如果要操作二进制数据，就需要使用BytesIO,也是实现了在内存中读写bytes<br>
```
>>>from io import BytesIO
>>>f.BytesIO()
>>>f.write('中文'.encode('utf-8'))         #注意是经过utf-8编码的bytes
6
>>>print(f.getcalue())
b'\xe4\xb8\xad\xe6\x96\x87'
```

### 2.3 关于StringIO,BytesIO的小结和小拓展
[感谢这个细心的总结](https://www.liaoxuefeng.com/discuss/001409195742008d822b26cf3de46aea14f2b7378a1ba91000/00151678811301342d71e0bb51144ad99baf09f99e4068e000)

3.0 操作文件和目录
------
平时在命令行里通过命令操作文件，目录其实只是调用了了系统提供的接口函数，python内置的os模块也可以直接调用操作系统提供的接口函数<br>
```
>>>import os
>>>os.name
'nt'                                            #'nt'代表Windows，'posix'是Linux,Unix,Mac OS X
```
#### 3.1 环境变量
一半是指在操作系统中用来指定操作系统运行环境的一些参数，如系统文件夹位置等<br>
在操作系统中定义的环境变量都放在os.environ里

#### 3.2 操作文件和目录
用来操作的函数一部分在os模块里一部分在os.path里
```
# 查看当前目录的绝对路径:
>>> os.path.abspath('.')
'/Users/michael'                             # 在某个目录下创建一个新目录，首先把新目录的完整路径表示出来:
>>> os.path.join('/Users/michael', 'testdir')
'/Users/michael/testdir'
>>> os.mkdir('/Users/michael/testdir')       # 然后创建一个目录:                      
>>> os.rmdir('/Users/michael/testdir')       # 删掉一个目录:
```
**把两个路径合成一个时，不要直接拼字符串，而要通过os.path.join()函数，这样可以正确处理不同操作系统的路径分隔符。**
```
>>> os.path.split('/Users/michael/testdir/file.txt')
('/Users/michael/testdir', 'file.txt')
```
**同样的道理，要拆分路径时，也不要直接去拆字符串，而要通过os.path.split()函数，这样可以把一个路径拆分为两部分，后一部分总是最后级别的目录或文件名**
```
>>> os.path.splitext('/path/to/file.txt')
('/path/to/file', '.txt')
```
**os.path.splitext()可以直接让你得到文件扩展名，很多时候非常方便：**<br>


***这些合并、拆分路径的函数并不要求目录和文件要真实存在，它们只对字符串进行操作。***
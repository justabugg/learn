9.20 还债daily16
========
下午要练车 现在把IO编程结束中午抽空解决掉单元测试和文档测试

1.0 IO编程
-----------

### 1.1序列化
程序运行时所有变量都是在内存中，结束后变量占用的内存就会被操作系统回收<br>
序列化就是把变量从内存中编程可存储或者传输的过程,python中交picking其他语言serialization,marshalling,flattening<br>
序列化后就可以存入磁盘或者通过网络传输(这里回忆一下是以什么样的字符编码，内存中str unicode,磁盘或者传输就是bytes utf-8)<br>
同理有反序列化重新读到内存里<br>
用pickle模块实现序列化
```
>>>import pickle
>>>d = dict(name='bob',age=20,score=88)
>>>pickle.dumps(d)                           #将d序列化
b'\x80\x03}q\x00(X\x03\x00\x00\x00ageq\x01K\x14X\x05\x00\x00\x00scoreq\x02KXX\x04\x00\x00\x00nameq\x03X\x03\x00\x00\x00Bobq\x04u.'
```
**pickle.dumps()可以把任意对象序列化成一个bytes，然后可以操作写入文件，或者用pickle.dump()直接序列化写入一个file-like-Object**
```
>>>f=open('dunm.txt','wb')
>>>pickle.dump(d,f)                    #序列化d后写入f
>>>f.close()
```
同理可以用pickle.loads()反序列化对象，也可以用pickle.load()从一个file-like-Object 反序列化出对象
```
>>>f=open('dunp.txt','rb')
>>>d=pickle.load(f)                    #从file-like-Object中反序列化出来
>>>f.close()
>>>d
{'age': 20, 'score': 88, 'name': 'Bob'}
```

### 1.2 JSON   utf-8编码
JSON表示的对象就是标准的javaScript语言的对象
```
>>>import json 
>>>d=dict(name='zhang')
>>>json.dumps(d)                                               #将python编程JSON格式
'{"name":"zhang"}'

>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> json.loads(json_str)                                        #反序列
{'age': 20, 'score': 88, 'name': 'Bob'}
```


### 1.3 JSON进阶
在用class的实例转JSON的时候报错，因为dump不知道怎么转，其实有其他参数可以改造<br>

```
#先将实例转为dict
def student2dict(std):              
    return {
        'name': std.name,
        'age': std.age,
        'score': std.score
    }

>>> print(json.dumps(s, default=student2dict))
{"age": 20, "name": "Bob", "score": 88}


#或者偷个懒
print(json.dumps(s, default=lambda obj: obj.__dict__))
```
同理先load出一个dict，再转为实例
```
def dict2student(d):
    return Student(d['name'], d['age'], d['score'])

>>> json_str = '{"age": 20, "score": 88, "name": "Bob"}'
>>> print(json.loads(json_str, object_hook=dict2student))
<__main__.Student object at 0x10cd3c190>
```


### 1.4小结
python的pickle模块是序列化的方法，但是如果想让序列化更加通用就可以用json模块<br>
关于接口：json的dumps(),loads()是很好的典范，使用时只需要传入一个必须的参数，也可以传入更多的可选参数来拓展，这样接口既简单又灵活
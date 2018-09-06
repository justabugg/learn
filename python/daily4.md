unicode ascll utf-8编码的问题  在内存中使用unicode在硬盘或者传输时utf-8
b'abc'的问题 他的意思是其中的每个字符例如a的字节是1而不是'abc'中的2个字节（unicode中）
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128) 
用encode改变为bytes
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
用decode由bytes变为str
len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数
今天全搞字符串格式这些了 明天多看点高级函数了
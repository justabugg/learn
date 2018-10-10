10.9 复习第二天
====

高级特性
------

#### 迭代
在迭代dict的时候 
```py
for value in d.values()
for k,v in d.items()
```
字符也可以迭代 那么input的内容也可以迭代啦

#### 生成器 迭代器
注意生成器中yield的执行顺序dDD(next执行yield返回，再next从返回yield处继续)
不过这里遇到了一个问题
```py
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```
为什么用for来调用每次都无法返回done，按理说继续从yield执行会轮到return<br>
顺便解决了杨辉三角 神清气爽<br>

函数式编程
---------


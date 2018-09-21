9.21 昨天练车 没有补16，今天尝试一哈
========

1.0 进程和线程
=====
首先知道cpu与多任务的关系，对于操作系统来说任务就是进程，在进程内可能还有许多子任务，叫做线程<br>
综上如果想实现多任务主要有三种方法，显而易见多进程+多线程复杂一些，因为多任务之间还是需要通信协调。

1.1 多进程
-----
首先在Unix/Linux/mac上有fork()来系统调用，fork()调用一次**返回两次**分别是父进程返回子进程的id，子进程返回0<br>

因为父进程要记住每个从自己复制出来的进程，而子进程只需要调用**getppid()**

```python
print('process(%s)start..'%os.getppid())
pid =os.fork()
if pid==0:
	print('i am chile process(%s) from %s.'%(os.getpid(),os.getppid()))
else:
    print('I (%s) just created a child process (%s).' % (os.getpid(), pid))

Process (876) start...
I (876) just created a child process (877).
I am child process (877) from 876.
```

现在在windows调用multiprocessing模块
```python
from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')

Parent process 928.
Process will start.
Run child process test (929)...
Process end.
```
需要一个传入一个执行函数和参数，再创建一个process实例，用start()启动，其中join()可以等待子进程结束后继续运行，**常用于进程之间的同步**


1.2 pool
-----
前提是需要批量创建子进程
```python
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        #这里注意一下阻塞和异步非阻塞
        p.apply_async(long_time_task, args=(i,))   
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')

 Parent process 4104.
Waiting for all subprocesses done...
Run task 0 (11632)...
Run task 1 (12332)...
Run task 2 (7860)...
Run task 3 (11524)...
Task 0 runs 0.37 seconds.
Run task 4 (11632)...
Task 2 runs 0.47 seconds.
Task 1 runs 2.05 seconds.
Task 3 runs 2.38 seconds.
Task 4 runs 2.09 seconds.
All subprocesses done.
```
对Pool对象调用join()方法会等待所有子进程执行完毕，调用join()之前必须先调用close()，调用close()之后就不能继续添加新的Process了。
[apply](https://www.liaoxuefeng.com/discuss/001409195742008d822b26cf3de46aea14f2b7378a1ba91000/0015348123154065faed2c29170427395e021fba405d83d000))
1.3 子进程
----
很多时候子进程是需要输入输出，这里用到了subprocess具体写在python2里<br>
[python2](https://github.com/justabugg/target/blob/master/python/python2.md#%E7%AC%AC%E5%8D%81%E4%B8%89%E4%B8%AA-%E5%AD%90%E8%BF%9B%E7%A8%8B%E6%8E%A7%E5%88%B6%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA)

1.4 进程间通信
----
```python
from multiprocessing import Process, Queue
import os, time, random

# 写数据进程执行的代码:
def write(q):
    print('Process to write: %s' % os.getpid())
    for value in ['A', 'B', 'C']:
        print('Put %s to queue...' % value)
        q.put(value)
        time.sleep(random.random())

# 读数据进程执行的代码:
def read(q):
    print('Process to read: %s' % os.getpid())
    while True:
        value = q.get(True)
        print('Get %s from queue.' % value)

if __name__=='__main__':
    # 父进程创建Queue，并传给各个子进程：
    q = Queue()
    pw = Process(target=write, args=(q,))
    pr = Process(target=read, args=(q,))
    # 启动子进程pw，写入:
    pw.start()
    # 启动子进程pr，读取:
    pr.start()
    # 等待pw结束:
    pw.join()
    # pr进程里是死循环，无法等待其结束，只能强行终止:
    pr.terminate()

运行结果如下：

Process to write: 50563
Put A to queue...
Process to read: 50564
Get A from queue.
Put B to queue...
Get B from queue.
Put C to queue...
Get C from queue.
```
这里的评论区出现了一个问题(https://www.liaoxuefeng.com/discuss/001409195742008d822b26cf3de46aea14f2b7378a1ba91000/00151937911313973d2490e71dc4f919b7f71c1616f3f8d000?page=1)

2.1 多线程
------
\__thread是封装的低级模块，threading是高级模块。<br>
启动线程就是传入一个函数和参数并创建Thread实例，然后调用start()
```python
import time, threading

# 新线程执行的代码:
def loop():
    print('thread %s is running...' % threading.current_thread().name)
    n = 0
    while n < 5:
        n = n + 1
        print('thread %s >>> %s' % (threading.current_thread().name, n))
        time.sleep(1)
    print('thread %s ended.' % threading.current_thread().name)

print('thread %s is running...' % threading.current_thread().name)
t = threading.Thread(target=loop, name='LoopThread')
t.start()
t.join()
print('thread %s ended.' % threading.current_thread().name)

执行结果如下：

thread MainThread is running...
thread LoopThread is running...
thread LoopThread >>> 1
thread LoopThread >>> 2
thread LoopThread >>> 3
thread LoopThread >>> 4
thread LoopThread >>> 5
thread LoopThread ended.
thread MainThread ended.
```
由于任何进程默认就会启动一个线程，我们把该线程称为主线程，主线程又可以启动新的线程，Python的threading模块有个current_thread()函数，它永远返回当前线程的实例。主线程实例的名字叫MainThread，子线程的名字在创建时指定

2.2 Lock
------
**多进程和多线程最大不同，多进程变量互不影响，多线程任意一个变量可以被任意更改**
```python
import time, threading

# 假定这是你的银行存款:
balance = 0

def change_it(n):
    global balance          #注意这个global 它将被两个线程共同调用          
    balance = balance + n
    balance = balance - n

def run_thread(n):
    for i in range(100000):
        change_it(n)

t1 = threading.Thread(target=run_thread, args=(5,))
t2 = threading.Thread(target=run_thread, args=(8,))
t1.start()
t2.start()
t1.join()
t2.join()
print(balance)
```
按理说应该输出0，可是循环次数多了，分别修改了global的值，那么结果就不是了
```python
balance=balance+n          #弄懂这个式子的本质
```
所以综上，要保证线程在执行chang_it的时候不能被打断也就是threading.Lock()
```python
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
```
这里使用try结构的原因是在锁执行后一定要释放，不然其他线程会一直等下去。


2.3 多核cpu
--------
因为python设计师有了GIL全局锁，所以在多核cpu上跑也只能用一个核<br>
关于GIL(http://cenalulu.github.io/python/gil-in-python/)

小结
====
今天心很乱 到此为止吧 晚上要出去吃饭 回来看有没有空补16的调试内容了
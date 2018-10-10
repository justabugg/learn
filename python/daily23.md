复习第三天 函数式编程
========
高阶函数
---------
#### map和reduce
记得generator的惰性运算所以一般用一个list输出map函数结果<br>
在reduce和map结合str转数字的函数中，我能想到reduce的用法，但是把单个的str转为数字没有想到，虽然很简单
```py
def char(s):
	  digits={'0': 0, '1': 1, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9}
	  return digits[s]
```
关于小数点的问题，在把123.456字符串化为数字时候用map，reduce结合再用到了
[find()](https://blog.csdn.net/yolandera/article/details/80264876)


#### filter筛选函数
埃氏筛法可以理解，方法也懂 还是generator中yield和return的执行问题，今晚解决<br>
解决了回文数 自己想的太复杂了 

#### sorted排序函数
知道可以用key来自定义排序就好

#### 返回函数
主要注意闭包中引用变量的问题

小结
=====
今天四周年忙了一点 下午无心看书 明天把赋值 generator解决<br>
今天跑了五千米 配速5.26，加油
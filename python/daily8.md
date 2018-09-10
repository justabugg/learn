9.10日志
====
周一早上满课贼忙，刚洗完衣服也没时间午睡咯,昨晚吃鸡没有写target 后悔，好后悔<br>
先做昨天高级函数的课后作业
-----
```
利用map()函数，把用户输入的不规范的英文名字，变为首字母大写，其他小写的规范名字。输入：['adam', 'LISA', 'barT']，输出：['Adam', 'Lisa', 'Bart']：
def nor(name):
  return name[0].upper()+name[1:].lower()
print(list(map(nor,L1)))
```
```
Python提供的sum()函数可以接受一个list并求和，请编写一个prod()函数，可以接受一个list并利用reduce()求积：
def prod(L):
        return reduce(lambda x,y:x*y,L)
```
filter函数
----
**和map()类似，filter()也接收一个函数和一个序列，返回一个generation。和map()不同的是，filter()把传入的函数依次作用于每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素。**
```
def not_empty(s):
    return s and s.strip()

list(filter(not_empty, ['A', '', 'B', None, 'C', '  ']))
# 结果: ['A', 'B', 'C']

```
这里有一个重要的filter求素数写在target里（埃氏筛法）[aishi](https://github.com/justabugg/target/blob/master/python/python.md)

排序算法sorted
----
内置的sorted函数可以自动排序
```
>>> sorted([36, 5, -12, 9, -21])
[-21, -12, 5, 9, 36]
```
也可以传入key参数 进行有条件的排序
```
>>> sorted([36, 5, -12, 9, -21], key=abs)
[5, 9, -12, -21, 36]
```
**记住 sorted是作用于列表中每一个成员，刚才我在做练习的时候还在纠结于怎么把list中的[0][1]取出来**  

返回函数 好玩
------
```
def lazy_sum(*args):
    def sum():
        ax = 0
        for n in args:
            ax = ax + n
        return ax
    return sum
```
内部的sum可以引用输入的参数和局部参数，**当sum返回时，参数变量保存在返回的函数里，这叫***闭包***！**<br>
而且每次返回的函数时不一样的（我理解为存放地址不同 肯定不一样啊<br>

闭包
------
一个新的例子，反映了返回函数不是立即执行<br>
```
def count():
    fs = []
    for i in range(1, 4):
        def f():
             return i*i
        fs.append(f)
    return fs
输出结果
>>> f1()
9
>>> f2()
9
>>> f3()
9       #因为调用完for后 i已经变成3  这时候f（）就都是9了
```
**闭包时要注意不要引用循环变量，如果一定要用，创建一个新函数绑定当前的值**
```
def count():
    def f(j):
        def g():
            return j*j
        return g
    fs = []
    for i in range(1, 4):
        fs.append(f(i)) # f(i)立刻被执行，因此i的当前值被传入f()
    return fs

>>> f1, f2, f3 = count()
>>> f1()
1
>>> f2()
4
>>> f3()
9

```
这里的f(j)传了参数给g()所以在f(）时闭包给了g，循环参数没有变

先去跑步了，回来把计数器做掉
-----
强迫症很难受啊，样下去我的target里问题会越来越多的啊 我决定从这周开始周末把target问题一个个解决<br>
然后新一周开新的<br>
希望这个月底之前可以把python基本解决掉，再用一个月做点实战再去考虑java还是js，或者把html和js还有css学了赚点零花钱去<br>
先走着吧，还是略带迷茫<br>
写target去了
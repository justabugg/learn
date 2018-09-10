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
返回函数
------


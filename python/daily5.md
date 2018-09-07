关于格式和C语言一样的 记住%s字符串%%是转义%  还可以控制输出的位数之类的 %.1f  
列表list 相关的list.append   list.insert(1,x) list.pop  多维数组L[0][0]  
元组tuple 要记住不能修改 定义时就确定下来了 定义一个元素的tuple时t=（1，)   
条件判断 elif  
for x in n循环很有用 计算0~10  
sum=0  
for a in range(1:11):  
  sum=sum+a  
  print(sum)  
然后就是while循环 true进行false退出 可以用break提前退出 
字典dict 有value和key{'key':Value,'key2':value2}  也可以通过d['张元']=66的方式放入字典  
用get（）可以知道key存不存在  d.get('thomas')
dict可以用在需要高速查找的很多地方，在Python代码中几乎无处不在，正确使用dict非常重要，需要牢记的第一条就是dict的key必须是不可变对象。  
这是因为dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。  
要保证hash的正确性，作为key的对象就不能变。在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key：  
记住dict是牺牲了大量空间换来调用速度  
set也是key的集合，没有重复的key  
创建set要通过list，s=set([1,2,3])  
在定义函数时不一定只要能输出对就可以 而是在错的时候可以抛出相应的错误（我记得在数据结构说过某种类似的？忘了 我该看数据结构了）  
肚子好饿 洗个衣服出去吃饭了       ep.1  
参数分为默认参数（x,n=2) 可变参数(*args)  
在输入的是tuple或者list时可以*L直接输入  
关键字参数**kwdef person(name, age, **kw):  
命名关键字参数 *
print('name:', name, 'age:', age, 'other:', kw)  
def person(name, age, *, city, job):  
    print(name, age, city, job)  
尾递归问题第二次看清晰了很多 其实就是意思在return时时一个本身函数而不是需要进行一次计算（计算在调用参数的时候已经计算好了）  
马上八点了 跟小胖出去跑步了 九点回来洗澡洗衣服 今天12.30断网 回来还可以看一会       ep.2  




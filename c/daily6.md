分支和跳转
====
### if
* 老问题 避免整数除法p178
* if与else的对应关系 p186

### getchar() putchar()
* 只用来处理字符所以更快更简洁

### ctype.h
* 用一些函数原型按条件返回真假

### 条件运算符experession1?a:b
```c
a=a/b
a+=((a%b==0))?0:1
```

### 辅助循环 continue break
* 用continue来做占位符比;明显
* 注意一下continue跳过循环体后的执行，对于for来说执行更新表达式再执行测试条件
* break和continue只影响内层所在嵌套的循环

### switch和break
* 如果没有匹配到case，如果有default就跳，没有就执行switch下一句
* 在switch中的只能是整数型(包括char)
* continue不可用于switch
* 多重标签的用法

### goto
* 不建议使用，只建议在函数出错的位置goto所需要层次(break只能跳出一层循环)


* 编译器的嵌套范围C99是127层
* 用while和continue丢弃输入
```c
while(getchar()!='/n')
continue;
```

### 程序设计思路 p194
## 小结
依旧是基本概念，还不错已经涉及到一些我以前不了解的知识了

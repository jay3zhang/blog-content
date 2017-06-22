* 随机数
python的随机数可以看做是真随机的。
```
>>> import random
>>> random.random()
0.7405984757173073
>>> random.random()
0.6869184361950926
>>> random.random()
0.4785728545692216
```
可以用随机种子使两次随机结果相同。
```
>>> random.seed(10)
>>> random.random()
0.5714025946899135	#第一次使用种子
>>> random.random()
0.4288890546751146
>>> random.random()
0.5780913011344704
>>> random.random()
0.20609823213950174
>>> random.seed(10)
>>> random.random()
0.5714025946899135	#第二次使用种子
```

* enumerate函数
```
enumerate(iterable[, start]) -> iterator for index, value of iterable
 |
 |  Return an enumerate object.  iterable must be another object that supports
 |  iteration.  The enumerate object yields pairs containing a count (from
 |  start, which defaults to zero) and a value yielded by the iterable argument.
 |  enumerate is useful for obtaining an indexed list:
 |      (0, seq[0]), (1, seq[1]), (2, seq[2]), ...
```
enumerate函数返回元素的索引值和元素的值：
```
>>> for ind,val in enumerate([10,20,30,40]):
...     print(ind,val)
...
0 10
1 20
2 30
3 40
```
enumerate函数有个可选参数指定输出索引的初始值（默认为0）：
```
>>> for ind,val in enumerate([10,20,30,40],5):
...     print(ind,val)
...
5 10
6 20
7 30
8 40
```

* collections 模块的 Counter()-计数器
```
In: Counter('aabcB')
Out: {'a':2,'b':1,'c':1,'B':1}
```

* 字符串替换 str.replace *vs* re.sub
	- 能用replace(),尽量用
	- 它避免了正则表达式的所有缺陷（如转义）
	- 更快
[stackoverflow回答](https://stackoverflow.com/questions/5668947/use-pythons-string-replace-vs-re-sub)

* zip() 函数
zip() 压缩，zip(* )解压
zip(*argv)
对于矩阵，zip(*mat) 会使行列互换，第一行为第一列...

* and 运算符
有0是为0，没0时等于后者
```
>>> t = 0 and 3
>>> t
0
>>> t = 3 and 9
>>> t
9
>>> t = 9 and 6
>>> t
6
>>> t = 9 and 0
>>> t
0
```

* eval() 函数
执行字符串或文件中的python表达式
```
eval('1+2') = 3
```
类似的函数还有exec()-执行字符串或文件中的python语句


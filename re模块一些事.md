* match对象

属性和方法 | 说明
----|----
 Pos	| 搜索的开始位置
 Endpos	| 搜索的结束位置
 String | 搜索的字符串
 Re | 当前使用的正则表达式的对象
 Lastindex | 最后匹配的组索引
 Lastgroup | 最后匹配的组名
 group(index=0)	 | 某个分组的匹配结果。如果index等于0，便是匹配整个正则表达式
 groups() | 所有分组的匹配结果，每个分组的结果组成一个列表返回
 Groupdict() | 返回组名作为key，每个分组的匹配结果座位value的字典
 start([group]) |  获取组的开始位置
 end([group]) | 获取组的结束位置
 span([group]) | 获取组的开始和结束位置
 expand(template) | 使用组的匹配结果来替换模板template中的内容，并把替换后的字符串返回

　
[参考链接](http://xukaizijian.blog.163.com/blog/static/170433119201111213111562/)

* re模块的split()方法：re.split()

```
>>> help(re.split)
Help on function split in module re:
# re模块的split()函数
split(pattern, string, maxsplit=0, flags=0)
    Split the source string by the occurrences of the pattern,
    returning a list containing the resulting substrings.  If
    capturing parentheses are used in pattern, then the text of all
    groups in the pattern are also returned as part of the resulting
    list.  If maxsplit is nonzero, at most maxsplit splits occur,
    and the remainder of the string is returned as the final element
    of the list.

>>> help(str.split)
Help on method_descriptor:
一般用到的split()函数
split(...)
    S.split(sep=None, maxsplit=-1) -> list of strings

    Return a list of the words in S, using sep as the
    delimiter string.  If maxsplit is given, at most maxsplit
    splits are done. If sep is not specified or is None, any
    whitespace string is a separator and empty strings are
    removed from the result.
```
**示例对比**
以空格分割字符串info='computer 20.00  1999   100页'，空格数为1个，2个，3个。
```
>>> info.split(' ')
['computer', '20.00', '', '1999', '', '', '100页']
>>> re.split(r'\s+',info)
['computer', '20.00', '1999', '100页']
```

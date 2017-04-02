Title: 记录遇到的一些配置bug
Date: 2017-3-20 11:03 
Category:  Note
Tags: 
Author: jay3zhang 
Summary: 记录一些非代码问题的bug

## 1. py2和py3共存时，python3 pip list 的格式问题
问题：

　　DEPRECATION: The default format will switch to columns in the future. You can use --format=(legacy|columns) (or define a format=(legacy|columns) in your pip.conf under the [list] section) to disable this warning.

解决的办法：

　　在`C:\\ProgramData`文件夹下创建pip文件夹，然后在该文件夹下新建`pip.ini` 文件（例如` C:\ProgramData\pip\pip.ini`）。然后打开文件，在文件中写入

    [list]
    format=columns

## 2. 用pelican 生成博客时，会出现.md文件的date无法解析
问题：

　　Value error: embedded null byte when using pelican content command to generate my site on Windows

解决办法：

　　设定正确的locale，中文参见[这-中文][2]，and [here for english][1]

还有一种解决方法是在配置文件pelicanconf.py中设置locale，

eg.` locale = 'en' `

[1]: http://stackoverflow.com/a/42869865/7729978
[2]:http://xingjian.me/how-to-fix-value-error-embedded-null-byte-error.html

## mongodb报错: 计算机积极拒绝服务
errno:10061由于目标计算机积极拒绝，无法连接

原因：mongodb服务未启动，mongodb安装时不会作为windows服务安装，在**服务**里没有mongodb相应的项。

解决：把mongodb作为windows服务，开机自动启动，也可以设为手动开启。另外建议把mongodb加到环境变量path中，这样就不用`cd`到`bin`目录下才能运行mongodb了。
详情参见[这里][3]

[3]:http://www.cnblogs.com/meitian/p/4614389.html

## mongodb 改变默认data路径

运行mongod命令，会以默认路径d:/Data/db 运行，每次输入指定路径太麻烦。改变方法：


## py安装lxml库出错(安装Twisted库出错时方法一样)

1. 安装wheel
2. 下载lxml库到`..\Lib\site-packages`文件夹，在该文件夹打开命令行（`shift+右键`会出现"在此处打开命令行"），
3. `pip install <lxml的全名>`， eg. `pip install lxml-3.6.0-cp35-cp35m-win32.whl`

## python写入文件时，以中文写入
```
with open('test.txt', 'a', encoding='utf-8') as f :
	f.write(json.dumps(content, ensure_ascii=False))
```
## 在MarkDown中使用尖括号<>
在MarkDown中，使用尖括号"<"和">"，会被文本默认为HTML语句。这将导致尖括号本身及尖括号中的内容都不会被显示。

	解决方法：使用转义字符。使用 "&lt;" 代替 “<” , 使用 "&gt;" 代替 ">"。 例如要输出<a>，则需要写为&lt;a&gt;   　注意分号;


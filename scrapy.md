## scrapy基础
```
--help	：scrapy各命令
version	：scrapy版本
version -v 	：scrapy及其组件版本
list	：列出工程目录下的所有spider
view url	：查看网页源码在浏览器中的样子
parse url	：使用spider中的parse函数解析网页
shell url	：可以用来分析网页
runspider spidername	：运行单个盘爬虫
bench	：基准测试，检测scrapy安装是否成功
```

## scrapy组件

```
extract()  返回列表
extract_first()  返回列表第一个
extract_first(default='error')  设置返回值为空时，返回字符串'error'
```
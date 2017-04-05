# Resquests自定义

* **session()**
```
import resquests

def DIY_requests():
	from requests import Request,Session
	s = Session()
	headers = {'User-Agent' : 'fake1.3.4'}
	#生成请求
	request = Request('GET', url, auth=('username','password'), headers=headers)
	# auth为认证信息

	# 准备请求
	prepped = request.prepare()
	print(prepped.body)
	print(prepped.headers)
	# 自定义请求何时发生
	response = s.send(prepped, timeout=5)
	print(response.status)
	print(response.request.headers)
	print(response.text)
```

* resquests库不支持相对链接
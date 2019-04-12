## requests

### 安装

```bash
pip install requests
```

### 导入

```python
import requests
```

### 请求

```python
url = 'https://www.baidu.com/'
data = {'key': 'value'}
headers = {'user-agent': 'my-app/0.0.1'}
files = {'file': open('report.xls', 'rb')}
proxies = {
  "http": "http://10.10.1.10:3128",
  "https": "http://10.10.1.10:1080",
}


# 	请求方法
r = requests.get(url, headers=headers)
r = requests.post(url, data=data)
r = requests.post(url, json=payload)	# 数据自动转换为json格式
r = requests.post(url, files=files)		# post一个文件
r = requests.put(url, data=data)
r = requests.delete(url)
r = requests.head(url)
r = requests.options(url)

# 路径传参
payload = {'key1': 'value1', 'key2': 'value2'}
r = requests.get(url, params=payload)

# 参数
headers=headers				# 设置请求头
data=data					# post提交的数据
json=data					# post提交的数据，自动转换为json格式
files=files					# post提交的文件
allow_redirects=False		# 禁止重定向
timeout=0.001				# 设置超时
verify=False				# 忽略SSL证书验证 默认为True
stream=True					# 推迟下载响应体，链接处于打开状态r.close关闭或者使用with
hooks=dict(response=print_url)# 钩子，print_url为回调函数
proxies=proxies				# 代理

```

### 响应

```python
r.url						# 请求的网址
r.text						# 响应的内容，文本格式
r.content					# 响应内容，二进制格式
r.json()					# 响应内容，json格式
r.encoding					# 响应的编码格式
r.encoding = 'gbk'			# 指定响应的编码格式
r.raw						# 原始套接字响应，请求中需要设置stream=True
r.iter_content(chunk_size)	# 迭代器
r.iter_lines()				
r.status_code				# 响应状态码
r.headers					# 响应头
r.request.headers			# 请求头
r.cookies.get_dict()		# 字典格式的cookies
r.history					# 追踪重定向
```

### 会话对象

同一个session实例对象发出的所有请求之间保持cookie

[官方文档](http://docs.python-requests.org/zh_CN/latest/api.html#requests.Session.params)

```python
s = requests.Session()				# 实例化一个session对象

s.auth = ('user', 'pass')
s.headers.update({'x-test': 'true'})
s.cert								# ssl证书
s.close()							# 关闭会话
s.cookies							# cookie
s.delete(url, **kwargs)				# 发送delete请求
s.get(url, **kwargs)				# get请求
s.get_adapter(url)					# 返回给定URL的相应​​连接适配器。
s.get_redirect_target(resp)			# 收到回复。返回重定向URI或None
s.head(url, **kwargs)				# head请求
s.headers							# 设置请求头
s.hooks								# 时间处理挂钩
s.max_redirects 					# 允许的最大重定向次数，默认30，超过抛异常
s.merge_environment_settings(url, proxies, stream, verify, cert)		
									# 检查环境并将其与某些设置合并。
s.mount(prefix, adapter)			# 将连接适配器注册到前缀。
s.options(url, **kwargs)			# options请求
s.params							# 参数
s.post(url, data=None, json=None, **kwargs)
									# post请求
s.prepare_request(request)			# 构造一个PreparedRequest进行传输并返回它。 PreparedRequest具有从Request实例和Session的实例合并的设置。
s.proxies 							# 代理
s.put(url, data=None, **kwargs)		# put请求
s.rebuild_auth(prepared_request, response)
# 重定向时，我们可能希望从请求中删除身份验证，以避免泄露凭据。此方法会尽可能智能地删除和重新应用身份验证，以避免凭据丢失。
s.rebuild_method(prepared_request, response)
# 重定向时，我们可能希望根据某些规范或浏览器行为更改请求的方法。
s.rebuild_proxies(prepared_request, proxies)
# 此方法通过考虑环境变量来重新评估代理配置。如果我们被重定向到NO_PROXY覆盖的URL，我们将删除代理配置。否则，我们为此URL设置了丢失的代理密钥（如果它们被先前的重定向剥离）。
# 此方法还会在必要时替换Proxy-Authorization标头。

s.request(method, url, params=None, data=None, headers=None, cookies=None, files=None, auth=None, timeout=None, allow_redirects=True, proxies=None, hooks=None, stream=None, verify=None, cert=None, json=None)

s.resolve_redirects(resp, req, stream=False, timeout=None, verify=True, cert=None, proxies=None, yield_requests=False, **adapter_kwargs)
# 收到回复，返回响应的生成器
s.send(request, **kwargs)			# 发送给定的PreparedRequest。
s.stream							# 流响应内容默认。
s.trust_env 						# 信任环境设置用于代理配置，默认身份验证等。
s.verify							# SSL验证默认值。

```




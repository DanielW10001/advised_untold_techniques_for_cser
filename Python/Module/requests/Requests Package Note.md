# Requests Package Note

`pip install requests[ pysocks]`

```python3
import requests
proxies = {'http': 'socks5://127.0.0.1:1080', 'https': 'socks5://127.0.0.1:1080'}
try:
    cookie_jar = requests.cookies.RequestsCookieJar()
    cookie_jar.set('VALUE'(, 'VALUE')*, KEY='VALUE'(, KEY='VALUE')*)

    response = requests.(get|delete|head|options)('URL', params={'KEY': 'VALUE', 'KEY': ['VALUE', 'VALUE']}, headers={'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36'}, cookies={'KEY': 'VALUE'}|COOKIE_JAR, proxies=proxies, timeout=SEC)
    response = requests.(put|post)('URL', params={'KEY': 'VALUE', 'KEY': ['VALUE', 'VALUE']}, data=({'KEY': 'VALUE', 'KEY': ['VALUE', 'VALUE']}|[('KEY', 'VALUE'), ('KEY', 'VALUE')]|'DATA'), json='JSON_DATA', headers={'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.116 Safari/537.36'}, cookies={'KEY': 'VALUE'}|COOKIE_JAR, proxies=proxies, timeout=SEC)

    response.raise_for_status()
except requests.exceptions.ConnectionError:
    # ...
except requests.exceptions.HTTPError:
    # ...
except requests.exceptions.Timeout:
    # ...
else:
    # Cookies
    cookie_jar = response.cookies
    value = response.cookies['KEY']

    # Text
    text = response.text

    # Bytes
    content = response.content

    # JSON
    try:
        jsonobj = response.json()
    except ValueError:  # JSON Parse Error
        # ...
    else:
        try:
            # Access jsonobj
        except KeyError:  # Invalid Dict Key
            # ...
        except IndexError:  # Invalid List Index
            # ...
```

[TOC]

## 1. Basic API
- 使用`requests.<Request>()`创建response对象
	- 对应http协议中的response
	- request方法的参数将作为request的 (低优先级) 属性
- 获取response对象的属性
	- response: 简报
	- response.history: 历史
	- response.status_code: 状态码
		- `rp.status_code == requests.codes.ok`: 检查状态码
	- `response.raise_for_status()`: 抛出状态异常
	- response.url: url
	- response.headers: 请求头
		- 字典
		- 每一项为<名称>:<值>
	- response.content: 内容字节(bytes), 一般为HTML文档或图片
	- response.text: 自动编码的文档内容 (content), str
		- 通过response.encoding获取及设置编码
	- response.json(): 自动解码text中的JSON对象为Python对象
	- response.cookies: cookies字典

```python
import requests

# 创建
rp = requests.get(url = "<Url>") # get response
rp = requests.get(url = "<url>", params = {"<Name>" : "<Value>", "<Name>" : ["<Value1>", "<Value2>"]}) # get with query, 为url添加查询, 查询的值可以为列表
rp = requests.get(url = "<url>", headers = {"<name>" : "<value>"}) # set request headers
rp = requests.get(url = "<Url>", cookies = {"<Name>" : "<Value>"}) # send cookies
rp = requests.get(url = "<Url>", allow_redirects = False) # get response without redirect
rp = requests.get(url = "<Url>", timeout = <Time>) # get response with timeout (seconds)
rp = requests.post(url = "<Url>", data = {"<Name1>" : "<Value1>"[, "<Name2> : "<Value2>"]}) # post data
rp = requests.post(url = "<Url>", data = (("<Name>", "<Value>"), ("<Name>, "<Value>")) # post data with same name
rp = requests.post(url = "<url>", json = {"<Name>" : "<Value>"}) # post json
rp = requests.post(url = "<url>", file = {"<File>" : open("<FileDir>", "rb")}) # post file
rp = requests.post(url = "<url>", file = {"<File>" : ("<FileName>", open("<FileDir>", "rb"), "<FileType>", <Headers>}) # post file with metadata
rp = requests.post(url = "<url>", file = {"<File>" : ("<FileName>", <FileContent>, "<FileType>", <Headers>}) # post file with metadata
rp = requests.put(url = "<Url>", data = {"<Name>" : "<Value>"}) # put data

# 使用
rp # get brief info
rp.history # get history
rp.status_code # get status code
rp.status_code == requests.codes.ok # check status
rp.raise_for_status() # throw Exception
rp.url # get url
rp.headers # get response header dic
rp.headers["<Name>"] # get specific header value str
rp.headers("<Name>") # get specific header value str
rp.cookies["<Name>"] # get specific cookie value str
rp.text # get content of response as text
rp.encoding # get current encoding for text
rp.encoding = "<Encoding>" # set encoding for text
rp.content # get content as bytes
rp.json() # parase json in content
with open(<FileDir>, "wb") as outfile: # save byte stream chunks
    for chunk in rp.iter_content([<chunksize>]):
        outfile.write(chunk)
```
post json:
```python
import requests
import json

rp = requests.post(url = "<url>", json = {"<Name>" : "<Value>"}) # post json
rp = requests.post(url = "<url>", data = json.dumps({"<Name>" : "<Value>"})) # post json
```
send cookies:
```python
import requests

jar = requests.cookies.RequestsCookieJar()
jar.set("<Name>", "<Value>", domain = "<Domain>", path = "<Path>")
rp = request.get(url = "<url>", cookies = jar)
```
## 2. Advanced API
### 2.1. Session类
- 对应一个http协议中的一个通信 (会话)
	- 包含一对或多对request和response
- 同一个Session下的 (req, rep) 序列能跨请求保持某些参数和cookie, 重用TCP链接

```python
from requests import Session

# 创建
ses = Session()

# 修改Session的域, 这些域将与request方法参数合并, 被自动应用于每一个request
# 但request方法参数的优先级高于Session域, 因此可以在request方法参数中删除 (设为None) 与修改相应属性
ses.auth = ("<UserName>", "<PassWord>") # 设置口令
ses.headers.update({"<HeaderName>" : "<HeaderValue>"}) # 添加Header

# 使用
rep = ses.<Request>(url = "<url>") # 取得response, resp之间的某些参数和全部cookie会被保留. 调用方式与requests类的request方法相同
rep = ses.<Request>(url = "<url>", headers = {"<HeaderName>" : "<HeaderValue>"}) # 注意: Session.<Request>()与requests.<Request>()不同. Session.<Request>()中的参数会修改request的属性和行为, 会覆盖Session默认的属性值; 而requests.<Request>()中的参数不会修改request的行为, 也不会覆盖requests类默认的属性值, 仅仅只是添加在属性列表的末尾

with Session() as ss: # 将Session用作资源, 退出with块时将自动关闭Session
	ss.<Request>(url = "<url>")
```
### 2.2. Request类
- 对应http协议中的request
	- 使用response对象时在底层自动创建和发送
	- 可以手动创建, 修改和使用

```python3
from requests import Request, Session
ss = Session()

# 创建
req = rep.request # 获得response对应的request
req = Request("<Request>", url = "<url>", data = <DataDic>, headers = <DeaderDic>) # 创建request
predreq = req.prepare() # prepare request, 得到prepared request
ssPredReq = ss.prepare_request(req) # prepare request with Session, 得到附有Session默认属性的predReq

# 修改
req.headers # 取得header字典
req.headers["<HeaderName>"] # 取得对应Header项

# 使用
rep = ss.send(predreq) # 通过Session发送request, 得到对应的reponse
```
## 3. Rep Novel
- use browser's "check" to location and modify html
- extract text from html using BeautifulSoup package
- 注意: 需修改Header中的User-Agent以获得完整内容
	- 使用默认的"python-requests/2.21.0"则服务器不提供完整内容
	- 修改为与Chrome一致的"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"即可
- ATTENTION
	- There will be some "\n" string tags in the html, which is unnoticable in the "Check" view of chrome.
- Print `"<Content>\r"` again and again to refresh the console.
- Use `os.mkdir("<Dir>")` to mkdir.
- Use `os.path.isdir("<Dir>")` to check if the dir exists.
## 4. Rep Image

```python
import requests
from PIL import Image
from io import BytesIO

rp = requests.get(url = "<ImageUrl>") # rp.content is the img data
img = Image.open(BytesIO(rp.content))
outfile = open("<Dir><FileMainName>." + img.format, "w") # create out file stream
img.save(outfile) # save image
outfile.close()
```
- 图片一般存储在`<img>`标签中
- 该网页图片为JavaScript动态加载
	- 使用抓包工具Fiddler找到加载图片的JavaScript脚本, 即可找到图片链接
- 解析JSON数据
	- 获取JSON数据

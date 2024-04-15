# Python

## 爬虫

### python访问互联网

urllib模块（其实是一个包）

URL的一般格式：protocol://hostname[port]/path/[;parameters]\[?query]#fragment

- urllib是一个包，有4个模块：

  - urllib.request
  - urllib.error
  - urllib.parse
  - urllib.robotparser

  [urllib帮助文档](https://docs.python.org/3/library/urllib.html)

```python
#urllib.request.ulropen()来访问页面
import urllib.request

response = urllib.request.urlopen("http://www.fishc.com")
html = response.read()
html = html.decode("utf-8") #解码成unicode编码
print(html)
```

[一次性解决你所有的编码检测问题](https://fishc.com.cn/thread-66086-1-1.html)

### 实战

#### 下载一只猫

访问http://placekitten.com这个网站，下载猫猫图片

```python
import urllib.request

response = urllib.request.urlopen("http://placekitten.com/g/300/300")
'''urlopen的参数可以是字符串或者Request对象，如果传入字符串，py会默认把字符串转换成Request对象。因此可以这样写
req = urllib.request.Request("http://placekitten.com/g/300/300")
response = urllib.request.urlopen(req)
'''
cat_img = response.read()
with open('cat_300_300.jpg','wb') as f:
    f.write(cat_img)
```

urlopen返回的是一个类文件对象，因此可以用read()方法来读取内容。介绍三个函数：

- geturl()——返回请求的url
- info()——返回一个httplib.HTTPMessage对象，包含远程服务器返回的头信息
- getcode()——返回HTTP状态码

#### 翻译文本

学会使用审查元素 Network

urlopen函数有一个data参数，如果给这个参数赋值，那么HTTP的请求就是POST方式，默认值就是GET方式。data参数的值必须符号这个application/x-www-form-urlencoded的格式，要用urllib.parse.urlencode()将字符串转换为这个格式。

```python
#利用有道翻译文本
import urllib.request
import urllib.parse
import json

content = input("输入要翻译的内容：")

url="https://fanyi.youdao.com/translate?smartresult=dict&smartresult=rule&smartresult=ugc&sessionForm=http://www.youdao.com"
data={}
data['type'] = 'AUTO'
data['i'] = content
data['doctype'] = 'json'
data['xmlVersion'] = '1.6'
data['keyform'] = 'fanyi.web'
data['ue'] = 'UTF-8'
data['typeResult'] = 'true'

data = urllib.parse.urlencode(data).encode('utf-8')
response = urllib.request.urlopen(url,data)
html = response.read().decode('utf-8')
target = json.loads(html) #解析json格式的字符串，变成一个字典

print(target['translateResult'][0][0]['tgt'])
```

[一次性解决你所有的编码检测问题](https://fishc.com.cn/thread-66086-1-1.html)

```python
#chardet 模块用法：使用该模块的 detect() 函数即可
import urllib.request
response = urllib.request.urlopen("http://bbs.fishc.com").read()
import chardet
chardet.detect(response) 
#{'confidence': 0.99, 'encoding': 'GB2312'}
#但小甲鱼其实使用的是 GBK 编码（GBK 是 GB2312 的扩展），所以直接 decode('GB2312') 还是会报错的：
response.decode("GB2312") #报错
#两种选择
#一、忽略识别不出的字符（GB2312 支持的汉字比较少，如果用这种方法会出现小部分乱码）
response.decode("GB2312", "ignore")
#二、（推荐）由于 GBK 是向下兼容 GB2312，因此你检测到是 GB2312，则直接用 GBK 来编码/解码
if chardet.detect(response)['encoding'] == 'GB2312':
    response.decode('GBK')
```

### 隐藏

#### 修改user-agent

Request有个headers参数，设置这个参数伪造可以user-agent。通过两种途径设置：实例化Request对象的时候将headers参数传递进去和通过add_header()方法往Request对象添headers。

```python
#第一种方式要求headers必须是一个字典的形式
head = {}
head['Referer'] = 'http://fanyi.youdao.com'
head['User-Agent'] = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36 Edg/101.0.1210.39'
...
req = urllib.request.Request(url,data,head)
response = urllib.request.urlopen(req)

#第二种方法 在Request对象生成后，用add_header()方法追加
req = urllib.request.Request(url,data)
req.add_header('Referer','http://fanyi.youdao.com')
req.add_header('User-Agent','Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36 Edg/101.0.1210.39')
response = urllib.request.urlopen(req)
```

#### 延迟提交数据

time模块

```python
import time
...
time.sleep(5)
```

#### 使用代理

```python
(1)proxy_support = urllib.request.ProxyHandler({})
#参数是一个字典，字典的键是代理的类型，如http ftp https，字典的值是代理的ip和端口
(2)opener = urllib.request.build_opener(proxy_support)
(3)urllib.request.install_opener(opener)#将定制好的opener安装到系统中
#如果不想替换掉默认的opener，也可以每次需要的时候，用opener.open()打开网页

#例
import urllib.request
url = 'http://www.whatismyip.com.tw/'
proxy_support = urllib.request.ProxyHandler({'http':'46.249.98.36'})
opener = urllib.request.build_opener(proxy_support)
urllib.request.install_opener(opener)
response = urllib.request.urlopen(url)
html = response.read().decode('utf-8')
print(html)
```

### Beautiful Soup

[Beautiful Soup 4.4.0 文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/index.html)

案例一：编写一个爬虫，爬百度百科“网络爬虫”的词条（http://baike.baidu.com/view/284853.htm），将所有包含“view”的链接按格式打印出来。

案例二：要求你的爬虫允许用户输入搜索的关键词。

### 正则表达式

[Python3 如何优雅地使用正则表达式（详解一）](https://fishc.com.cn/thread-57073-1-1.html)

[Python3 如何优雅地使用正则表达式（详解二）](https://fishc.com.cn/thread-57188-1-1.html)

[Python3 如何优雅地使用正则表达式（详解三）](https://fishc.com.cn/thread-57207-1-1.html)

[Python3 如何优雅地使用正则表达式（详解四）](https://fishc.com.cn/thread-57271-1-1.html)

#### re模块

```python
import re
re.search(r'FishC','Ilove fishc.com!')
```

#### 通配符

.

#### 反斜杠

\\. 在正则表达式中，反斜杠可以剥夺元字符的特殊能力

\d 匹配数字

#### 字符类

[a-z] [0-9]

`re.search(r'[a-z]','123liane32iii1！')`

#### 重复匹配

匹配个数问题

`re.search(r'ab{3}c','abbbc')`

重复的次数也可以取一个范围

`re.search(r'ab{3,5}c','abbbbc')`

如何匹配0-255之间的数？

`re.search(r'[0-1]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5]','balabala192balabala')`

匹配ip地址？

`re.search(r'(([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])\.){3}([01]{0,1}\d{0,1}\d|2[0-4]\d|25[0-5])','192.168.80.123')`

小括号表示分组

#### 特殊符号及用法

翻书

#### 元字符

. ^ $ * + ? {} [] \ | ()

\1~99 表示引用序号对应的子组所匹配的字符串
\0xx或者\xxx 八进制数，表示这个数对应的ASCII字符

()本身就是一对元字符，被它们括起来的正则表达式称为一个子组

```python
re.search(r"(FishC)\1","FishCFishC") #\1表示引用前面序号为1的子组 相当于r"FishCFishC"

re.search(r"FishC\060","FishC0") #060对应的ASCII字符是数字0
```

[] 它生成一个字符类，它里面的内容都当成普通字符类看待，除了几个特殊的字符

```python
re.search(r"[.]","Fishc.cim")

#1. - 表示范围
re.findall(r"[a-z]","FishC.com")
#2. \ 转义
re.search(r"[\n]","Fishc\n")
#3. ^ 取反
re.findall(r"[^a-z]","FishC.com")
```

表示重复的元字符

```python
#{}
re.search(r'(fish){1,3}','fishfish')
'''
*相当于{0,}
+相当于{1,}
?相当于{0,1}
'''
```

#### 贪婪和非贪婪

默认是贪婪的匹配模式

```python
s="<html><title>abc</title></html>"
re.search('<.+>',s) #贪婪的匹配模式<re.Match object; span=(0, 31), match='<html><title>abc</title></html>'>

#启用非贪婪模式，在表示重复的元字符后面加上?即可
re.search('<.+?>',s)
```

#### \普通字母

\A 默认情况相当于^，表示匹配字符串的起始位置
\Z 默认情况相当于$，表示匹配字符串的结束位置
如果设置了re.MUTILINE标志，^和$还能匹配换行符的位置，\A \Z只能匹配字符串的起始和结束位置
零宽断言
\b \B \d \D \s \t \n \r \f \v \S \w \W

正则表达式还支持大部分py字符串的转义符号\a \b \f \n \r \t \u \U \v \x \\\

#### 编译正则表达式

`p = re.compile("[A-Z]")
p.search("I love FishC")`

#### 编译标志

ASCII,A
DOTALL,S
IGNORECASE,I
LOCALE,L
MULTILINE,M
VERBOSE,X

#### 实用的方法

模块级别的search()方法re.search()，编译后的正则表达式模式对象也有search()方法，它们的原型：
re.search(pattern,string,flags=0)
regex.search(string[, pos[, endpos]])
re.search()方法返回的是一个匹配对象，使用group()获取内容，如果存在子组 在group()中设置序号，start() end() span()分别返回匹配的开始位置、结束位置、匹配的范围

```python
result = re.search(r"(\w+)(\w+)","I love Fishc.com!")
result.group()
result.start()
result.end()
result.span()
```

findall()
在没有子组的情况下返回列表
如果正则表达式里面包含的子组，将匹配的内容组合成元组返回

获取ip时出现的问题：

```python
import urllib.request
import re
```

(?:···)

### 异常处理

#### URLError

urlopen()无法处理一个响应的时候，会引发URLError异常，伴随一个reason属性

```python
import urllib.request
req = urllib.request.Request('http://www.demo-fishc.com')
try:
    urllib.request.urlopen(req)
except urllib.error.URLError as e:
    print(e.reason)
```

#### HTTPError

HTTPError是URLError的子类

常用的HTTP状态码（略）

| **状态码** | **内容**                                                     | **详细内容**                                                 |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1xx        | 这一类型的状态码，代表请求已被接受，需要继续处理。           |                                                              |
| 100        | Continue                                                     | 收到请求，客户端应当继续发送请求。                           |
| 101        | Switching Protocols                                          | 服务器通过 Upgrade 消息头通知客户端采用不同的协议来完成这个请求。 |
|            |                                                              |                                                              |
| 2xx        | 成功 \| 这一类型的状态码，代表请求已成功被服务器接收、理解、并接受。 |                                                              |
| 200        | OK                                                           | 请求已成功，请求的响应头或数据体将随此响应返回。             |
| 201        | Created                                                      | 请求已经被实现，而且有一个新的资源已经依据请求的需要而创建，且其 URI 已经随 Location 头信息返回。 |
| 202        | Accepted                                                     | 服务器已接受请求，但尚未处理。正如它可能被拒绝一样，最终该请求可能会也可能不会被执行。 |
| 203        | Non-Authoritative Information                                | 服务器已成功处理了请求，但返回的实体头部元信息不是在原始服务器上有效的确定集合，而是来自本地或者第三方的拷贝。 |
| 204        | No Content                                                   | 服务器成功处理了请求，但没有返回任何实体内容。               |
| 205        | Reset Content                                                | 服务器成功处理了请求，且没有返回任何内容。但是与204响应不同，返回此状态码的响应要求请求者重置文档视图。 |
| 206        | Partial Content                                              | 服务器已经成功处理了部分 GET 请求。                          |
|            |                                                              |                                                              |
| 3xx        | 重定向 \| 这类状态码代表需要客户端采取进一步的操作才能完成请求。通常，这些状态码用来重定向，后续的请求地址（重定向目标）在本次响应的 Location 域中指明。 |                                                              |
| 300        | Multiple Choices                                             | 被请求的资源有一系列可供选择的回馈信息，每个都有自己特定的地址和浏览器驱动的商议信息。用户或浏览器能够自行选择一个首选的地址进行重定向。 |
| 301        | Moved Permanently                                            | 被请求的资源已永久移动到新位置，并且将来任何对此资源的引用都应该使用本响应返回的若干个 URI 之一。 |
| 302        | Found                                                        | 请求的资源现在临时从不同的 URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。 |
| 303        | See Other                                                    | 对应当前请求的响应可以在另一个 URI 上被找到，而且客户端应当采用 GET 的方式访问那个资源。 |
| 304        | Not Modified                                                 | 如果客户端发送了一个带条件的 GET 请求且该请求已被允许，而文档的内容（自上次访问以来或者根据请求的条件）并没有改变，则服务器应当返回这个状态码。 |
| 305        | Use Proxy                                                    | 被请求的资源必须通过指定的代理才能被访问。Location 域中将给出指定的代理所在的URI信息，接收者需要重复发送一个单独的请求，通过这个代理才能访问相应资源。 |
| 307        | Temporary Redirect                                           | 请求的资源现在临时从不同的 URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。 |
|            |                                                              |                                                              |
| 4xx        | 客户端错误 \| 这类的状态码代表了客户端看起来可能发生了错误，妨碍了服务器的处理。 |                                                              |
| 400        | Bad Request                                                  | 由于包含语法错误，当前请求无法被服务器理解。                 |
| 401        | Unauthorized                                                 | 当前请求需要用户验证。                                       |
| 402        | Payment Required                                             | 该状态码是为了将来可能的需求而预留的。                       |
| 403        | Forbidden                                                    | 服务器已经理解请求，但是拒绝执行它。                         |
| 404        | Not Found                                                    | 请求失败，请求的资源在服务器上找不到。                       |
| 405        | Method Not Allowed                                           | 请求中指定的请求方法不能被用于请求相应的资源。               |
| 406        | Not Acceptable                                               | 请求的资源的内容特性无法满足请求头中的条件，因而无法生成响应实体。 |
| 407        | Proxy Authentication Required                                | 与 401 状态码类似，只不过客户端必须在代理服务器上进行身份验证。 |
| 408        | Request Timeout                                              | 请求超时。客户端没有在服务器预备等待的时间内完成一个请求的发送。 |
| 409        | Conflict                                                     | 由于和被请求的资源的当前状态之间存在冲突，请求无法完成。     |
| 410        | Gone                                                         | 被请求的资源在服务器上已经不再可用，而且没有任何已知的转发地址。 |
| 411        | Length Required                                              | 服务器拒绝在没有定义 Content-Length 头的情况下接受请求。     |
| 412        | Precondition Failed                                          | 服务器在验证在请求的头字段中给出先决条件时，没能满足其中的一个或多个。 |
| 413        | Request Entity Too Large                                     | 服务器拒绝处理当前请求，因为该请求提交的实体数据大小超过了服务器愿意或者能够处理的范围。 |
| 414        | Request-URI Too Long                                         | 请求的 URI 长度超过了服务器能够解释的长度，因此服务器拒绝对该请求提供服务。 |
| 415        | Unsupported Media Type                                       | 对于当前请求的方法和所请求的资源，请求中提交的实体并不是服务器中所支持的格式，因此请求被拒绝。 |
| 416        | Requested Range Not Satisfiable                              | 如果请求中包含了 Range 请求头，并且 Range 中指定的任何数据范围都与当前资源的可用范围不重合，同时请求中又没有定义 If-Range 请求头，那么服务器就应当返回 416 状态码。 |
| 417        | Expectation Failed                                           | 在请求头 Expect 中指定的预期内容无法被服务器满足，或者这个服务器是一个代理服务器，它有明显的证据证明在当前路由的下一个节点上，Expect 的内容无法被满足。 |
|            |                                                              |                                                              |
| 5xx        | 服务器错误 \| 这类状态码代表了服务器在处理请求的过程中有错误或者异常状态发生。 |                                                              |
| 500        | Internal Server Error                                        | 服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。 |
| 501        | Not Implemented                                              | 服务器不支持当前请求所需要的某个功能。                       |
| 502        | Bad Gateway                                                  | 作为网关或者代理工作的服务器尝试执行请求时，从上游服务器接收到无效的响应。 |
| 503        | Service Unavailable                                          | 由于临时的服务器维护或者过载，服务器当前无法处理请求。       |
| 504        | Gateway Timeout                                              | 作为网关或者代理工作的服务器尝试执行请求时，未能及时从上游服务器（URI 标识出的服务器，例如 HTTP、FTP、LDAP）或者辅助服务器（例如 DNS）收到响应。 |
| 505        | HTTP Version Not Supported                                   | 服务器不支持，或者拒绝支持在请求中使用的HTTP版本。           |

```python
req = urllib.request.Request('http://www.fishc.com/demo.html')
try:
    urllib.request.urlopen(req)
except urllib.error.HTTPError as e:
    print(e.code)
    print(e.read())
```

#### 处理异常

两种方法

```python
#1
from urllib.request import Request,urlopen
from urllib.error import URLError,HTTPError

req = Request(someurl)
try:
    response = urlopen(req)
except HTTPError as e:
    print('Error code:',e.code)
except URLError as e:
    print("Reason:",e.reason)
else:
# nothing

#2 推荐
from urllib.request import Request,urlopen
from urllib.error import URLError,HTTPError

req = Request(someurl)
try:
    response = urlopen(req)
except URLError as e:
    if hasattr(e,'reason'):
        print('Reason:',e.reason)
    elif hasattr(e,'code'):
        print('Error code:',e.code)
else:
    #nothing
```

### Scrapy

不支持py3，需要py2

使用Scrapy抓取一个网站需要四个步骤：
（1）创建一个Scrapy项目
（2）定义Item容器
（3）编写爬虫
（4）存储内容

#### Scrapy框架

![scrapy](./Image/py/\scrapy.jpg)



以抓取dmoz-odp.org有关python的Books和Resources为例

#### 创建Scrapy项目

scrapy startproject tutoria

#### 定义Item容器

编辑tutoria目录下的items.py

```python
import scrapy

class DmozItem(scrapy.Item):
    title = scrapy.Field()
    link = scrapy.Field()
    desc = scrapy.Field()
```

#### 编写爬虫

编辑tutoria/spiders目录下的dmoz_spider.py

```python
import scrapy

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "https://www.dmoz-odp.org/Computers/Programming/Languages/Python/Books/",
        "https://www.dmoz-odp.org/Computers/Programming/Languages/Python/Resources/"
        ]
    
    def parse(self,response):
        filename = response.url.split("/")[-2]
        with open(filename,'wb') as f:
            f.write(response,body)
```

#### 爬

cd tutorial

scrapy crawl dmoz

#### 取

从网页内容中提取我们需要的数据

scrapy中使用一种基于XPath和CSS的表达式机制：Scrapy Selectors

Selector是一个选择器，它有四个基本方法：
（1）xpath()
（2）css()
（3）extract()
（4）re()

#### 在Shell中尝试Selector选择器

cmd项目根目录下启动Scrapy shell
scrapy shell "https://www.dmoz-odp.org/Computers/Programming/Languages/Python/Books/"
可以输入response.body、response.headers

可以使用`response.selector.xpath()`等四个基本方法。

#### 使用XPath

XPath是一门在网页中查找特定信息的语言。所以用XPath来筛选数据，要比使用正则表达式容易些。一些例子：

- /html/head/title——选择HTML文档中<head>标签内的<title>元素
- /html/head/title/text()——选择上面提到的<title>元素的文字
- //td——选择所有的<td>标签
- //div[@class="mine"]——选择所有具有class="mine"属性的div元素

response.xpath()、response.css()已经被映射到response.selector.xpath()、response.selector.css()，所有直接使用`response.xpath()`即可。

```python
response.xpath('//title') #得到一个列表
response.xpath('//title').extract() #字符串化
response.xpath('//title/text()').extract() #
response.xpath('//title/text()').re('(\w+)')
```

#### 提取数据

shell根据response的类型自动为我们初始化了变量sel，可以直接使用

`sel.xpath('//ul/li/a/text()').extract()`
`sel.xpath('//ul/li/a/@href').extract()`

如果不加extract(),xpath()直接返回一个Selector对象组成的列表。循环打印：
```python
sites = sel.xpath('//ul[@class="directort-url"]/li')
for site in sites:
	title = site.xpath('a/text()').extract()
	link = site.xpath('a/@href').extract()
	desc = site.xpath('text()').extract()
	print(title,link,desc)
```

实际使用：修改spiders/dmoz_spider.py的parse()方法

```python
def parse(self,response):
	sel = scrapy.selector.Selector(response)
    sites = sel.xpath('//ul[@class="directort-url"]/li')
	for site in sites:
		title = site.xpath('a/text()').extract()
		link = site.xpath('a/@href').extract()
		desc = site.xpath('text()').extract()
		print(title,link,desc)
```

#### 使用item

item是自定义的一个容器，用法跟python的字典一样。我们希望spiders将爬取并筛选后的数据存放到item容器中

```python
import scrapy

class DmozSpider(scrapy.Spider):
    name = "dmoz"
    allowed_domains = ["dmoz.org"]
    start_urls = [
        "https://www.dmoz-odp.org/Computers/Programming/Languages/Python/Books/",
        "https://www.dmoz-odp.org/Computers/Programming/Languages/Python/Resources/"
        ]
    
    def parse(self,response):
		sel = scrapy.selector.Selector(response)
        sites = sel.xpath('//ul[@class="directort-url"]/li')
        items = []
		for site in sites:
        	item = DmozItem()
			item['title'] = site.xpath('a/text()').extract()
			item['link'] = site.xpath('a/@href').extract()
			item['desc'] = site.xpath('text()').extract()
			items.append(item)
    	return items
```

#### 存储内容

最简单存储爬取的数据的方式是使用Feed expoerts，主要可以导出四种格式：JSON、JSON lines、CSV、XML
这里将结果导出为JSON格式

scrapy crawl dmoz -o items.json -t json	






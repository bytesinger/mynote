* python webservice 组合(soaplib, suds)
https://pypi.python.org/pypi/soaplib/2.0.0-beta2
https://pypi.python.org/pypi/suds/0.4

soaplib 和 suds 的栗子
http://soaplib.github.io/soaplib/2_0/pages/helloworld.html

但是有个问题, suds client 在不能访问外网的机器上 请求webservice timeout, pdb debug下
发现根源在于, suds 创建client对象的时候用到了w3c,soaplib的网站service
suds/xsd/sxbasic.py 末尾有如下配置
```
Import.bind(
   'http://schemas.xmlsoap.org/soap/encoding/',
   'suds://schemas.xmlsoap.org/soap/encoding/')
Import.bind(
   'http://www.w3.org/XML/1998/namespace',
   'http://www.w3.org/2001/xml.xsd')
Import.bind(
   'http://www.w3.org/2001/XMLSchema',
   'http://www.w3.org/2001/XMLSchema.xsd')
```

解决办法有三, 1开启外网访问 2 将这些xml本地化 3 走代理
前两个不用多说, 第三个suds 中urllib2 proxy 是不好使的, 所以用到了suds_requests
贴下走代理的代码
```
from suds.client import Client
import suds_requests
wsdl_url = 'http://127.0.0.1:8000/?wsdl'
proxy = {'http': 'http://hostip:hostport', 'https': 'http://hostip:hostport'}
transport = suds_requests.RequestsTransport()
my_client = Client(wsdl_url, transport=transport, proxy=proxy, cache=NoCache())
```
再贴下suds_requests
https://pypi.python.org/pypi/suds_requests/

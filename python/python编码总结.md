### python 的编码是一个坑，并且２.x 和３.x还不一样，总结一下，先看看平时代码中遇到的四种情况:
* 情况一

```
# -*- coding: utf-8 -*-
s = '我的'
print(s)
print(type(s))　#　python2.7: <type 'str'> ,python3.5:<class 'str'>
```
* 情况二

```
# -*- coding: utf-8 -*-
s = u'我的'
print(s)
print(type(s))　#　python2.7: <type 'unicode'> ,python3.5:<class 'str'>
```
* 情况三

```
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
s = '我的'
print(s)
print(type(s)) #　python2.7: <type 'unicode'> ,python3.5:<class 'str'>
```
* 情况四

```
# -*- coding: utf-8 -*-
from __future__ import unicode_literals
s = u'我的'
print(s)
print(type(s)) #　python2.7: <type 'unicode'> ,python3.5:<class 'str'>
```

总结一下：首先要明白一个问题，python3下的str 就是unicode，python2下u'我的'是unicode,'我的'.decode('utf-8')是unicode．编码的时候要使用的原则是，处理前先decode成unicode ，处理后encode成相应的编码．上面的四种情况，兼容的情况下，最好使用情况三，这种情况下（from future import unicode_literals），定义的字符串都是unicode，可以统一处理．觉得第四种情况字符串前面加u有点画蛇添足了．

# python3 时间格式转换(datetime转utc和GMT)

### pyspider下 python3 时间格式转换 (datetime转utc和GMT)


**datetime格式：**2018-07-26 17:31:32.747860

**utc格式：**2018-07-26T17:07:34.050734Z
**GMT格式：**Thu Jul 26 2018 17:07:34 GMT+0800

#### 一、首先引入detetime和timedelta
```Python
from datetime import datetime, timedelta
```
#### 二、获取当前时间
```python
now = datetime.now()
```

#### 三、timedelta计算时间，负数代表几天前，整数表示几天后
```python
 timeStr = now + timedelta(days=n)
```

#### 四、使用strftime函数处理格式
strftime第一个参数是datetime类型或者字符串类型的时间，第二个参数是想要得到的时间格式串
```python
utcTime = datetime.strftime(timeStr, '%Y-%m-%dT%H:%M:%S.%fZ')
GMT_Time = datetime.strftime(timeStr, '%a %b %d %Y %H:%M:%S GMT+0800')
```
完整代码如下，已经在pysipider上测试通过：
```python
#!/usr/bin/env python
# -*- encoding: utf-8 -*-
# Created on 2018-08-02 17:03:33
# Project: test

from pyspider.libs.base_handler import *
from datetime import datetime, timedelta


class Handler(BaseHandler):
    
    @every(minutes=24 * 60)
    def on_start(self):
        self.crawl('http://www.baidu.com', callback=self.index_page)

    @config(age=10 * 24 * 60 * 60)
    def index_page(self, response):
        # -7 表示7天前
        print(self.from_datetime_to_utc(-7)) #2018-07-26T17:07:34.050734Z
        print(self.from_datetime_to_GMT(-7)) #Thu Jul 26 2018 17:07:34 GMT+0800

    #datetime转utc
    def from_datetime_to_utc(self, n):
        now = datetime.now()
        timeStr = now + timedelta(days=n)
        utcTime = datetime.strftime(timeStr, '%Y-%m-%dT%H:%M:%S.%fZ') 
        return utcTime
    
    # datetime转GMT中国标准时间
    def from_datetime_to_GMT(self, n):
        now = datetime.now()
        timeStr = now + timedelta(days=n)
        # 中国标准时间 GMT  Thu Jun 14 2018 14:15:37 GMT+0800
        # 传参需要 Thu+Jun+14+2018+14%3A15%3A37+GMT%2B0800
        GMT_Time = datetime.strftime(timeStr, '%a %b %d %Y %H:%M:%S GMT+0800') 
        return GMT_Time
```

【后记】：
想要了解更多日期格式转换的方法或者有其他问题等去查文档吧少年。

- [附快速查文档神器链接](http://devdocs.io/)





---
title: 基于Python request刷博客访问量
date: 2019-08-10 10:46:02
tags: [Python, 脚本]
category: Python
---

# 最简单的方式
> 直接写个html文件，运行即可。只能刷静态blog的访问量。说白了就是打开个html页面无限刷新。
```html
<!-- 用iframe主要是为了避免使用window.open（）方法，一直弹窗，弹得你怀疑人生。用子窗口让他在一个网页内部自己刷自己的就成了。 -->
<!-- attr的第二个参数是刷新时间-->
<iframe src="http://hsiaoyeekwan.top" width="800" height="600"></iframe> 
<script src="http://libs.baidu.com/jquery/2.0.0/jquery.js"></script>
<script type="text/javascript">
	setInterval(function(){ console.log("刷屏"); $('iframe').attr('src', $('iframe').attr('src')); },30)
</script>
```
<!--more-->

# Python Request实现
> 首先利用正则模块爬取代理IP，保存在list中。由于是免费的，很多都不可用。之后无限循环，使用request模块访问即可。
```python
import random
import re
import requests
import time
from bs4 import BeautifulSoup
from selenium import webdriver
import sys
import os

# 随机获取浏览器标识
def get_UA():
    UA_list = [
        "Mozilla/5.0 (Linux; Android 4.1.1; Nexus 7 Build/JRO03D) AppleWebKit/535.19 (KHTML, like Gecko) Chrome/18.0.1025.166 Safari/535.19",
        "Mozilla/5.0 (Linux; U; Android 4.0.4; en-gb; GT-I9300 Build/IMM76D) AppleWebKit/534.30 (KHTML, like Gecko) Version/4.0 Mobile Safari/534.30",
        "Mozilla/5.0 (Linux; U; Android 2.2; en-gb; GT-P1000 Build/FROYO) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1",
        "Mozilla/5.0 (Windows NT 6.2; WOW64; rv:21.0) Gecko/20100101 Firefox/21.0",
        "Mozilla/5.0 (Android; Mobile; rv:14.0) Gecko/14.0 Firefox/14.0",
        "Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.94 Safari/537.36"
    ]
    randnum = random.randint(0, len(UA_list) - 1)
    h_list = UA_list[randnum]
    return h_list

def Get_proxy_ip():
    headers = {
        'Host': "www.xicidaili.com",
        'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_5) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36',
        'Accept': r'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3'
    }
    req=requests.get(r'https://www.xicidaili.com/wt/5',headers=headers)
    html=req.text
    proxy_list=[]
    IP_list=re.findall(r'\d+\.\d+\.\d+\.\d+',html)
    port_lits=re.findall(r'<td>\d+</td>',html)
    for  i in range(len(IP_list)):
        ip=IP_list[i]
        port=re.sub(r'<td>|</td>','',port_lits[i])
        proxy='%s:%s' %(ip,port)
        proxy_list.append(proxy)
    return proxy_list

if __name__ == '__main__':
    url = "http://hsiaoyeekwan.top"
    # 无限循环，每次都要打开一个浏览器窗口，
    proxy_list = Get_proxy_ip()
    pos = 0
    while 1:
        # 调用函数获取浏览器标识
        headers = get_UA()
        # 调用函数获取IP代理地址,这里获取是字符串，而不是像前两个教程获得的是数组
        pos += 1
        if pos >= len(proxy_list):
            pos -= len(proxy_list)
        proxy = proxy_list[pos]
        print(proxy)
        import requests
        try:
            result = requests.get('http://hsiaoyeekwan.top',proxies={"http": "http://"+proxy})
        except:
            print('connect failed')
        else:
            print('success')
        # 使用chrome自定义
        chrome_options = webdriver.ChromeOptions()
        # 设置代理
        chrome_options.add_argument('--proxy-server=http://' + proxy)
        # 设置UA
        chrome_options.add_argument('--user-agent="' + headers + '"')
        # 使用设置初始化webdriver
        driver = webdriver.Chrome(chrome_options=chrome_options)

        try:
            # 访问超时30秒
            driver.set_page_load_timeout(2)
            # 访问网页
            driver.get(url)
            # 退出当前浏览器
            driver.close()
            # 延迟1~3秒继续
            time_delay = random.randint(1, 3)
            while time_delay > 0:
                print(str(time_delay) + " seconds left!!")
                time.sleep(1)
                time_delay = time_delay - 1
                pass
        except:
            print("timeout")
            # 退出浏览器
            driver.quit()
            time.sleep(1)
            # 重启脚本, 之所以选择重启脚本是因为，长时间运行该脚本会出现一些莫名其妙的问题，不如重启解决
            python = sys.executable
            os.execl(python, python, *sys.argv)
        finally:
            pass
```
# 对于不蒜子-一个简单的方式
> 在测试代码的时候无意中发现的。大体搜了下原因，应该是<code>driver = webdriver.Chrome()</code>无参数时是启动的一个新用户而不是默认用户，其中端口是随机的，可能不蒜子是基于IP+端口来计数，所以无限启动的时候访客人数和访问数量都会增加。
```python
import random
import sys
import time
import selenium
import os
from selenium import webdriver

driver = webdriver.Chrome()
url = "http://hsiaoyeekwan.top"
while True:
    try:
        # 访问超时30秒
        driver.set_page_load_timeout(3)
        # 访问网页
        driver.get(url)
        # 退出当前浏览器
        driver.close()
        # 延迟1~2秒继续
        time_delay = random.randint(1, 2)
        while time_delay > 0:
            print(str(time_delay) + " seconds left!!")
            time.sleep(1)
            time_delay = time_delay - 1
            pass
    except:
        print("timeout")
        # 退出浏览器
        driver.quit()
        time.sleep(1)
        # 重启脚本, 之所以选择重启脚本是因为，长时间运行该脚本会出现一些莫名其妙的问题，不如重启解决
        python = sys.executable
        os.execl(python, python, *sys.argv)
    finally:
        pass
```
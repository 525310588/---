# 院士-爬虫
urllib.request
import urllib.request
import re
import os
import time

def url_open(url):
    req = urllib.request.Request(url)
    req.add_header('User-Agent','Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/57.0.2987.98 Safari/537.36 LBBROWSER')
    response = urllib.request.urlopen(url)
    html = response.read()
    return  html

def get_addr(url):
    html = url_open(url).decode('utf-8')
    p = r'http://www.casad.cas.cn:80/aca/371/sxwlxb-(?:\w+)-(?:\w+).html'
    addrs = re.findall(p,html)
    return addrs

def get_cont(addrs):
    for each in addrs:
        html = url_open(each).decode('utf-8')
        #print(html)
        p1 = r'(?:<p align="justify">|<p>)(.*?)<!--contentBar end-->'
        p2 = r'<img src=".*" alt="(.*?)"'
        c = re.findall(p1,html,re.S)
        text = re.sub(r'</p><p align="justify">|</?span>|&nbsp;|</?p>|</div>|<br />','',c[0]).strip()
        name = re.findall(p2, html)[0]
        with open(r'E:\python\院士.txt','a+',encoding='utf-8') as f:
            f.write(name+':'+text+'\n'+'\n')
        time.sleep(1)

def ys():
    url = 'http://www.casad.cas.cn/chnl/371/index.html'
    addrs = get_addr(url)
    get_cont(addrs)

if __name__=='__main__':
    ys()

## python爬虫入门_练习一_静态页面文本爬取_(html内中文乱码问题处理)

##### 1.前置知识

  * html一些知识
  * python基本语法
  * 简单的一些爬虫库api调用

##### 2.所用到的包

  * requests
  * bs4 import BeautifulSoup Beautiful Soup 是一个可以从HTML或XML文件中提取数据的Python库(可以理解为 一个处理文本工具吧)
  * os
  * sys  
<https://cn.python-requests.org/zh_CN/latest/>  
<https://beautifulsoup.readthedocs.io/zh_CN/latest/>

##### 3.我练习所遇到的问题

  1. 部分页面文本get下来 出现中文 乱码
  2. request(url, headers，timout) 其中headers 经常出现无效 和 timeout
  3. 文本写入文件时出现 gdk 无法写入

## 正文

这里我用来练习的网站和内容是 python 下载 <https://www.biqukan.com/1_1094/>
一念永恒这章节特别多的小说吧？(这是小说？)  
目标是 下载全部的作者发表的全部章节( ~~我选取这本小说是因为 这是我翻了半天最多的~~ )

#### 步骤1

获得一个页面返回

    
    
    user_agent = [
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36 OPR/26.0.1656.60',
    		'Opera/8.0 (Windows NT 5.1; U; en)',
    		'Mozilla/5.0 (Windows NT 5.1; U; en; rv:1.8.1) Gecko/20061208 Firefox/2.0.0 Opera 9.50',
    		'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; en) Opera 9.50',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0',
    		'Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2 ',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36',
    		'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11',
    		'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/30.0.1599.101 Safari/537.36',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.11 TaoBrowser/2.0 Safari/536.11',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER',
    		'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 732; .NET4.0C; .NET4.0E)',
    		'Mozilla/5.0 (Windows NT 5.1) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.84 Safari/535.11 SE 2.X MetaSr 1.0',
    		'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SV1; QQDownload 732; .NET4.0C; .NET4.0E; SE 2.X MetaSr 1.0)'
    ]
    
    ers = 1
    
    def get_html(url) :
        user_agent = 'Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.94 Safari/537.36'
        headers = {'User-Agent' : user_agent}
        while 1 : 
            try : 
                res = requests.get(url, headers = {'User-Agent':random.choice(user_agent)},  timeout=10)
                break
            except Exception as e:
                global ers
                print("第 ", ers, " loop ", e)
                ers = ers + 1
                continue
        print(res.encoding)  #查看网页返回的字符集类型
        print(type(res))
        return res
    

这里我用死循环 来处理偶尔出现的 无效的浏览头 timeout

#### 步骤2

分析 既然是获取全部章节 我首先看到了
这个网站对每一个小说章节直接可以在[对应书id目录](https://www.biqukan.com/1_1094/) 全部看到 连翻页都不需要…
这样就简单了 F12 查看每个章节href 位置 看看如何 find_all()  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109182716603.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
这里我们发现 只要获得 div 中class_=‘listmain’ 就完事了 之后放到soup中 find_(‘a’) 就可以获得全部的 文章名字和链接  
i.get(’*’) 获得关键字 i.string 获得它所含区域文本

    
    
    def get_list(url) :
        html = get_html(url)
        soup = BeautifulSoup(html.text)
        div_list = soup.find_all('div', class_='listmain')
        div_url = BeautifulSoup(str(div_list[0]))
        dd_list_url = div_url.find_all('a')
        url_second = []
        base_url = 'https://www.biqukan.com'
        for i in dd_list_url :
            url_second.append([i.string, base_url + i.get('href')]) 
            # print(i.string, i.get('href'))
        return url_second
    

#### 步骤3

我们get_list返回的 所有文章地址和title 此时我们考虑如何获得文章  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109184504404.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
文章内容在 div id=‘content’， class_=‘showtxt’  
页面放入soup中 直接find_all(‘div’ id=‘content’ class_=‘showtxt’)  
这里你可能看到这样的事情  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191109190257750.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)

你可能发现 我们encode进行编码 而且用的是’ISO-8859-1’ （request默认编码 可能是反爬也可能是网站就是这么写的）
这样操作中文就正常了![在这里插入图片描述](https://img-
blog.csdnimg.cn/20191109190355376.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)

    
    
    def get_text(list_url) :
        cnt = 1
        for i in list_url :
            cnt = cnt + 1
            print(i[0], i[1])
            if(os.path.exists('data/' + i[0] + '.txt')) : continue
            html = get_html(i[1])
            soup = BeautifulSoup(html.text.encode('ISO-8859-1'))
            div_text = soup.find_all('div', id='content', class_='showtxt')
            f = open('data/' + i[0] + '.txt', 'w+', encoding='utf-8')
            f.write(div_text[0].text)
            f.close()
    

如果没有encode这一步的话 你看你会看到

上面是直接request的 后面是 .encode(‘ISO-8859-1’)过的  
我看页面已经写了 是gdk编码 但是 print(res.encoding) 是 ‘ISO-8859-1’ 这里encode这个编码算是我乱试出的

##### 以下是全部代码

    
    
    #!/usr/bin/env python3
    
    import requests
    import time
    import random
    from bs4 import BeautifulSoup
    import os
    import sys
    
    
    user_agent = [
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.95 Safari/537.36 OPR/26.0.1656.60',
    		'Opera/8.0 (Windows NT 5.1; U; en)',
    		'Mozilla/5.0 (Windows NT 5.1; U; en; rv:1.8.1) Gecko/20061208 Firefox/2.0.0 Opera 9.50',
    		'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; en) Opera 9.50',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64; rv:34.0) Gecko/20100101 Firefox/34.0',
    		'Mozilla/5.0 (X11; U; Linux x86_64; zh-CN; rv:1.9.2.10) Gecko/20100922 Ubuntu/10.10 (maverick) Firefox/3.6.10',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/534.57.2 (KHTML, like Gecko) Version/5.1.7 Safari/534.57.2 ',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/39.0.2171.71 Safari/537.36',
    		'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.11 (KHTML, like Gecko) Chrome/23.0.1271.64 Safari/537.11',
    		'Mozilla/5.0 (Windows; U; Windows NT 6.1; en-US) AppleWebKit/534.16 (KHTML, like Gecko) Chrome/10.0.648.133 Safari/534.16',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/30.0.1599.101 Safari/537.36',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64; Trident/7.0; rv:11.0) like Gecko',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/536.11 (KHTML, like Gecko) Chrome/20.0.1132.11 TaoBrowser/2.0 Safari/536.11',
    		'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.1 (KHTML, like Gecko) Chrome/21.0.1180.71 Safari/537.1 LBBROWSER',
    		'Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; QQDownload 732; .NET4.0C; .NET4.0E)',
    		'Mozilla/5.0 (Windows NT 5.1) AppleWebKit/535.11 (KHTML, like Gecko) Chrome/17.0.963.84 Safari/535.11 SE 2.X MetaSr 1.0',
    		'Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 5.1; Trident/4.0; SV1; QQDownload 732; .NET4.0C; .NET4.0E; SE 2.X MetaSr 1.0)'
    ]
    
    ers = 1
    
    def get_html(url) :
        user_agent = 'Mozilla/5.0 (Windows NT 6.2; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/27.0.1453.94 Safari/537.36'
        headers = {'User-Agent' : user_agent}
        while 1 : 
            try : 
                res = requests.get(url, headers = {'User-Agent':random.choice(user_agent)},  timeout=10)
                break
            except Exception as e:
                global ers
                print("第 ", ers, " loop ", e)
                ers = ers + 1
                continue
        print(res.encoding)  #查看网页返回的字符集类型
        print(type(res))
        return res
    
    
    def get_list(url) :
        html = get_html(url)
        soup = BeautifulSoup(html.text)
        div_list = soup.find_all('div', class_='listmain')
        div_url = BeautifulSoup(str(div_list[0]))
        dd_list_url = div_url.find_all('a')
        url_second = []
        base_url = 'https://www.biqukan.com'
        for i in dd_list_url :
            url_second.append([i.string, base_url + i.get('href')]) 
            # print(i.string, i.get('href'))
        return url_second
    
    
    def get_text(list_url) :
        cnt = 1
        for i in list_url :
            cnt = cnt + 1
            print(i[0], i[1])
            sys.stdout.write("  进度:%.3f%%" %  float(cnt/len(list_url)) + '\r')
            sys.stdout.flush()
            if(os.path.exists('data/' + i[0] + '.txt')) : continue
            html = get_html(i[1])
            soup = BeautifulSoup(html.text.encode('ISO-8859-1'))
            div_text = soup.find_all('div', id='content', class_='showtxt')
            f = open('data/' + i[0] + '.txt', 'w+', encoding='utf-8')
            f.write(div_text[0].text)
            f.close()
    
    def get_t(url) :
        time.sleep(1)
        html = get_html(url)
        tmp = BeautifulSoup(html.text.encode('ISO-8859-1'))
        print(html.text)
        print(html.text.encode('ISO-8859-1'))
        div_text = tmp.find_all('div', id='content', class_='showtxt')
        print(div_text[0].text)
    
    
    if __name__ == "__main__":
        base_url = 'https://www.biqukan.com/1_1094'
        novel_list = get_list(base_url)
        get_text(novel_list)
        # get_html('https://www.biqukan.com/1_1094')
        # get_t('https://www.biqukan.com/1_1094/5447905.html')
    


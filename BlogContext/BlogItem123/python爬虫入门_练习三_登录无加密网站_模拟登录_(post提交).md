## python爬虫入门_练习三_登录无加密网站_模拟登录_(post提交)

#### 前言

作为requests.post的练习 本次就比较简单了 代码比之前的不知道少了多少23333  
这次只是选了一个登录只是简单的 post 账户名和密码 的论坛 知乎和其他网站现在都有要用打某个页面下载后某个json文件的关键值取作hash
验证是不是机器人醉了 目前还没有学会

#### 正文

这里我检验自己的方式是 模拟登录 获得 刚注册系统发来的提示信息

#### 步骤1

既然是用post 进行登录 我们显然要知道post到那个url  
这里我使用IE的Devtools 获得找到post方法对应的url ~~F12~~  
这里先自己手动登录找到post请求![在这里插入图片描述](https://img-
blog.csdnimg.cn/20191111192025936.png)

#### 步骤2

我们想要获得登录之后的数据 首先要确保自己爬虫登录后是保持链接的  
这里有  
`session = requests.Session()` 能请求保持某些参数

#### 步骤3

登录并获得系统私信代码  
登录代码

    
    
    headers = {
       'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.18362'
    }
    data = {
       'username': '',
       'password': ''
    }
    session = requests.Session()
    session.post(url='https://www.1point3acres.com/bbs/member.php?mod=logging&action=login&loginsubmit=yes&infloat=yes&lssubmit=yes&inajax=1', headers=headers, data=data)
    

获得 系统私信代码 进入消息页面 找到html对应路径就好 如果你看了前2练习 这里应该非常简单  
这里看到页面是gdk 文件写入encoding(‘gb2312’)

    
    
    res = session.get(url='https://www.1point3acres.com/bbs/home.php?mod=space&do=notice&view=system', headers=headers, timeout=5)
    print(res.encoding)
    soup = BeautifulSoup(res.text)
    f = open('html.txt', "w+", encoding='gb2312')
    f.write(res.text)
    f.close()
    
    chk = soup.find('dd', class_='ntc_body')
    f = open('ymsfd_login/chk.txt', 'w+')
    f.write(chk.text)
    f.close()
    

以下完整代码

    
    
    import time
    import requests
    from bs4 import BeautifulSoup
    
    headers = {
       'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.18362'
    }
    url = 'https://www.1point3acres.com/bbs/member.php?mod=logging&action=login&loginsubmit=yes&infloat=yes&lssubmit=yes&inajax=1'
    data = {
       'username': '',
       'password': ''
    }
    session = requests.Session()
    session.post(url='https://www.1point3acres.com/bbs/member.php?mod=logging&action=login&loginsubmit=yes&infloat=yes&lssubmit=yes&inajax=1', headers=headers, data=data)
    
    res = session.get(url='https://www.1point3acres.com/bbs/home.php?mod=space&do=notice&view=system', headers=headers, timeout=5)
    print(res.encoding)
    soup = BeautifulSoup(res.text)
    f = open('html.txt', "w+", encoding='gb2312')
    f.write(res.text)
    f.close()
    
    chk = soup.find('dd', class_='ntc_body')
    f = open('ymsfd_login/chk.txt', 'w+')
    f.write(chk.text)
    f.close()
    session.close()
    


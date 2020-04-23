## python_爬虫入门_练习四_selenium_保存登录cookie到本地反复使用

##### 本次用到的主要是 selenium chrome 模拟

~~这个真的是太慢了~~  
其实存下cookie 用 request 也是可以的

##### 本次遇到的问题

  * 发生异常: InvalidCookieDomainException  
Message: invalid cookie domain

  * InvalidCookieDomainException  
Message: invalid cookie expiry

##### 以上处理办法

    
    
    for cookies in cookie :
                   if 'expiry' in cookies:
                        del cookies['expiry']
                   if 'domain' in cookie:
                        del cookies['domain']
    

直接删除会报错元素 醉了

##### get_cookie

其实这里cookie 也可以拿 json 存  
pickle.dump 存码到本地 本质没有啥区别

    
    
    def save_cookie(self) :
              print("首先第一次登录获得cookie\n完成任意键继续")
              self._brower.get(self._url_douban)
              input()
              cookie = self._brower.get_cookies()
              print(self._brower.get_cookies())
              f = open(self._cookies_file, 'wb')
              f.write(pickle.dumps(cookie))
              f.close()
              print('over')
    

##### 完整代码

以下 代码难度不大 纯粹模拟 调之前学习的API

    
    
    import selenium.webdriver
    import pickle
    import time
    import os
    
    
    def getPureDomainCookies(cookies):
        domain2cookie={}  #做一个域到cookie的映射
        for cookie in cookies:
            domain=cookie['domain']
            if domain in domain2cookie:
                domain2cookie[domain].append(cookie)
            else:
                domain2cookie[domain]=[]
        maxCnt=0
        ansDomain=''
        for domain in domain2cookie.keys():
            cnt=len(domain2cookie[domain])
            if cnt > maxCnt:
                maxCnt=cnt
                ansDomain=domain
        ansCookies=domain2cookie[ansDomain]
        return ansCookies
    
    
    class Seledouban():
         _path_file = 'douban_cookie_login'
         _path_chromdriver = 'source/chromedriver.exe'
         _url_douban = 'https://www.zhihu.com/'
         _brower = None
         _cookies_file = _path_file + '/douban_cookie.pkl'
         _header_data = {
              'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',
              'Accept-Encoding': 'gzip, deflate, br',
              'Accept-Language': 'zh-CN,zh;q=0.9',
              'Cache-Control': 'max-age=0',
              'Connection': 'keep-alive',
              'Host': 'www.douban.com',
              'Sec-Fetch-Mode': 'navigate',
              'Sec-Fetch-Site': 'none',
              'Sec-Fetch-User': '?1',
              'Upgrade-Insecure-Requests': '1',
              'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.97 Safari/537.36'
         }
         def save_cookie(self) :
              print("首先第一次登录获得cookie\n完成任意键继续")
              self._brower.get(self._url_douban)
              input()
              cookie = self._brower.get_cookies()
              print(self._brower.get_cookies())
              f = open(self._cookies_file, 'wb')
              f.write(pickle.dumps(cookie))
              f.close()
              print('over')
    
    
         def load_cookie(self) :
              f = open(self._cookies_file, 'rb')
              cookie = pickle.load(f)
              f.close()
              for cookies in cookie :
                   if 'expiry' in cookies:
                        del cookies['expiry']
                   if 'domain' in cookie:
                        del cookies['domain']
    
              self._brower.get(self._url_douban)
    
              for i in cookie :
                   self._brower.add_cookie(i)
              print("load over")
              self._brower.get(self._url_douban)
              input()
              self._brower.refresh()
              input()
    
         def __init__(self) :
              self._brower = selenium.webdriver.Chrome(self._path_chromdriver)
              self._brower.get(self._url_douban)
              if(os.path.exists(self._cookies_file) == False) : 
                   self.save_cookie()
              else :
                   self.load_cookie()
              
    
    if __name__ == '__main__' :
         zh = Seledouban()
    


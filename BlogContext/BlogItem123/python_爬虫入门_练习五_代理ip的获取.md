## python_爬虫入门_练习五_代理ip的获取

##### 获得西祠代理

https://www.xicidaili.com/nn/1  
任务 获得 代理

##### 遇到的问题

//*[@id=“ip_list”]/tbody/tr[2]/td[2]  
chrome 浏览器 直接获得xpath 但是运行后获得是空列表

##### 处理方式

浏览器会对html文本进行一定的规范化，所以会自动在路径中加入tbody，导致读取失败，在此处直接在路径中去除tbody即可。  
~~醉了~~

    
    
    import requests
    from lxml import etree
    from requests import urllib3
    import random
    import time
    
    text = ''
    
    f = open('tmp.file', 'r')
    text = f.read()
    f.close()
    
    tree = etree.HTML(text)
    print(tree.xpath(f'//*[@id="ip_list"]/'))
    # for j in range(2, 101) :
    #     # //*[@id="ip_list"]/tbody/tr[2]/td[2]
        
    #     print(tree.xpath(f'//*[@id="ip_list"]/tbody/tr[{j}]/td[2]/text()'))
    #     print(tree.xpath(f'//table[@id="ip_list"]/tr[{j}]/td[2]/text()'))
    #     http = tree.xpath(f'//table[@id="ip_list"]/tr[{j}]/td[6]/text()')[0]
    #     host = tree.xpath(f'//*[@id="ip_list"]/tbody/tr[{j}]/td[2]/text()')
    #     port = tree.xpath(f'//table[@id="ip_list"]/tr[{j}]/td[3]/text()')[0]
    #     print(f"第 {j - 1} 个 chk {http}: {host}:{port}")
    

##### 以下完整代码

    
    
    import requests
    from lxml import etree
    from requests import urllib3
    import random
    import time
    
    
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
    
    
    def get_header_xc(urls, i) :
        headers = {
                'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
                'Accept-Encoding': 'gzip, deflate, br',
                'Accept-Language': 'zh-CN,zh;q=0.9',
                'Connection': 'keep-alive',
                'Referer': urls.format(i if i > 0 else ''),
                'User-Agent': random.choice(user_agent)
            }
        return headers
    
    
    def get_header_adnmb() :
        headers = {
                'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
                'Accept-Encoding': 'gzip, deflate, br',
                'Accept-Language': 'zh-CN,zh;q=0.9',
                'Connection': 'keep-alive',
                'User-Agent': random.choice(user_agent)
            }
        return headers
    
    
    def get_proxy():
        with open('ips_pool.csv', 'r') as f:
            datas = f.readlines()
        tmp = random.choice(datas)
        ip = tmp.strip().split(',')
        proxies = {ip[0]: ip[1] + ':' + ip[2]}
        return proxies
    
    def check_porxy(http, host, port) :
        url = 'https://adnmb2.com/Forum'
        proxy = {http: host + ':' + port}
        try :
            respond = requests.get(url, headers=get_header_adnmb(), proxies=proxy, timeout=3)
            if(respond.status_code == 200) : return True
            else : return False
        except Exception as e:
            print(e)
            return False
    
    
    def check_local_ip(fn, test_url) :
        with open(fn, 'r') as f:
            datas = f.readlines()
            ip_pools = []
        for data in datas:
            # time.sleep(1)
            ip_msg = data.strip().split(',')
            http = ip_msg[0]
            host = ip_msg[1]
            port = ip_msg[2]
            proxies = {http: host + ':' + port}
            try:
                res = requests.get(test_url, proxies=proxies, timeout=2)
                if res.status_code == 200:
                    ip_pools.append(data)
                    print(f'{proxies}检测通过')
                    with open('ips_pool.csv', 'a+') as f:
                        f.write(','.join([http, host, port]) + '\n')
            except Exception as e:
                print(e)
                continue
    
    
    def get_ip(page_sum=3000, max_try = 3) :
        s = requests.session()
        s.trust_env = None
        s.verify = None
        base_url = 'https://www.xicidaili.com/nn/{}'
        proxy = get_proxy()
        cnt = 0
        for i in range(1, page_sum + 1) :
            urls = base_url.format(i)
            s.headers = get_header_xc(urls, i - 1)
            try_cnt = 0
    
            while True :
                content = s.get(urls, headers = s.headers, proxies=proxy, timeout = random.uniform(3.0, 4.5))
                time.sleep(random.uniform(1.5, 2))
                if(content.status_code == 503) :
                    proxy = get_proxy()
                    try_cnt = try_cnt + 1
                    if(try_cnt > max_try) : 
                        print(f'{try_cnt} > max_try {max_try} 跳过')
                        break
                    print(f'第{try_cnt}改变: {proxy}')
                    continue
                else : break
    
            # print(content.text)
    
            print(f'第 {i} / {page_sum} page页')
            tree = etree.HTML(content.text)
            for j in range(2, 102) :
                http = tree.xpath(f'//table[@id="ip_list"]/tr[{j}]/td[6]/text()')[0]
                host = tree.xpath(f'//table[@id="ip_list"]/tr[{j}]/td[2]/text()')[0]
                port = tree.xpath(f'//table[@id="ip_list"]/tr[{j}]/td[3]/text()')[0]
                print(f"第 {i} 页 {j - 2} 个 chk {http}: {host}:{port}")
                if(check_porxy(http, host, port)) :
                    f = open('ips_pool.csv', 'a+')
                    f.write(','.join([http, host, port]) + '\n')
                    f.close()
                    cnt = cnt + 1
                    print(f"get 现有{cnt}")
    
    
    if __name__ == '__main__' :
        get_ip()
        # check_local_ip('ips_pool.csv', 'https://adnmb2.com/Forum')
    


## python_爬虫入门_练习六_(ajax+json)_异步加载的网站

#### ajax 滚动页面的数据获取

一般的 刚开始我们获得页面不全 而且 页面随着滚动不断加载新的  
我们考虑 找到提供数据的 js文件 ~~(幸好没有加密啊 加密还得学orz)~~  
https://www.guokr.com/scientific/

#### 步骤

https://www.guokr.com/scientific/  
滚动 尝试找到 js 然后发现其实就一个 还没有加密 很实在  
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191119160343459.png?x-oss-
process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODMxMzQw,size_16,color_FFFFFF,t_70)  
https://www.guokr.com/apis/minisite/article.json?retrieve_type=by_subject&limit=20&offset=38（&_=1574142521345
这里可以没有）  
获得当前页面 发现是一个json 给出这次20个文章数据  
offset是文章位置 每次只能limit 20 个 offset 第一页数据给出 现在有 12350 个

    
    
    {
    	"now": "2019-11-19T16:07:24.777921+08:00",
    	"ok": true,
    	"limit": 1,
    	"result": [{
    		"image": "",
    		"is_replyable": true,
    		"channels": [{
    			"url": "https://www.guokr.com/scientific/channel/frontier/",
    			"date_created": "2014-05-23T16:22:09.282129+08:00",
    			"name": "\u524d\u6cbf",
    			"key": "frontier",
    			"articles_count": 4299
    		}],
    		"channel_keys": ["frontier"],
    		"preface": "",
    		"id": 454947,
    		"subject": {
    			"url": "https://www.guokr.com/scientific/subject/engineering/",
    			"date_created": "2014-05-23T16:22:09.282129+08:00",
    			"name": "\u5de5\u7a0b",
    			"key": "engineering",
    			"articles_count": 369
    		},
    		"is_editor_recommend": false,
    		"copyright": "owned_by_guokr",
    		"author": {
    			"ukey": "7kysvk",
    			"is_title_authorized": false,
    			"nickname": "\u5706\u7684\u65b9\u5757",
    			"master_category": "normal",
    			"amended_reliability": "0",
    			"is_exists": true,
    			"title": "\u6293\u4f4f\u8868\u8fbe\u6b32",
    			"url": "https://www.guokr.com/i/0458479280/",
    			"gender": "male",
    			"followers_count": 34,
    			"avatar": {
    				"large": "https://2-im.guokr.com/3QE6l-9tPaKLDhT84toa0vV_qxTQlkUNNKyuobuofl-gAAAAoAAAAEpQ.jpg?imageView2/1/w/160/h/160",
    				"small": "https://2-im.guokr.com/3QE6l-9tPaKLDhT84toa0vV_qxTQlkUNNKyuobuofl-gAAAAoAAAAEpQ.jpg?imageView2/1/w/24/h/24",
    				"normal": "https://2-im.guokr.com/3QE6l-9tPaKLDhT84toa0vV_qxTQlkUNNKyuobuofl-gAAAAoAAAAEpQ.jpg?imageView2/1/w/48/h/48"
    			},
    			"resource_url": "https://apis.guokr.com/community/user/7kysvk.json"
    		},
    		"image_description": "",
    		"is_show_summary": false,
    		"minisite_key": null,
    		"image_info": {
    			"url": "https://2-im.guokr.com/LzCb_4lAe8TN7J1vFduyVrtGMIbNIdbRTUqp3iLGDZkCAwAAIAIAAEpQ.jpg",
    			"width": 770,
    			"height": 544
    		},
    		"subject_key": "engineering",
    		"minisite": null,
    		"tags": [],
    		"date_published": "2019-11-01T12:25:38.179681+08:00",
    		"video_content": "",
    		"authors": [{
    			"ukey": "7kysvk",
    			"is_title_authorized": false,
    			"nickname": "\u5706\u7684\u65b9\u5757",
    			"master_category": "normal",
    			"amended_reliability": "0",
    			"is_exists": true,
    			"title": "\u6293\u4f4f\u8868\u8fbe\u6b32",
    			"url": "https://www.guokr.com/i/0458479280/",
    			"gender": "male",
    			"followers_count": 34,
    			"avatar": {
    				"large": "https://2-im.guokr.com/3QE6l-9tPaKLDhT84toa0vV_qxTQlkUNNKyuobuofl-gAAAAoAAAAEpQ.jpg?imageView2/1/w/160/h/160",
    				"small": "https://2-im.guokr.com/3QE6l-9tPaKLDhT84toa0vV_qxTQlkUNNKyuobuofl-gAAAAoAAAAEpQ.jpg?imageView2/1/w/24/h/24",
    				"normal": "https://2-im.guokr.com/3QE6l-9tPaKLDhT84toa0vV_qxTQlkUNNKyuobuofl-gAAAAoAAAAEpQ.jpg?imageView2/1/w/48/h/48"
    			},
    			"resource_url": "https://apis.guokr.com/community/user/7kysvk.json"
    		}],
    		"replies_count": 7,
    		"is_author_external": false,
    		"recommends_count": 1,
    		"title_hide": "\u7eb3\u7c73\u673a\u5668\u4eba\uff0c\u771f\u6709\u4f20\u8bf4\u7684\u90a3\u4e48\u795e\u5417\uff1f",
    		"date_modified": "2019-10-30T15:25:32.881462+08:00",
    		"url": "https://www.guokr.com/article/454947/",
    		"title": "\u7eb3\u7c73\u673a\u5668\u4eba\uff0c\u771f\u6709\u4f20\u8bf4\u7684\u90a3\u4e48\u795e\u5417\uff1f",
    		"small_image": "https://2-im.guokr.com/LzCb_4lAe8TN7J1vFduyVrtGMIbNIdbRTUqp3iLGDZkCAwAAIAIAAEpQ.jpg",
    		"summary": "\u672a\u6765\u5e76\u4e0d\u9065\u8fdc\uff0c\u4f46\u4e5f\u6ca1\u90a3\u4e48\u8fd1\u3002",
    		"is_liyan_article": false,
    		"ukey_author": "7kysvk",
    		"date_created": "2019-11-01T12:25:38.179681+08:00",
    		"resource_url": "https://apis.guokr.com/minisite/article/454947.json"
    	}],
    	"offset": 0,
    	"total": 12350
    }
    
    
    
    import json
    import random
    import time
    from multiprocessing.dummy import Pool as ThreadPool
    
    import requests
    from lxml import etree
    from requests import urllib3
    
    proxies_list = []
    
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
    
    
    def get_header() :
        headers = {
                'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3',
                'Accept-Encoding': 'gzip, deflate, br',
                'Accept-Language': 'zh-CN,zh;q=0.9,en-US;q=0.8,en;q=0.7',
                'Cache-Control': 'max-age=0',
                'Connection': 'keep-alive',
                'User-Agent': 'random.choice(user_agent)'
            }
        return headers
    
    
    def load_proxy() :
        with open('ips_pool.csv', 'r') as f:
            datas = f.readlines()
        global proxies_list
        for i in datas :
            ip = i.split(',')
            proxies = {ip[0]: ip[1] + ':' + ip[2]}
            proxies_list.append(proxies)
    
    
    def save_json(add_js, page) :
        # print(f'ajax/data{page}.json')
        f = open(f'ajax/data{page}.json', 'r', encoding='utf-8')
        js = json.loads(f.read())
        f.close()
        lis = js['art']
        lis.append(add_js)
        js['art'] = lis
        # print(lis)
        f = open(f'ajax/data{page}.json', 'w', encoding='utf-8')
        f.write(json.dumps(js, ensure_ascii=False, indent=2))
        f.close()
    
    
    def get_info(js, page) :
        info = js['result']
        for i in info :
            info_dict = {}
            info_dict['id'] = i['id'] # 文章id https://www.guokr.com/article/id
            j = i['authors'][0] # https://apis.guokr.com/community/user/ ~7jsn00~.json~
            info_dict['nickname'] = j['nickname'] # authors name
            info_dict['ukey'] = j['ukey'] # authors ukey
    
            subject = i['subject'] # 文章类别
            info_dict['class'] = subject['key']
            info_dict['title'] = i['title']
            info_dict['summary'] = i['summary']
    
            save_json(info_dict, page)
    
    
    def get_json(url) :
        while 1 :
            try :
                html = requests.get(url, headers=get_header(), proxies=random.choice(proxies_list), timeout=3)
                js = json.loads(html.text)
                return js
            except Exception as e:
                print(e)
    
    
    def sol(url, page) :
        json = get_json(url)
        get_info(json, page)
    
    
    def creat_file(page) :
        f = open(f'ajax/data{page}.json', 'w+', encoding='utf-8')
        f.write('{"art":[]}')
        f.close()
    
    
    if __name__ == '__main__' :
        # save_json({'1':'2'}, 1)
        load_proxy()
        base_url = 'https://www.guokr.com/apis/minisite/article.json?retrieve_type=by_subject&limit=20&offset={}'
        offset = 0
        cnt = 9
        page = 0
        while offset < 12350 :
            cnt = cnt + 1;
            if(cnt == 10) :
                cnt = 0
                page = page + 1
                creat_file(page)
                print('第 ' + str(page) + ' * 20 条目完成/ 最后一页只有10个 ' + str(page * 20) + '/ 12350')
                
            url = base_url.format(offset)
            sol(url, page)
            offset = offset + 20
    


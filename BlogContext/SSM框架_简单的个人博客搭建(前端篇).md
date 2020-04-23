# SSM框架_简单的个人博客搭建(前端篇)

## ~~前言~~

~~我并不了解什么前端~~

~~我找了好多网站 最后选了个好看点的 套进来的~~

本文仅仅记录源码 和部分写的时候遇到的问题



## 部分我用到的小东西

marked .md转html 展示

heredoc 用注释转大段字符串 ~~2333 不会前端~~

~~用这方法也是没有谁了~~

```html
<script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>


<script>
    function heredoc(fn) {
        return fn.toString().split('\n').slice(1,-1).join('\n') + '\n'
    }

    var tmpl = heredoc(function(){/*
文本
*/});

    document.getElementById('content').innerHTML = marked(tmpl);
</script>
```

## index.jsp

界面 和 readme.jsp 是一样的 就随便改了 文本

![image-20200207153518560](../BlogImage/SSM%E6%A1%86%E6%9E%B6_%E7%AE%80%E5%8D%95%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA(%E5%89%8D%E7%AB%AF%E7%AF%87).assets/image-20200207153518560.png)



代码 一些通用部分是引入的

代码

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>ZHXU_首页</title>
    <link rel="stylesheet" href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/main.css">
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <!-- Bootstrap -->
    <link href="/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="/css/index.css">
</head>
<body>
    <jsp:include page="/WEB-INF/jsp/piclunbo.jsp" />
    <!-- 功能模块 -->
    <jsp:include page="/WEB-INF/jsp/functiondo.jsp" />
    <!-- 技术日记 -->
    <div class="container div_divider">
        <!-- 分割线 -->
        <hr class="hr_1">这里是我学习 实验 SSM 框架 搭建个人博客的练习现场 保证各种翻车加意外
        <hr class="hr_2">
        <div class="row">
            <!-- 文章列表 -->
            <div class="col-xs-9">
                <div class="list-group div_article">
                    <!-- 子头栏 -->
                    <a href="#" class="list-group-item active item_article_first">
                        <h4 class="list-group-item-heading">
                            <strong>
                                前言
                            </strong>
                        </h4>
                    </a>
                    <!-- 文章列表 -->
                    <div class="container col-xs-12" id="content"> </div>
                </div>
            </div>
            <!-- 右侧 -->
            <jsp:include page="/WEB-INF/jsp/right_bar.jsp" />
        </div>
    </div>

    <script>
        function heredoc(fn) {
            return fn.toString().split('\n').slice(1, -1).join('\n') + '\n'
        }

        var tmpl = heredoc(function () {/*
好难啊
还是去我的CSDN博客看把

https://blog.csdn.net/qq_40831340

我自己搭建的这个只是实验下我SSM 框架 学的怎么样 随便复习下自己javaweb

2333 感觉白学了
*/});
        document.getElementById('content').innerHTML = marked(tmpl);
    </script>

    <script src="/js/jquery-3.3.1.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
    <script src="/js/init.js" type="text/script"></script>
    <script src="/js/util.js"></script>
</body>
</html>
```

## 博客列表.jsp

我感觉都差不多 都是从一个页面改的 后面的博客展示 也只是

![image-20200207154049149](../BlogImage/SSM%E6%A1%86%E6%9E%B6_%E7%AE%80%E5%8D%95%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA(%E5%89%8D%E7%AB%AF%E7%AF%87).assets/image-20200207154049149.png)

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>ZHXU_首页</title>
    <link rel="stylesheet" href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/main.css">
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <!-- Bootstrap -->
    <link href="/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="/css/index.css">
</head>

<body>
    <!--图片轮播-->
    <jsp:include page="piclunbo.jsp"/>
    <!-- 功能模块 -->
    <jsp:include page="/WEB-INF/jsp/functiondo.jsp"/>
    <!-- 技术日记 -->
    <div class="container div_divider">
        <!-- 分割线 -->
        <hr class="hr_1">这里是我学习 实验 SSM 框架 搭建个人博客的练习现场 保证各种翻车加意外 !
        <hr class="hr_2">
        <div class="row">
            <!-- 文章列表 -->
            <div class="col-xs-9">
                <div class="list-group div_article">
                    <!-- 子头栏 -->
                    <a href="#" class="list-group-item active item_article_first">
                        <h4 class="list-group-item-heading">
                            博客列表
                        </h4>
                    </a>
                    <!--列表 -->
                    <c:forEach var="blog" items="${requestScope.get('list')}">
                        <div class="list-group-item item_article">
                            <div class="row">
                                <div class="div_center col-xs-12">
                                    <div class="list-group-item-heading div_article_title">
                                        <strong>
                                            <a href="${pageContext.request.contextPath}/Blogs/content/${blog.getTitle_id()}" class="title" target="_blank">${blog.getTitle_id()}</a>
                                        </strong>
                                    </div>
                                    <p class="list-group-item-text div_article_content">
                                        <div class="status" style="float:right;">post_time: ${blog.getPost_time()} | author: ZHXU98</div>
                                    </p>
                                </div>
                            </div>
                        </div>
                    </c:forEach>
                    <div>

                        <ul class="pagination">
                            <c:forEach var="p" begin="1" end="10">
                                <li> <a href="/Blogs/gotos/${p}">${p}</a> </li>
                            </c:forEach>
                        </ul>
                        
                    </div>
                </div>
            </div>
            <!-- 右侧 -->
            <jsp:include page="/WEB-INF/jsp/right_bar.jsp"/>
        </div>
    </div>

    <script src="/js/jquery-3.3.1.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
    <script src="/js/init.js" type="text/script"></script>
    <script src="/js/util.js"></script>
</body>
</html>
```



## 博客内容页面

![image-20200207160103392](../BlogImage/SSM%E6%A1%86%E6%9E%B6_%E7%AE%80%E5%8D%95%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA(%E5%89%8D%E7%AB%AF%E7%AF%87).assets/image-20200207160103392.png)

~~讲道理前端不会还是不行的~~

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html lang="zh-CN">
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <!-- 上述3个meta标签*必须*放在最前面，任何其他内容都*必须*跟随其后！ -->
    <title>ZHXU_首页</title>
    <link rel="stylesheet" href="https://cdn.bootcss.com/bootstrap/3.3.7/css/bootstrap.min.css">
    <link rel="stylesheet" href="/css/main.css">
    <script src="https://cdn.jsdelivr.net/npm/marked/marked.min.js"></script>
    <!-- Bootstrap -->
    <link href="/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" type="text/css" href="/css/index.css">
</head>
    <jsp:include page="/WEB-INF/jsp/piclunbo.jsp"/>
<!-- 功能模块 -->
    <jsp:include page="/WEB-INF/jsp/functiondo.jsp"/>
<!-- 技术日记 -->
    <div class="container div_divider">
        <!-- 分割线 -->
        <hr class="hr_1">这里是我学习 实验 SSM 框架 搭建个人博客的练习现场 保证各种翻车加意外 !
        <hr class="hr_2">
        <div class="row">
            <!-- 文章列表 -->
            <div class="col-xs-9">
                <div class="list-group div_article">
                    <!-- 子头栏 -->
                    <a href="#" class="list-group-item active item_article_first">
                        <h4 class="list-group-item-heading">
                            ${Blog.title_id} post_time: ${Blog.post_time} | author: ZHXU98
                        </h4>
                    </a>
                    <!--列表 -->
                    <div class="container col-xs-12" id="content">

                    </div>
                </div>
            </div>
            <!-- 右侧 -->
            <jsp:include page="/WEB-INF/jsp/right_bar.jsp"/>
        </div>
    </div>

    <script src="/js/jquery-3.3.1.min.js"></script>
    <script src="/js/bootstrap.min.js"></script>
    <script src="/js/init.js" type="text/script"></script>
    <script src="/js/util.js"></script>
    <script>
        function heredoc(fn) {
            return fn.toString().split('\n').slice(1,-1).join('\n') + '\n'
        }
        var tmpl = heredoc(function(){/*
${BlogContent}
        */});
        document.getElementById('content').innerHTML = marked(tmpl);
    </script>
</body>
</html>
```



## 后续

博客 .md 数据更新 图片上传 并没有写好页面 功能写好了

遇到的问题就是文件夹的上传 貌似只能用chrome 太惨了

```jsp
<form action="${pageContext.request.contextPath}/Blogs/afterAddBlog" method="post">
        名称: <input type="text" name="title_id"><br><br><br>
        time: <input type="date" name="post_time"><br><br><br>
        <input type="submit" value="添加">
    </form>
    </br>
    </br>
    </br>
    </br>
    <form action="/Blogs/upload" enctype="multipart/form-data" method="post">
        这里上传md主体文本(.md)<input type="file" name="file"/>
        <input type="submit" value=".md上传">
    </form>
    </br>
    </br>
    </br>
    </br>
    <form action="/Blogs/upload2" enctype="multipart/form-data" method="post">
        这里上传md图片文件夹(*.assets)<input type="file" name="file" multiple webkitdirectory/>
        <input type="submit" value="*.assets上传">
    </form>
```

post-time：2020-2-7
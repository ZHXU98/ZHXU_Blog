# SMM框架 简单的个人博客搭建(后端篇)

## 1, 前言

老早之前用了hexo 傻瓜式部署到git.io上 那个时候想 自己搭建一个试一试 没有想到现在回头来补这个坑了 处处都是问题 毕竟刚看完SSM框架 自己打算练习下 （没有写得太难 本身自己也不太会）
数据库 完全用不上啊 个人博客如果没有什么标签 要搞啥多表  数据库意义不大 而且数据量太小了
这里就当走个流程了

## 2，目录结构建立 各层次的编写

###  2、1 关于数据表 

~~几乎还是文件管理~~

![image-20200203153959932](../BlogImage/SMM%E6%A1%86%E6%9E%B6_%E7%AE%80%E5%8D%95%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA(%E5%90%8E%E7%AB%AF%E7%AF%87).assets/image-20200203153959932.png)

~~约等于没有~~ 之后 列表blog列表展示 虽然是是从 数据库取得 之后分页查到pagehelper插件 方便得一批23333

### 2、2 pojo层

对应数据表就行 用了lombok插件

```java
package org.zhxu.pojo;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import java.sql.Date;
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Blog {
    public String title_id;
    public Date post_time;
}
```

### 2、3 dao层

BlogMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.zhxu.dao.BlogMapper">
</mapper>
```

BlogMapper.java

```java
package org.zhxu.dao;
import org.apache.ibatis.annotations.*;
import org.zhxu.pojo.Blog;
import java.sql.Date;
import java.util.List;
// 文章
//        contextid -> titile
//        post-time
public interface BlogMapper {
    //增加一个blog
    @Insert("insert into blog (title_id, post_time) value (#{title_id}, #{post_time})")
    int addBlog(Blog blog);

    //根据id删除一个blog
    @Delete("delete from blog where title_id = #{title_id}")
    int deleteBlogById(@Param("title_id") String title_id);

    //更新blog
    @Update("update blog set title_id = #{nid}, post_time = #{npt} where title_id = #{old_id}")
    int updateBlog(@Param("nid") String nid, @Param("npt") Date ntp, @Param("old_id") String old_id);
//
    //根据id查询,返回一个blog
    @Select("select * from blog where title_id = #{title_id}")
    Blog queryBlogById(String title_id);

    //查询全部Book,返回list集合
    @Select("select * from blog")
    List<Blog> queryAllBlog();
}
```

### 2、3 service层

BlogService.java

``` java
package org.zhxu.service;
import org.zhxu.pojo.Blog;
import java.sql.Date;
import java.util.List;

public interface BlogService {
    //增加一个blog
    int addBlog(Blog blog);
//    根据id删除一个blog
    int deleteBlogById(String title_id);
//    //更新Blog
    int updateBlog(Blog blog, String old_id);
//    //根据id查询,返回一个Blog
    Blog queryBlogById(String title_id);
    //查询全部Book,返回list集合
    List<Blog> queryAllBlog();
    // 读取服务器文件 返回md文件字符串
    String BlogContext(String title_id);
}
```

BlogServiceImpl.java

```java
package org.zhxu.service;
import org.zhxu.dao.BlogMapper;
import org.zhxu.pojo.Blog;
import java.io.*;
import java.sql.Date;
import java.util.List;

public class BlogServiceImpl implements BlogService {

    private BlogMapper blogMapper;

    public void setBlogMapper(BlogMapper blogMapper) {
        this.blogMapper = blogMapper;
    }

    @Override
    public int addBlog(Blog blog) {
        return blogMapper.addBlog(blog);
    }

    @Override
    public int deleteBlogById(String title_id) {
        return blogMapper.deleteBlogById(title_id);
    }

    @Override
    public int updateBlog(Blog blog, String old_id) {
        return blogMapper.updateBlog(blog.title_id, blog.post_time, old_id);
    }

    @Override
    public Blog queryBlogById(String title_id) {
        return blogMapper.queryBlogById(title_id);
    }

    @Override
    public List<Blog> queryAllBlog() {
        return blogMapper.queryAllBlog();
    }

    @Override
    public String BlogContext(String title_id) {
        File file = new File(BlogServiceImpl.class.getClassLoader().getResource("../").getPath());
        System.out.println(file.getAbsoluteFile());
        BufferedReader br = null;
        StringBuffer sb = null;
        try {
            br = new BufferedReader(new InputStreamReader(new FileInputStream(file + "/BlogContent/" + title_id + ".md"),"utf-8")); //这里可以控制编码
            sb = new StringBuffer();
            String line = null;
            while((line = br.readLine()) != null) {
                sb.append(line + "\n");// md 格式 记得返回\n
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            try {
                br.close();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
        String data = new String(sb); //StringBuffer ==> String
        return data;
    }
}
```

### 2、4 controller层

到这里 就是和前端对接得地方了 稍微学了下 restful风格 其实蛮简单得 还方便

BlogController.java

```java
package org.zhxu.controller;

import com.github.pagehelper.PageHelper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.zhxu.pojo.Blog;
import org.zhxu.service.BlogService;

import java.util.List;

@Controller
@RequestMapping("/Blogs")
public class BlogController {

    @Autowired
    @Qualifier("BlogServiceImpl")
    private BlogService blogService;

    @RequestMapping("/showall")
    public String list(Model model) {
        List<Blog> list = blogService.queryAllBlog();
        model.addAttribute("list", list);
        return "allBlog";
    }

    @RequestMapping("/gotos/{p}")
    public String showall(Model model, @PathVariable(value = "p", required = false) int p) {
        PageHelper.startPage(p, 3);
        List<Blog> list = blogService.queryAllBlog();
        model.addAttribute("list", list);
        return "displayall";
    }

    @RequestMapping("/beforeAddBlog")
    public String beforeAddBlog() {
        return "addBlog";
    }
    @RequestMapping("/afterAddBlog")
    public String addBlog(Blog blog) {
        blogService.addBlog(blog);
        return "redirect:/Blogs/showall";
    }

    @RequestMapping("/del/{title_id}")
    public String deleteBolg(@PathVariable("title_id") String id) {
        blogService.deleteBlogById(id);
        return "redirect:/Blogs/showall";
    }

    @RequestMapping("/toUpdateBlog")
    public String toUpdateBook(Model model, String id) {
        Blog blog = blogService.queryBlogById(id);
        model.addAttribute("blog",blog);
        model.addAttribute("old_id", blog.title_id);
        System.out.println(blog);
        return "updateBlog";
    }

    @RequestMapping("/updateBlog")
    public String updateBook(Model model, Blog blog, String old_id) {
        System.out.println(blog.toString() + old_id);
        blogService.updateBlog(blog, old_id);
        return "redirect:/Blogs/showall";
    }

    @RequestMapping("/content/{Title_id}")
    public String recontent(Model model, @PathVariable(value = "Title_id", required = false) String Title_id) {
        model.addAttribute("BlogContent", blogService.BlogContext(Title_id));
        model.addAttribute("Blog", blogService.queryBlogById(Title_id));
        return "BlogContext";
    }
}
```

### 2、5目录结构

![image-20200203164017292](../BlogImage/SMM%E6%A1%86%E6%9E%B6_%E7%AE%80%E5%8D%95%E7%9A%84%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2%E6%90%AD%E5%BB%BA(%E5%90%8E%E7%AB%AF%E7%AF%87).assets/image-20200203164017292.png)



## 3 后续

​	之后便是前端得乱写 胡搞

​	在controller层 我用到了一个分页插件

​	md反馈到前端 我想了一会（其实是没有人告诉我原来数据库文本可以存很长 很长。。。 所有我就把md搞本地了 这样之后维护也方便 直接操作上传文件就行）

~~整个项目是我乱搞出来得 没有啥难度 只是刚学完 瞎写写~~

前端篇 之后发

​	



post-time: 2020-2-3
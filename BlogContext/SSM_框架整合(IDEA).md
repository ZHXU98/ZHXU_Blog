# SSM_框架整合(IDEA)

## 1. 配置文件

- applicationContext.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd">
  
      <import resource="spring-dao.xml"/>
      <import resource="spring-service.xml"/>
      <import resource="spring-mvc.xml"/>
  
  </beans>
  ```

数据库 Mysql 8.0

- database.properties

  ```properties
  jdbc.driver=com.mysql.cj.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:3306/runoob?useSSL=true&useUnicode=true&characterEncoding=utf8&serverTimezone=UTC
  jdbc.username=root
  jdbc.password=123456
  ```

- mybatis-config.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
  
      <typeAliases>
          <package name="org.zhxu.pojo"/>
      </typeAliases>
      <mappers>
          <mapper resource="org/zhxu/mapper/BookMapper.xml"/>
      </mappers>
  
  </configuration>
  ```

- spring-dao.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/context
          https://www.springframework.org/schema/context/spring-context.xsd">
      <!-- 配置整合mybatis -->
      <!-- 1.关联数据库文件 -->
      <context:property-placeholder location="classpath:database.properties"/>
  
      <!-- 2.数据库连接池 -->
      <!--数据库连接池
  dbcp  半自动化操作  不能自动连接
  c3p0  自动化操作（自动的加载配置文件 并且设置到对象里面）
      -->
      <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
          <!-- 配置连接池属性 -->
          <property name="driverClass" value="${jdbc.driver}"/>
          <property name="jdbcUrl" value="${jdbc.url}"/>
          <property name="user" value="${jdbc.username}"/>
          <property name="password" value="${jdbc.password}"/>
  
          <!-- c3p0连接池的私有属性 -->
          <property name="maxPoolSize" value="30"/>
          <property name="minPoolSize" value="10"/>
          <!-- 关闭连接后不自动commit -->
          <property name="autoCommitOnClose" value="false"/>
          <!-- 获取连接超时时间 -->
          <property name="checkoutTimeout" value="10000"/>
          <!-- 当获取连接失败重试次数 -->
          <property name="acquireRetryAttempts" value="2"/>
      </bean>
  
      <!-- 3.配置SqlSessionFactory对象 -->
      <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
          <!-- 注入数据库连接池 -->
          <property name="dataSource" ref="dataSource"/>
          <!-- 配置MyBaties全局配置文件:mybatis-config.xml -->
          <property name="configLocation" value="classpath:mybatis-config.xml"/>
      </bean>
  
      <!-- 4.配置扫描Dao接口包，动态实现Dao接口注入到spring容器中 -->
      <!--解释 ： https://www.cnblogs.com/jpfss/p/7799806.html-->
      <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
          <!-- 注入sqlSessionFactory -->
          <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
          <!-- 给出需要扫描Dao接口包 -->
          <property name="basePackage" value="org.zhxu.dao"/>
      </bean>
  
  </beans>
  ```

- spring-mvc.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xmlns:mvc="http://www.springframework.org/schema/mvc"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/mvc
      https://www.springframework.org/schema/mvc/spring-mvc.xsd">
  
      <!-- 配置SpringMVC -->
      <!-- 1.开启SpringMVC注解驱动 -->
      <mvc:annotation-driven />
      <!-- 2.静态资源默认servlet配置-->
      <mvc:default-servlet-handler/>
  
      <!-- 3.配置jsp 显示ViewResolver视图解析器 -->
      <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
          <property name="viewClass" value="org.springframework.web.servlet.view.JstlView" />
          <property name="prefix" value="/WEB-INF/jsp/" />
          <property name="suffix" value=".jsp" />
      </bean>
  
      <!-- 4.扫描web相关的bean -->
      <context:component-scan base-package="org.zhxu.controller" />
  
  </beans>
  ```

- spring-service.xml

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans
      http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/context
      http://www.springframework.org/schema/context/spring-context.xsd">
  
      <!-- 扫描service相关的bean -->
      <context:component-scan base-package="org.zhxu.service" />
  
      <!--BookServiceImpl注入到IOC容器中-->
      <bean id="BookServiceImpl" class="org.zhxu.service.BookServiceImpl">
          <property name="bookMapper" ref="bookMapper"/>
      </bean>
  
      <!-- 配置事务管理器 -->
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <!-- 注入数据库连接池 -->
          <property name="dataSource" ref="dataSource" />
      </bean>
  
  </beans>
  ```

#### 注意点

![image-20200201130647601](../BlogImage/SSM_%E6%A1%86%E6%9E%B6%E6%95%B4%E5%90%88(IDEA).assets/image-20200201130647601-1580634399468.png)

在Project Structure 中 Artifacts 里面 自己手动导入 lib 依赖 不然tomcat 关于bean 注解 页面404

其次是 静态资源问题 不要忘了在MVC层配置

```xml
<!-- 2.静态资源默认servlet配置-->
    <mvc:default-servlet-handler/>
    <mvc:resources location="/WEB-INF/css/" mapping="/css/**" />
    <mvc:resources location="/WEB-INF/BlogImage/" mapping="/Blogs/BlogImage/**" />
    <mvc:resources location="/WEB-INF/BlogContent/" mapping="/BlogContext/**" />
    <mvc:resources location="/WEB-INF/fonts/" mapping="/fonts/**" />
    <mvc:resources location="/WEB-INF/js/" mapping="/js/**" />
    <mvc:resources location="/WEB-INF/img/" mapping="/img/**" />
```



post-time: 2020-1-31


# SpringCloud

+++

## 一、课程介绍及版本选择

课程链接：[尚硅谷SpringCloud框架开发教程(SpringCloudAlibaba微服务分布式架构丨Spring Cloud)](https://www.bilibili.com/video/BV18E411x7eT?p=8&vd_source=6e6b2286ee9a603d7bdb2bc5ba80e449)

> Springcloud和Springboot之间的依赖关系如何看：

1. 查询：[Springcloud和Springboot之间的依赖关系](https://spring.io/projects/spring-cloud#overview)

   ![image-20221222180806380](SpringCloud-尚硅谷.assets/image-20221222180806380.png)

2. 更详细的版本对应查看方法

   https://start.spring.io/actuator/info

   查看json串返回结果

   ![image-20221222181021209](SpringCloud-尚硅谷.assets/image-20221222181021209.png)

3. 同时用boot和cloud，需要照顾cloud，由cloud决定boot版本

   ![image-20221222181536760](SpringCloud-尚硅谷.assets/image-20221222181536760.png)

   ![image-20221222181538805](SpringCloud-尚硅谷.assets/image-20221222181538805.png)



> 本次SpringCloud课程定稿

1. SpringCloud：Hoxton.SR1
2. SpringBoot：2.2.2.RELEASE
3. SpringCloud Alibaba：2.1.0.RELEASE
4. Java：JDK8
5. Maven：3.8.1
6. MySQL：5.7



+++

## 二、Cloud各种组件的停更/升级/替换

> 由停更引发的“升级惨案”

- 停更不停用

  1. 被动修复bugs
  2. 不再接受合并请求
  3. 不再发布新版本

- 明细条目

  1. 2020年以前

     ![image-20221222181934248](SpringCloud-尚硅谷.assets/image-20221222181934248.png)

  2. 截止至2020年上半年

     ![image-20221222181955259](SpringCloud-尚硅谷.assets/image-20221222181955259.png)

> 参考资料见官网

- Spring Cloud
  - https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/
  - Spring Cloud中文文档：https://www.bookstack.cn/read/spring-cloud-docs/docs-index.md
- Spring Boot
  - https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/

+++

## 三、微服务架构编码构建

### 1 约定>配置>编码



### 2 IDEA新建project工作空间

#### 2.1 微服务cloud整体聚合父工程Project

父工程步骤：

1. New Project

   ![image-20221222183123821](SpringCloud-尚硅谷.assets/image-20221222183123821.png)

2. Maven选版本

   ![image-20221222190047669](SpringCloud-尚硅谷.assets/image-20221222190047669.png)

3. 字符编码

   ![image-20221222190205127](SpringCloud-尚硅谷.assets/image-20221222190205127.png)

4. 注解生效激活

   ![image-20221222190258020](SpringCloud-尚硅谷.assets/image-20221222190258020.png)

5. java编译版本选8

   ![image-20221222190414767](SpringCloud-尚硅谷.assets/image-20221222190414767.png)

#### 2.2 父工程POM

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.shanhai.springcloud</groupId>
  <artifactId>sh-cloud</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>pom</packaging>

  <modules>
    <module>cloud-provider-payment8001</module>
  </modules>

  <!-- 统一管理jar包版本 -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
    <junit.version>4.12</junit.version>
    <log4j.version>1.2.17</log4j.version>
    <lombok.version>1.16.18</lombok.version>
    <mysql.version>5.1.47</mysql.version>
    <druid.version>1.1.16</druid.version>
    <mybatis.spring.boot.version>1.3.0</mybatis.spring.boot.version>
  </properties>

  <!-- 子模块继承之后，提供作用：锁定版本+子modlue不用写groupId和version  -->
  <dependencyManagement>
    <dependencies>
      <!--spring boot 2.2.2-->
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-dependencies</artifactId>
        <version>2.2.2.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud Hoxton.SR1-->
      <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-dependencies</artifactId>
        <version>Hoxton.SR1</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <!--spring cloud alibaba 2.1.0.RELEASE-->
      <dependency>
        <groupId>com.alibaba.cloud</groupId>
        <artifactId>spring-cloud-alibaba-dependencies</artifactId>
        <version>2.1.0.RELEASE</version>
        <type>pom</type>
        <scope>import</scope>
      </dependency>
      <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>${mysql.version}</version>
      </dependency>
      <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>${druid.version}</version>
      </dependency>
      <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>${mybatis.spring.boot.version}</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
      </dependency>
      <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>${log4j.version}</version>
      </dependency>
      <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
        <optional>true</optional>
      </dependency>
    </dependencies>
  </dependencyManagement>

  <build>
    <plugins>
      <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <configuration>
          <fork>true</fork>
          <addResources>true</addResources>
        </configuration>
        <version>2.2.2.RELEASE</version>
      </plugin>
    </plugins>
  </build>

</project>
```



#### 2.3 Maven工程复习

- Maven中的`DependencyManagement`和`Dependencies`

  dependencyManagement：Maven 使用 dependencyManagement 元素来提供了一种管理依赖版本号的方式。*通常会在一个组织或者项目的最顶层的父POM 中看到 dependencyManagement 元素*。

  使用 pom.xml 中的 dependencyManagement 元素能让所有在子项目中引用一个依赖而不用显式的列出版本号。

  Maven 会沿着父子层次向上走，直到找到一个拥有 dependencyManagement 元素的项目，然后它就会使用这个 dependencyManagement 元素中指定的版本号。

  ![image-20221222191337738](SpringCloud-尚硅谷.assets/image-20221222191337738.png)

  这样做的好处就是：如果有多个子项目都引用同一样依赖，则可以避免在每个使用的子项目里都声明一个版本号，这样当想升级或切换到另一个版本时，只需要在顶层父容器里更新，而不需要一个一个子项目的修改；另外如果某个子项目需要另外的一个版本，只需要声明version就可。

  1. dependencyManagement里只是声明依赖，并不实现引入，因此子项目需要显示的声明需要用的依赖。
  2. 如果不在子项目中声明依赖，是不会从父项目中继承下来的；只有在子项目中写了该依赖项，并且没有指定具体版本，才会从父项目中继承该项，并且version和scope都读取自父pom；
  3. 如果子项目中指定了版本号，那么会使用子项目中指定的jar版本。

- maven中跳过单元测试

  1. 配置

     ```xml
     <build><!-- maven中跳过单元测试 -->
         <plugins>
             <plugin>
                 <groupId>org.apache.maven.plugins</groupId>
                 <artifactId>maven-surefire-plugin</artifactId>
                 <configuration>
                     <skip>true</skip>
                 </configuration>
             </plugin>
         </plugins>
     </build>
     ```

  2. IDEA工具支持(推荐)

     ![image-20221222191642936](SpringCloud-尚硅谷.assets/image-20221222191642936.png)

> 父工程创建完成执行mvn:install将父工程发布到仓库方便子工程继承



### 3 Rest微服务工程构建

#### 3.1 微服务提供者支付Module模块

微服务提供者支付Module模块：`cloud-provider-payment8001`

```
1 建module
2 改POM
3 写YML
4 主启动类
5 业务类
6 测试
```



##### 3.1.1 建立module模块

1. 建Maven子工程：cloud-provider-payment8001

   创建完成后请回到父工程查看pom文件变化

   ![image-20221222195506348](SpringCloud-尚硅谷.assets/image-20221222195506348.png)

2. 改POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-provider-payment8001</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid-spring-boot-starter</artifactId>
               <version>1.1.10</version>
           </dependency>
           <!--mysql-connector-java-->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <!--jdbc-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-jdbc</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   
   </project>
   ```

3. 写YML：application.yml

   ```yaml
   server:
     port: 8001
   
   spring:
     application:
       name: cloud-payment-service
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
       driver-class-name: com.mysql.jdbc.Driver              # mysql驱动包 com.mysql.jdbc.Driver
       url: jdbc:mysql://localhost:3306/cloud_test?useUnicode=true&characterEncoding=utf-8&useSSL=false
       username: root
       password: 123456
   
   mybatis:
     mapperLocations: classpath:mapper/*.xml
     type-aliases-package: com.shanhai.springcloud.entities    # 所有Entity别名类所在包
   ```

4. 主启动类

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 20:02
    */
   @SpringBootApplication
   public class PaymentMain8001 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8001.class, args);
       }
   }
   ```

至此子模块项目构建完成，下面就是业务类的代码编写。



##### 3.1.2 业务类

1. 建表SQL

   ```sql
   CREATE TABLE `payment` (
     `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'ID',
     `serial` varchar(200) DEFAULT '',
     PRIMARY KEY (`id`)
   ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8
   ```

2. entities

   主实体Payment

   ```java
   package com.shanhai.springcloud.entities;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   import java.io.Serializable;
   
   /**
    * @description: 主实体Payment
    * @author: xu
    * @date: 2022/12/22 20:10
    */
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Payment implements Serializable {
       private static final long serialVersionUID = 1L;
   
       private Long id;
       private String serial;
   }
   ```

   Json封装体CommonResult

   ```java
   package com.shanhai.springcloud.entities;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   /**
    * @description: Json封装体CommonResult
    * @author: xu
    * @date: 2022/12/22 20:13
    */
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class CommonResult<T> {
       private Integer code;
       private String  message;
       private T data;
   
       public CommonResult(Integer code, String message) {
           this(code, message,null);
       }
   }
   ```

3. dao/mapper

   接口PaymentDao

   ```java
   package com.shanhai.springcloud.dao;
   
   import com.shanhai.springcloud.entities.Payment;
   import org.apache.ibatis.annotations.Mapper;
   import org.apache.ibatis.annotations.Param;
   
   /**
    * @description: 接口PaymentDao
    * @author: xu
    * @date: 2022/12/22 20:16
    */
   @Mapper
   public interface PaymentDao {
       int create(Payment payment);
   
       Payment getPaymentById(@Param("id") Long id);
   }
   ```

   mybaits的映射文件 src\main\resources\mapper\PaymentMapper.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
   <mapper namespace="com.shanhai.springcloud.dao.PaymentDao">
   
       <insert id="create" parameterType="Payment" useGeneratedKeys="true" keyProperty="id" >
           insert into payment(serial) values (#{serial})
       </insert>
   
       <resultMap id="BaseResultMap" type="com.shanhai.springcloud.entities.Payment" >
           <id column="id" property="id" jdbcType="BIGINT" />
           <id column="serial" property="serial" jdbcType="VARCHAR" />
       </resultMap>
   
       <select id="getPaymentById" parameterType="long" resultMap="BaseResultMap">
           select * from payment where id=#{id}
       </select>
   </mapper>
   ```

4. service

   接口PaymentService

   ```java
   package com.shanhai.springcloud.service;
   
   import com.shanhai.springcloud.entities.Payment;
   import org.apache.ibatis.annotations.Param;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 20:27
    */
   public interface PaymentService {
       int create(Payment payment);
   
       Payment getPaymentById(@Param("id") Long id);
   }
   ```

   实现类PaymentServiceImpl

   ```java
   package com.shanhai.springcloud.service.impl;
   
   import com.shanhai.springcloud.dao.PaymentDao;
   import com.shanhai.springcloud.entities.Payment;
   import com.shanhai.springcloud.service.PaymentService;
   import org.springframework.stereotype.Service;
   
   import javax.annotation.Resource;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 20:28
    */
   @Service
   public class PaymentServiceImpl implements PaymentService {
       @Resource
       private PaymentDao paymentDao;
   
       @Override
       public int create(Payment payment) {
           return paymentDao.create(payment);
       }
   
       @Override
       public Payment getPaymentById(Long id) {
           return paymentDao.getPaymentById(id);
       }
   }
   ```

5. controller

   ```java
   package com.shanhai.springcloud.controller;
   
   import com.shanhai.springcloud.entities.CommonResult;
   import com.shanhai.springcloud.entities.Payment;
   import com.shanhai.springcloud.service.PaymentService;
   import lombok.extern.slf4j.Slf4j;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.PostMapping;
   import org.springframework.web.bind.annotation.RestController;
   
   import javax.annotation.Resource;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 20:30
    */
   @RestController
   @Slf4j
   public class PaymentController {
       @Resource
       private PaymentService paymentService;
   
       @PostMapping(value = "/payment/create")
       public CommonResult<Integer> create(@RequestBody Payment payment) {
           int result = paymentService.create(payment);
           log.info("插入结果：" + result);
           if (result > 0) {
               return new CommonResult(200, "插入数据库成功", result);
           } else {
               return new CommonResult(444, "插入数据库失败", null);
           }
       }
   
       @GetMapping(value = "/payment/get/{id}")
       public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
           Payment payment = paymentService.getPaymentById(id);
           log.info("结果：" + payment);
           if (payment != null) {
               return new CommonResult(200, "查询成功", payment);
           } else {
               return new CommonResult(444, "没有对应记录，查询失败，查询ID：" + id, null);
           }
       }
   }
   ```



##### 3.1.3 测试

> `http://localhost:8001/payment/get/31`
>
> `http://localhost:8001/payment/create`

![image-20221222210804655](SpringCloud-尚硅谷.assets/image-20221222210804655.png)

![image-20221222210806479](SpringCloud-尚硅谷.assets/image-20221222210806479.png)



##### 3.1.4 热部署Devtools

1. Adding devtools to your project

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-devtools</artifactId>
       <scope>runtime</scope>
       <optional>true</optional>
   </dependency>
   ```

2. Adding plugin to your parent pom.xml

   下段配置我们粘贴进聚合父类总工程的pom.xml里

   ```xml
   <build>
     <plugins>
       <plugin>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-maven-plugin</artifactId>
         <configuration>
           <fork>true</fork>
           <addResources>true</addResources>
         </configuration>
         <version>2.2.2.RELEASE</version>
       </plugin>
     </plugins>
   </build>
   ```

3. Enabling automatic build

   ![image-20221222212842525](SpringCloud-尚硅谷.assets/image-20221222212842525.png)

4. Update the value of

   2021.2版本之前

   ![image-20221222212907819](SpringCloud-尚硅谷.assets/image-20221222212907819.png)

   ![image-20221222214822835](SpringCloud-尚硅谷.assets/image-20221222214822835.png)

   2021.2版本之后

   ![image-20221222214934864](SpringCloud-尚硅谷.assets/image-20221222214934864.png)

5. 重启IDEA



#### 3.2 微服务消费者订单Module模块

微服务消费者订单Module模块：`cloud-consumer-order80`



##### 3.2.1 建立module模块

1. 建Maven子工程：cloud-consumer-order80

   创建完成后请回到父工程查看pom文件变化

   ![image-20221222215702158](SpringCloud-尚硅谷.assets/image-20221222215702158.png)

2. 改POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-consumer-order80</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   
   </project>
   ```

3. 写YML：application.yml

   ```yaml
   server:
     port: 80
   ```

4. 主启动类

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 21:59
    */
   @SpringBootApplication
   public class MainApp80 {
       public static void main(String[] args) {
           SpringApplication.run(MainApp80.class,args);
       }
   }
   ```



##### 3.2.2 业务类

1. entities

   主实体Payment

   ```java
   package com.shanhai.springcloud.entities;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   import java.io.Serializable;
   
   /**
    * @description: 主实体Payment
    * @author: xu
    * @date: 2022/12/22 20:10
    */
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class Payment implements Serializable {
       private static final long serialVersionUID = 1L;
   
       private Long id;
       private String serial;
   }
   ```

   Json封装体CommonResult

   ```java
   package com.shanhai.springcloud.entities;
   
   import lombok.AllArgsConstructor;
   import lombok.Data;
   import lombok.NoArgsConstructor;
   
   /**
    * @description: Json封装体CommonResult
    * @author: xu
    * @date: 2022/12/22 20:13
    */
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class CommonResult<T> {
       private Integer code;
       private String  message;
       private T data;
   
       public CommonResult(Integer code, String message) {
           this(code, message,null);
       }
   }
   ```

2. RestTemplate

   > 是什么：RestTemplate提供了多种便捷访问远程Http服务的方法，是一种简单便捷的访问restful服务模板类，是Spring提供的用于访问Rest服务的**客户端模板工具集**。
   >
   > 
   >
   > 官网地址：https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html
   >
   > 使用：
   >
   > 使用restTemplate访问restful接口非常的简单粗暴无脑。
   >
   > (url, requestMap, ResponseBean.class)这三个参数分别代表
   >
   > REST请求地址、请求参数、HTTP响应转换被转换成的对象类型。

3. config配置类

   ```java
   package com.shanhai.springcloud.config;
   
   import org.springframework.context.annotation.Bean;
   import org.springframework.context.annotation.Configuration;
   import org.springframework.web.client.RestTemplate;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 22:08
    */
   @Configuration
   public class ApplicationContextConfig {
       @Bean
       public RestTemplate getRestTemplate() {
           return new RestTemplate();
       }
   }
   ```

4. controller

   ```java
   package com.shanhai.springcloud.controller;
   
   import com.shanhai.springcloud.entities.CommonResult;
   import com.shanhai.springcloud.entities.Payment;
   import lombok.extern.slf4j.Slf4j;
   import org.springframework.web.bind.annotation.GetMapping;
   import org.springframework.web.bind.annotation.PathVariable;
   import org.springframework.web.bind.annotation.RestController;
   import org.springframework.web.client.RestTemplate;
   
   import javax.annotation.Resource;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/22 22:04
    */
   @RestController
   @Slf4j
   public class OrderController {
       private static final String PAYMENT_URL = "http://localhost:8001";
   
       @Resource
       private RestTemplate restTemplate;
   
       @GetMapping("/consumer/payment/create")
       public CommonResult<Integer> create(Payment payment) {
           log.info("OrderController.create");
           return restTemplate.postForObject(PAYMENT_URL +  "/payment/create", payment, CommonResult.class);
       }
   
       @GetMapping("/consumer/payment/get/{id}")
       public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
           log.info("OrderController.getPaymentById");
           return restTemplate.getForObject(PAYMENT_URL +  "/payment/get/"+ id, CommonResult.class);
       }
   }
   ```



##### 3.2.3 测试

> `http://localhost:80/consumer/payment/get/32`
>
> `http://localhost:80/consumer/payment/create?serial=atguigu003`

![image-20221223101509765](SpringCloud-尚硅谷.assets/image-20221223101509765.png)

![image-20221223101511771](SpringCloud-尚硅谷.assets/image-20221223101511771.png)



#### 3.3 工程重构

> 观察问题：系统中有重复部分，重构

![image-20221222230452239](SpringCloud-尚硅谷.assets/image-20221222230452239.png)

> 建立`cloud-api-commons`模块

1. 新建Maven工程：cloud-api-commons

2. 改POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-api-commons</artifactId>
   
       <dependencies>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>cn.hutool</groupId>
               <artifactId>hutool-all</artifactId>
               <version>5.1.0</version>
           </dependency>
       </dependencies>
   
   </project>
   ```

3. 将`支付模块`和`订单模块`的`公共实体类`拷贝到当前模块下

4. maven命令

   clean ---> install

   ![image-20221222231256625](SpringCloud-尚硅谷.assets/image-20221222231256625.png)

5. 订单模块和支付模块分别改造

   删除各自的原先有过的entities文件夹

   各自添加依赖

   ```xml
   <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
   	<groupId>com.shanhai.springcloud</groupId>
   	<artifactId>cloud-api-commons</artifactId>
   	<version>${project.version}</version>
   </dependency>
   ```



> 目前工程样图

![image-20221222231919826](SpringCloud-尚硅谷.assets/image-20221222231919826.png)

+++

## 四、Eureka服务注册与发现

### 1 Eureka基础知识

> 什么是服务治理？

SpringCloud 封装了 Netflix 公司开发的 Eureka 模块来**实现服务治理**。

在传统的rpc远程调用框架中，管理每个服务与服务之间依赖关系比较复杂，管理比较复杂，所以需要使用服务治理，管理服务于服务之间依赖关系，可以实现服务调用、负载均衡、容错等，实现服务发现与注册。

> 什么是服务注册与发现？

Eureka采用了CS的设计架构，Eureka Server作为服务注册功能的服务器，它是服务注册中心。而系统中的其他微服务，使用Eureka的客户端连接到Eureka Server并维持心跳连接。这样系统的维护人员就可以通过Eureka Server来监控系统中各个微服务是否正常运行。

在服务注册与发现中，有一个注册中心。当服务器启动的时候，会把当前自己服务器的信息 比如 服务地址通讯地址等以别名方式注册到注册中心上。另一方（消费者|服务提供者），以该别名的方式去注册中心上获取到实际的服务通讯地址，然后再实现本地RPC调用RPC远程调用框架核心设计思想：在于注册中心，因为使用注册中心管理每个服务与服务之间的一个依赖关系(服务治理概念)。在任何rpc远程框架中，都会有一个注册中心(存放服务地址相关信息(接口地址))。

> Eureka两组件：<font color='gree'>Eureka Server</font>和<font color="gree">Eureka Client</font>。

1. *Eureka Server*提供服务注册服务

   各个微服务节点通过配置启动后，会在Eureka Server中进行注册，这样Eureka Server中的服务注册表中将会存储所有可用服务节点的信息，服务节点的信息可以在界面中直观看到。

2. *Eureka Client*通过注册中心进行访问

   是一个Java客户端，用于简化Eureka Server的交互，客户端同时也具备一个内置的、使用轮询(round-robin)负载算法的负载均衡器。在应用启动后，将会向Eureka Server发送心跳(默认周期为30秒)。如果Eureka Server在多个心跳周期内没有接收到某个节点的心跳，EurekaServer将会从服务注册表中把这个服务节点移除（默认90秒）



### 2 单机Eureka构建步骤

#### 2.1 Eureka Server端服务注册中心

IDEA生成eurekaServer端服务注册中心：类似物业公司

1. 建Maven子工程：cloud-eureka-server7001

2. 改POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-eureka-server7001</artifactId>
   
       <dependencies>
           <!--eureka-server-->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
           </dependency>
           <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
           <dependency>
               <groupId>com.shanhai.springcloud</groupId>
               <artifactId>cloud-api-commons</artifactId>
               <version>1.0-SNAPSHOT</version>
           </dependency>
           <!--boot web actuator-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--一般通用配置-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
           </dependency>
       </dependencies>
   </project>
   ```

3. 写YML：application.yml

   ```yaml
   server:
     port: 7001
   
   eureka:
     instance:
       hostname: localhost # eureka服务端的实例名称
     client:
       # false表示不向注册中心注册自己。
       register-with-eureka: false
       # false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
       fetch-registry: false
       service-url:
         #设置与Eureka Server交互的地址查询服务和注册服务都需要依赖这个地址。
         defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
   ```

4. 主启动类

   注：加`@EnableEurekaServer`注解

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/23 9:28
    */
   @EnableEurekaServer
   @SpringBootApplication
   public class EurekaMain7001 {
       public static void main(String[] args) {
           SpringApplication.run(EurekaMain7001.class, args);
       }
   }
   ```

5. 测试

   访问：`http://localhost:7001/`

   No application available 没有服务被发现 O(∩_∩)O

   因为没有注册服务进来当然不可能有服务被发现

   ![image-20221223093829079](SpringCloud-尚硅谷.assets/image-20221223093829079.png)



#### 2.2 Eureka Client端

> Eureka Client端将 cloud-provider-payment8001 注册进Eureka Server成为服务提供者provider
>
> 类似尚硅谷学校对外提供授课服务

*修改cloud-provider-payment8001模块*：

1. 改POM

   添加依赖

   ```xml
   <!--eureka-client-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
   </dependency>
   ```

2. 修改YML文件：application.yml

   ```yml
   server:
     port: 8001
   
   spring:
     application:
       name: cloud-payment-service
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource
       driver-class-name: com.mysql.jdbc.Driver
       url: jdbc:mysql://192.168.88.100:3306/cloud_test?useUnicode=true&characterEncoding=utf-8&useSSL=false
       username: root
       password: 123456
   
   mybatis:
     mapperLocations: classpath:mapper/*.xml
     type-aliases-package: com.shanhai.springcloud.entities
   
   eureka:
     client:
       # 表示是否将自己注册进EurekaServer默认为true。
       register-with-eureka: true
       # 是否从EurekaServer抓取已有的注册信息，默认为true。
       # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
       fetchRegistry: true
       service-url:
         defaultZone: http://localhost:7001/eureka
   ```

3. 主启动类：加`@EnableEurekaClient`注解

   ```java
   @EnableEurekaClient
   @SpringBootApplication
   public class PaymentMain8001 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8001.class, args);
       }
   }
   ```

4. 测试

   **先要启动EurekaServer**，访问`http://localhost:7001/`：

   ![image-20221223095727126](SpringCloud-尚硅谷.assets/image-20221223095727126.png)

   微服务注册名配置说明

   ![image-20221223095757417](SpringCloud-尚硅谷.assets/image-20221223095757417.png)



> Eureka Client端 cloud-consumer-order80 将注册进Eureka Server成为服务消费者consumer
>
> 类似来尚硅谷上课消费的各位同学

*修改cloud-consumer-order80模块*：

1. 改POM

   添加依赖

   ```xml
   <!--eureka-client-->
   <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
   </dependency>
   ```

2. 修改YML文件：application.yml

   ```yaml
   server:
     port: 80
   
   spring:
     application:
       name: cloud-order-service
   
   eureka:
     client:
       # 表示是否将自己注册进EurekaServer默认为true。
       register-with-eureka: true
       # 是否从EurekaServer抓取已有的注册信息，默认为true。
       # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
       fetchRegistry: true
       service-url:
         defaultZone: http://localhost:7001/eureka
   ```

3. 主启动类：加`@EnableEurekaClient`注解

   ```java
   @EnableEurekaClient
   @SpringBootApplication
   public class MainApp80 {
       public static void main(String[] args) {
           SpringApplication.run(MainApp80.class,args);
       }
   }
   ```

4. 测试

   **先要启动EurekaServer，7001服务**；**再要启动服务提供者provider，8001服务**；

   访问`http://localhost:7001/`：

   ![image-20221223100818522](SpringCloud-尚硅谷.assets/image-20221223100818522.png)



### 3 集群Eureka构建步骤

#### 3.1 Eureka集群原理说明

![image-20221223114508506](SpringCloud-尚硅谷.assets/image-20221223114508506.png)

1. 先启动Eureka注册中心
2. 启动服务提供者payment支付服务
3. 支付服务启动后会把自身信息(比如服务地址以别名方式)注册进Eureka
4. 消费者order服务在需要调用接口时，使用服务别名去注册中心获取实际的RPC远程调用地址
5. 消费者获得调用地址后，底层实际是利用HttpClient技术实现远程调用
6. 消费者获得服务地址后会缓存在本地JVM内存中，默认每间隔30s更新一次服务调用地址

> 问题：微服务RPC远程服务调用最核心的是什么？

高可用，试想你的注册中心只有一个only one，它出故障了那就呵呵了，会导致整个为服务环境不可用

解决办法：搭建Eureka注册中心集群 ，实现负载均衡+故障容错



#### 3.2 Eureka Server集群环境构建步骤

参考*cloud-eureka-server7001*：

1. 新建module：cloud-eureka-server7002

2. 改POM：和cloud-eureka-server7001模块的POM文件一模一样

3. 修改映射配置

   找到C:\Windows\System32\drivers\etc路径下的hosts文件

   修改映射配置添加进hosts文件

   ```
   127.0.0.1  eureka7001.com
   127.0.0.1  eureka7002.com
   ```

4. 写YML

   cloud-eureka-server7001

   ```yaml
   server:
     port: 7001
   
   eureka:
     instance:
       hostname: eureka7001.com #eureka服务端的实例名称
     client:
       register-with-eureka: false     #false表示不向注册中心注册自己。
       fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
       service-url:
         # defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
         defaultZone: http://eureka7002.com:7002/eureka/
   ```

   cloud-eureka-server7002

   ```yaml
   server:
     port: 7002
   
   eureka:
     instance:
       hostname: eureka7002.com #eureka服务端的实例名称
     client:
       register-with-eureka: false     #false表示不向注册中心注册自己。
       fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/
   ```

5. 主启动类

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.server.EnableEurekaServer;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/23 11:11
    */
   @EnableEurekaServer
   @SpringBootApplication
   public class EurekaMain7002 {
       public static void main(String[] args) {
           SpringApplication.run(EurekaMain7002.class, args);
       }
   }
   ```

6. 测试：相互注册，相互守望

   分别访问：`http://eureka7001.com:7001/`和`http://eureka7002.com:7002/`。

   ![image-20221223120056588](SpringCloud-尚硅谷.assets/image-20221223120056588.png)

   ![image-20221223120058325](SpringCloud-尚硅谷.assets/image-20221223120058325.png)



#### 3.3 将微服务发布到Eureka集群配置中

> 将支付服务8001微服务发布到上面2台Eureka集群配置中

> 将订单服务80微服务发布到上面2台Eureka集群配置中

只需要分别修改两个服务的YML文件即可：

- cloud-provider-payment8001

  ```yaml
  server:
    port: 8001
  
  spring:
    application:
      name: cloud-payment-service
    datasource:
      type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
      driver-class-name: com.mysql.jdbc.Driver              # mysql驱动包 com.mysql.jdbc.Driver
      url: jdbc:mysql://192.168.88.100:3306/cloud_test?useUnicode=true&characterEncoding=utf-8&useSSL=false
      username: root
      password: 123456
  
  mybatis:
    mapperLocations: classpath:mapper/*.xml
    type-aliases-package: com.shanhai.springcloud.entities    # 所有Entity别名类所在包
  
  eureka:
    client:
      # 表示是否将自己注册进EurekaServer默认为true。
      register-with-eureka: true
      # 是否从EurekaServer抓取已有的注册信息，默认为true。
      # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
      fetchRegistry: true
      service-url:
        # defaultZone: http://localhost:7001/eureka
        # 集群版
        defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  ```

- cloud-consumer-order80

  ```yaml
  server:
    port: 80
  
  spring:
    application:
      name: cloud-order-service
  
  eureka:
    client:
      # 表示是否将自己注册进EurekaServer默认为true。
      register-with-eureka: true
      # 是否从EurekaServer抓取已有的注册信息，默认为true。
      # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
      fetchRegistry: true
      service-url:
        # defaultZone: http://localhost:7001/eureka
        # 集群版
        defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  ```

> 测试

*先要启动EurekaServer，7001/7002服务*；*再要启动服务提供者provider，8001服务*；*再要启动消费者，80服务*。

分别访问：`http://eureka7001.com:7001/`和`http://eureka7002.com:7002/`。

![image-20221223121056890](SpringCloud-尚硅谷.assets/image-20221223121056890.png)

![image-20221223121059075](SpringCloud-尚硅谷.assets/image-20221223121059075.png)

`http://localhost:80/consumer/payment/get/32`：OK

![image-20221223121223780](SpringCloud-尚硅谷.assets/image-20221223121223780.png)



#### 3.4 支付服务提供者集群环境构建

参考cloud-provider-payment8001

1. 新建module：cloud-provider-payment8002

2. 改POM：依赖环境和cloud-provider-payment8001模块的POM文件一模一样

3. 写YML：和cloud-provider-payment8001的YML文件基本一致(只修改服务端口即可)

   ```yaml
   server:
     port: 8002
   
   spring:
     application:
       name: cloud-payment-service
     datasource:
       type: com.alibaba.druid.pool.DruidDataSource            # 当前数据源操作类型
       driver-class-name: com.mysql.jdbc.Driver              # mysql驱动包 com.mysql.jdbc.Driver
       url: jdbc:mysql://192.168.88.100:3306/cloud_test?useUnicode=true&characterEncoding=utf-8&useSSL=false
       username: root
       password: 123456
   
   mybatis:
     mapperLocations: classpath:mapper/*.xml
     type-aliases-package: com.shanhai.springcloud.entities    # 所有Entity别名类所在包
   
   eureka:
     client:
       # 表示是否将自己注册进EurekaServer默认为true。
       register-with-eureka: true
       # 是否从EurekaServer抓取已有的注册信息，默认为true。
       # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
       fetchRegistry: true
       service-url:
         # defaultZone: http://localhost:7001/eureka
         # 集群版
         defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
   ```

4. 主启动类

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/23 12:19
    */
   @SpringBootApplication
   @EnableEurekaClient
   public class PaymentMain8002 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8002.class, args);
       }
   }
   ```

5. 业务类：直接从8001粘

6. 修改8001/8002的Controller

   cloud-provider-payment8001

   ```java
   @RestController
   @Slf4j
   public class PaymentController {
       @Resource
       private PaymentService paymentService;
   
       @Value("${server.port}")
       private String serverPort;
   
       @PostMapping(value = "/payment/create")
       public CommonResult<Integer> create(@RequestBody Payment payment) {
           int result = paymentService.create(payment);
           log.info("插入结果：" + result);
           if (result > 0) {
               return new CommonResult(200, "插入数据库成功，serverPort" + serverPort, result);
           } else {
               return new CommonResult(444, "插入数据库失败", null);
           }
       }
   
       @GetMapping(value = "/payment/get/{id}")
       public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
           Payment payment = paymentService.getPaymentById(id);
           log.info("结果：" + payment);
           if (payment != null) {
               return new CommonResult(200, "查询成功，serverPort" + serverPort, payment);
           } else {
               return new CommonResult(444, "没有对应记录，查询失败，查询ID：" + id, null);
           }
       }
   }
   ```

   cloud-provider-payment8002

   ```java
   @RestController
   @Slf4j
   public class PaymentController {
       @Resource
       private PaymentService paymentService;
   
       @Value("${server.port}")
       private String serverPort;
   
       @PostMapping(value = "/payment/create")
       public CommonResult<Integer> create(@RequestBody Payment payment) {
           int result = paymentService.create(payment);
           log.info("插入结果：" + result);
           if (result > 0) {
               return new CommonResult(200, "插入数据库成功，serverPort" + serverPort, result);
           } else {
               return new CommonResult(444, "插入数据库失败", null);
           }
       }
   
       @GetMapping(value = "/payment/get/{id}")
       public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
           Payment payment = paymentService.getPaymentById(id);
           log.info("结果：" + payment);
           if (payment != null) {
               return new CommonResult(200, "查询成功，serverPort" + serverPort, payment);
           } else {
               return new CommonResult(444, "没有对应记录，查询失败，查询ID：" + id, null);
           }
       }
   }
   ```

7. 测试

   分别访问：`http://eureka7001.com:7001/`和`http://eureka7002.com:7002/`。

   ![image-20221223123041297](SpringCloud-尚硅谷.assets/image-20221223123041297.png)

   ![image-20221223123043708](SpringCloud-尚硅谷.assets/image-20221223123043708.png)



#### 3.5 负载均衡

> BUG：80订单服务访问地址不能写死

cloud-consumer-order80：订单模块的Controller类

```java
package com.shanhai.springcloud.controller;

import com.shanhai.springcloud.entities.CommonResult;
import com.shanhai.springcloud.entities.Payment;
import lombok.extern.slf4j.Slf4j;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;
import org.springframework.web.client.RestTemplate;

import javax.annotation.Resource;

/**
 * @description:
 * @author: xu
 * @date: 2022/12/22 22:04
 */
@RestController
@Slf4j
public class OrderController {
    //private static final String PAYMENT_URL = "http://localhost:8001";
    private static final String PAYMENT_URL = "http://CLOUD-PAYMENT-SERVICE";

    @Resource
    private RestTemplate restTemplate;

    @GetMapping("/consumer/payment/create")
    public CommonResult<Integer> create(Payment payment) {
        log.info("OrderController.create");
        return restTemplate.postForObject(PAYMENT_URL +  "/payment/create", payment, CommonResult.class);
    }

    @GetMapping("/consumer/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
        log.info("OrderController.getPaymentById");
        return restTemplate.getForObject(PAYMENT_URL +  "/payment/get/"+ id, CommonResult.class);
    }
}
```



> 使用`@LoadBalanced`注解赋予`RestTemplate`负载均衡的能力

cloud-consumer-order80：订单模块的config配置类：ApplicationContextConfig

```java
package com.shanhai.springcloud.config;

import org.springframework.cloud.client.loadbalancer.LoadBalanced;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.client.RestTemplate;

/**
 * @description:
 * @author: xu
 * @date: 2022/12/22 22:08
 */
@Configuration
public class ApplicationContextConfig {
    @Bean
    @LoadBalanced
    public RestTemplate getRestTemplate() {
        return new RestTemplate();
    }
}
```



> 测试

*先要启动EurekaServer，7001/7002服务*；*再要启动服务提供者provider，8001/8002服务*；*再要启动消费者，80服务*。

多次访问：`http://localhost/consumer/payment/get/31`

结果：负载均衡效果达到，8001/8002端口交替出现

Ribbon和Eureka整合后Consumer可以直接调用服务而不用再关心地址和端口号，且该服务还有负载功能了。O(∩_∩)O

![image-20221223124500406](SpringCloud-尚硅谷.assets/image-20221223124500406.png)

![image-20221223124138937](SpringCloud-尚硅谷.assets/image-20221223124138937.png)



### 4 actuator微服务信息完善

> 主机名称:服务名称修改

当前问题：含有主机名称

![image-20221223125643948](SpringCloud-尚硅谷.assets/image-20221223125643948.png)

分别修改`cloud-provider-payment8001`和修改`cloud-provider-payment8002`的YML文件。

修改部分：

```yaml
eureka:
  client:
    # 表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    # 是否从EurekaServer抓取已有的注册信息，默认为true。
    # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      # defaultZone: http://localhost:7001/eureka
      # 集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  instance:
    instance-id: payment8001 # payment8002
```

修改之后：

![image-20221223125954392](SpringCloud-尚硅谷.assets/image-20221223125954392.png)



> 访问信息有IP信息提示

当前问题：没有IP提示

![image-20221223130225029](SpringCloud-尚硅谷.assets/image-20221223130225029.png)

分别修改`cloud-provider-payment8001`和修改`cloud-provider-payment8002`的YML文件。

修改部分：

```yaml
eureka:
  client:
    # 表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    # 是否从EurekaServer抓取已有的注册信息，默认为true。
    # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  instance:
    instance-id: payment8001 # payment8002
    prefer-ip-address: true
```

修改之后：

![image-20221223130521184](SpringCloud-尚硅谷.assets/image-20221223130521184.png)



### 5 服务发现Discovery

对于注册进eureka里面的微服务，可以通过服务发现来获得该服务的信息

修改`cloud-provider-payment8001`的`Controller`：

```java
package com.shanhai.springcloud.controller;

import com.shanhai.springcloud.entities.CommonResult;
import com.shanhai.springcloud.entities.Payment;
import com.shanhai.springcloud.service.PaymentService;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.cloud.client.ServiceInstance;
import org.springframework.cloud.client.discovery.DiscoveryClient;
import org.springframework.web.bind.annotation.*;

import javax.annotation.Resource;
import java.util.List;

/**
 * @description:
 * @author: xu
 * @date: 2022/12/22 20:30
 */
@RestController
@Slf4j
public class PaymentController {
    @Resource
    private PaymentService paymentService;

    @Value("${server.port}")
    private String serverPort;

    @Resource
    private DiscoveryClient discoveryClient;

    @PostMapping(value = "/payment/create")
    public CommonResult<Integer> create(@RequestBody Payment payment) {
        int result = paymentService.create(payment);
        log.info("插入结果：" + result);
        if (result > 0) {
            return new CommonResult(200, "插入数据库成功，serverPort" + serverPort, result);
        } else {
            return new CommonResult(444, "插入数据库失败", null);
        }
    }

    @GetMapping(value = "/payment/get/{id}")
    public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
        Payment payment = paymentService.getPaymentById(id);
        log.info("结果：" + payment);
        if (payment != null) {
            return new CommonResult(200, "查询成功，serverPort" + serverPort, payment);
        } else {
            return new CommonResult(444, "没有对应记录，查询失败，查询ID：" + id, null);
        }
    }

    @GetMapping(value = "/payment/discovery")
    public Object discovery() {
        List<String> services = discoveryClient.getServices();
        for (String service : services) {
            log.info("service：" + service);
        }

        List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
        for (ServiceInstance instance : instances) {
            log.info(instance.getServiceId() + "\t" + instance.getHost() + "\t" +
                    instance.getPort() + "\t" + instance.getUri());
        }
        return discoveryClient;
    }
}
```

8001主启动类：添加`@EnableDiscoveryClient`注解

```java
package com.shanhai.springcloud;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.eureka.EnableEurekaClient;

/**
 * @description: 主启动类
 * @author: xu
 * @date: 2022/12/22 20:02
 */
@EnableDiscoveryClient
@EnableEurekaClient
@SpringBootApplication
public class PaymentMain8001 {
    public static void main(String[] args) {
        SpringApplication.run(PaymentMain8001.class, args);
    }
}
```

测试：

先要启动EurekaServer，再启动8001主启动类

访问`http://localhost:8001/payment/discovery`。

![image-20221223134139784](SpringCloud-尚硅谷.assets/image-20221223134139784.png)

![image-20221223134223541](SpringCloud-尚硅谷.assets/image-20221223134223541.png)

> 修改`cloud-provider-payment8002`的`Controller`和其主启动类的操作和以上操作一样。



### 6 Eureka自我保护

#### 6.1 概述

> 故障现象

概述：保护模式主要用于一组客户端和Eureka Server之间存在网络分区场景下的保护。一旦进入保护模式，Eureka Server将会尝试保护其服务注册表中的信息，不再删除服务注册表中的数据，也就是不会注销任何微服务。

如果在Eureka Server的首页看到以下这段提示，则说明Eureka进入了保护模式：

`EMERGENCY! EUREKA MAY BE INCORRECTLY CLAIMING INSTANCES ARE UP WHEN THEY'RE NOT. 
RENEWALS ARE LESSER THAN THRESHOLD AND HENCE THE INSTANCES ARE NOT BEING EXPIRED JUST TO BE SAFE `

![image-20221223134655179](SpringCloud-尚硅谷.assets/image-20221223134655179.png)



> 导致原因

*为什么会产生Eureka自我保护机制*？

为了防止EurekaClient可以正常运行，但是 与 EurekaServer网络不通情况下，EurekaServer不会立刻将EurekaClient服务剔除。

*什么是自我保护模式*？

默认情况下，如果EurekaServer在一定时间内没有接收到某个微服务实例的心跳，EurekaServer将会注销该实例（默认90秒）。但是当网络分区故障发生(延时、卡顿、拥挤)时，微服务与EurekaServer之间无法正常通信，以上行为可能变得非常危险了——因为微服务本身其实是健康的，**此时本不应该注销这个微服务**。Eureka通过“自我保护模式”来解决这个问题——当EurekaServer节点在短时间内丢失过多客户端时（可能发生了网络分区故障），那么这个节点就会进入自我保护模式。

![image-20221223135734608](SpringCloud-尚硅谷.assets/image-20221223135734608.png)

**在自我保护模式中，Eureka Server会保护服务注册表中的信息，不再注销任何服务实例**。

它的设计哲学就是宁可保留错误的服务注册信息，也不盲目注销任何可能健康的服务实例。*一句话讲解：好死不如赖活着*。

综上，自我保护模式是一种应对网络异常的安全保护措施。它的架构哲学是宁可同时保留所有微服务（健康的微服务和不健康的微服务都会保留）也不盲目注销任何健康的微服务。使用自我保护模式，可以让Eureka集群更加的健壮、稳定。



一句话：某时刻某一个微服务不可用了，Eureka不会立刻清理，依旧会对该微服务的信息进行保存。

属于CAP里面的AP分支



#### 6.2 怎么禁止自我保护

> 注册中心eureakeServer端7001
>
> 注册中心eureakeServer端7002

出厂默认，自我保护机制是开启的：`eureka.server.enable-self-preservation=true`

使用`eureka.server.enable-self-preservation=false`可以禁用自我保护模式

修改 7001 和 7002 两个模块的YML文件即可

```yaml
server:
  port: 7001 #7002

eureka:
  instance:
    hostname: eureka7001.com #eureka服务端的实例名称
  client:
    register-with-eureka: false     #false表示不向注册中心注册自己。
    fetch-registry: false     #false表示自己端就是注册中心，我的职责就是维护服务实例，并不需要去检索服务
    service-url:
      # defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka/
      defaultZone: http://eureka7002.com:7002/eureka/ # http://eureka7001.com:7001/eureka/
  server:
    #关闭自我保护机制，保证不可用服务被及时踢除
    enable-self-preservation: false
    eviction-interval-timer-in-ms: 2000
```

关闭效果：

![image-20221223140749586](SpringCloud-尚硅谷.assets/image-20221223140749586.png)



> 生产者客户端eureakeClient端8001
>
> 生产者客户端eureakeClient端8002

默认配置：

- eureka.instance.lease-renewal-interval-in-seconds=30 (单位为秒(默认是30秒))
- eureka.instance.lease-expiration-duration-in-seconds=90 (单位为秒(默认是90秒))

修改 8001 和 8002 两个模块的YML文件即可

```yaml
eureka:
  client:
    # 表示是否将自己注册进EurekaServer默认为true。
    register-with-eureka: true
    # 是否从EurekaServer抓取已有的注册信息，默认为true。
    # 单节点无所谓，集群必须设置为true才能配合ribbon使用负载均衡
    fetchRegistry: true
    service-url:
      # defaultZone: http://localhost:7001/eureka
      # 集群版
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
  instance:
    instance-id: payment8001
    prefer-ip-address: true
    # Eureka客户端向服务端发送心跳的时间间隔，单位为秒(默认是30秒)
    lease-renewal-interval-in-seconds: 1
    # Eureka服务端在收到最后一次心跳后等待时间上限，单位为秒(默认是90秒)，超时将剔除服务
    lease-expiration-duration-in-seconds: 2
```



> 测试

7001、7002和8001、8002都配置完成

先启动7001、7002再启动8001、8002

![image-20221223141809909](SpringCloud-尚硅谷.assets/image-20221223141809909.png)

先关闭8001：马上被删除了

![image-20221223141947567](SpringCloud-尚硅谷.assets/image-20221223141947567.png)

+++

## 五、Zookeeper服务注册与发现

> 问题：Eureka停止更新了你怎么办？
>
> https://github.com/Netflix/eureka/wiki

![image-20221223142706086](SpringCloud-尚硅谷.assets/image-20221223142706086.png)

答案之一：*SpringCloud整合Zookeeper代替Eureka*。



下面就就进行SpringCloud整合Zookeeper的操作：

### 1 注册中心Zookeeper

- zookeeper是一个分布式协调工具，可以实现注册中心功能
- 关闭Linux服务器防火墙后启动zookeeper服务器
- zookeeper服务器取代Eureka服务器，zk作为服务注册中心



### 2 服务提供者

1. 新建Maven子模块：cloud-provider-payment8004

2. POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-provider-payment8004</artifactId>
       <dependencies>
           <!-- SpringBoot整合Web组件 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency><!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
               <groupId>com.shanhai.springcloud</groupId>
               <artifactId>cloud-api-commons</artifactId>
               <version>${project.version}</version>
           </dependency>
           <!-- SpringBoot整合zookeeper客户端 -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   </project>
   ```

3. YML：application.yml

   ```yaml
   #8004表示注册到zookeeper服务器的支付服务提供者端口号
   server:
     port: 8004
   #服务别名----注册zookeeper到注册中心名称
   spring:
     application:
       name: cloud-provider-zk-payment
     cloud:
       zookeeper:
         connect-string: 192.168.88.100:2181
   ```

4. 主启动类：添加`@EnableDiscoveryClient`注解

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/23 15:05
    */
   //该注解用于向使用consul或者zookeeper作为注册中心时注册服务
   @EnableDiscoveryClient
   @SpringBootApplication
   public class PaymentMain8004 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8004.class, args);
       }
   }
   ```

5. Controller

   ```java
   package com.shanhai.springcloud.controller;
   
   import org.springframework.beans.factory.annotation.Value;
   import org.springframework.web.bind.annotation.RestController;
   
   import java.util.UUID;
   
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/23 15:07
    */
   @RestController
   public class PaymentController {
       @Value("${server.port}")
       private String serverPort;
   
       public String paymentZk(){
           return "SpringCloud With Zookeeper : " + serverPort + "\t"
                   + UUID.randomUUID().toString();
       }
   }
   ```

6. 启动8004注册进zookeeper

   启动后可能出现的问题：

   ![image-20221223151949080](SpringCloud-尚硅谷.assets/image-20221223151949080.png)

   why？

   ![image-20221223152025104](SpringCloud-尚硅谷.assets/image-20221223152025104.png)

   ![image-20221223152035396](SpringCloud-尚硅谷.assets/image-20221223152035396.png)

   解决zookeeper版本jar包冲突问题

   ```xml
   <!-- SpringBoot整合zookeeper客户端 -->
   <dependency>
   	<groupId>org.springframework.cloud</groupId>
   	<artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
   	<!--先排除自带的zookeeper3.5.3-->
   	<exclusions>
   		<exclusion>
       		<groupId>org.apache.zookeeper</groupId>
       		<artifactId>zookeeper</artifactId>
   		</exclusion>
   	</exclusions>
   </dependency>
   <!--添加zookeeper3.4.9版本-->
   <dependency>
       <groupId>org.apache.zookeeper</groupId>
       <artifactId>zookeeper</artifactId>
       <version>3.4.9</version>
   </dependency>
   ```

7. 验证测试：`http://localhost:8004/payment/zk`

   ![image-20221223152724385](SpringCloud-尚硅谷.assets/image-20221223152724385.png)

   ![image-20221223152935575](SpringCloud-尚硅谷.assets/image-20221223152935575.png)

8. 验证测试2：获得json串后用在线工具查看试试

   ![image-20221223153312003](SpringCloud-尚硅谷.assets/image-20221223153312003.png)

   ![image-20221223153642436](SpringCloud-尚硅谷.assets/image-20221223153642436.png)

9. 思考：服务节点是临时节点还是持久节点？

   答：临时节点

   ![image-20221223154010454](SpringCloud-尚硅谷.assets/image-20221223154010454.png)



### 3 服务消费者

1. 新建Maven子模块：cloud-consumerzk-order80

2. POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-consumerzk-order80</artifactId>
       <dependencies>
           <!-- SpringBoot整合Web组件 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <!-- SpringBoot整合zookeeper客户端 -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-zookeeper-discovery</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   </project>
   ```

3. YML：application.yml

   ```yaml
   server:
     port: 80
   
   spring:
     application:
       name: cloud-consumer-zk-order
     cloud:
       #注册到zookeeper地址
       zookeeper:
         connect-string: 192.168.88.100:2181
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   public class OrderZK80 {
       public static void main(String[] args) {
           SpringApplication.run(OrderZK80.class, args);
       }
   }
   ```

5. 业务类

   配置Bean：RestTemplate

   ```java
   @Configuration
   public class ApplicationContextConfig {
       @Bean
       @LoadBalanced
       public RestTemplate getRestTemplate() {
           return new RestTemplate();
       }
   }
   ```

   Controller

   ```java
   @RestController
   public class OrderZKController {
       public static final String INVOKE_URL = "http://cloud-provider-zk-payment";
   
       @Resource
       private RestTemplate restTemplate;
   
       @RequestMapping(value = "/consumer/payment/zk")
       public String paymentInfo() {
           String result = restTemplate.getForObject(INVOKE_URL+"/payment/zk", String.class);
           System.out.println("消费者调用支付服务(zookeeper)--->result:" + result);
           return result;
       }
   }
   ```

6. 验证测试

   ![image-20221223155756135](SpringCloud-尚硅谷.assets/image-20221223155756135.png)

   访问`http://localhost/consumer/payment/zk`：

   ![image-20221223155903173](SpringCloud-尚硅谷.assets/image-20221223155903173.png)

+++

## 六、Consul服务注册与发现

### 1 Consul简介

官网：https://www.consul.io/intro/index.html



#### 1.1 Consul是什么？

![image-20221224004756366](SpringCloud-尚硅谷.assets/image-20221224004756366.png)

Consul 是一套开源的分布式服务发现和配置管理系统，由 HashiCorp 公司**用 Go 语言开发**。

提供了微服务系统中的*服务治理*、*配置中心*、*控制总线*等功能。这些功能中的每一个都可以根据需要单独使用，也可以一起使用以构建全方位的服务网格，总之Consul提供了一种完整的服务网格解决方案。

它具有很多优点。包括：基于 raft 协议，比较简洁；支持健康检查，同时支持 HTTP 和 DNS 协议，支持跨数据中心的 WAN 集群，提供图形界面跨平台，支持Linux、Mac、Windows。



#### 1.2 Consul能干嘛？

Spring Cloud Consul 具有如下特性：

![image-20221224005126542](SpringCloud-尚硅谷.assets/image-20221224005126542.png)

1. 服务发现：提供HTTP和DNS两种发现方式。
2. 健康监测：支持多种方式，HTTP、TCP、Docker、Shell脚本定制化监控
3. KV存储：Key、Value的存储方式
4. 多数据中心：Consul支持多数据中心
5. 可视化Web界面



#### 1.3 下载与使用

去哪下：https://www.consul.io/downloads.html

怎么玩：https://www.springcloud.cc/spring-cloud-consul.html



### 2 安装并运行Consul

官网安装说明：https://learn.hashicorp.com/consul/getting-started/install.html

下载完成后只有一个consul.exe文件，硬盘路径下cmd运行，查看版本号信息

![image-20221224005956809](SpringCloud-尚硅谷.assets/image-20221224005956809.png)

![image-20221224010113005](SpringCloud-尚硅谷.assets/image-20221224010113005.png)

使用开发模式启动：`consul agent -dev`

通过以下地址可以访问Consul的首页：`http://localhost:8500`

结果页面：

![image-20221224010508045](SpringCloud-尚硅谷.assets/image-20221224010508045.png)



### 3 服务提供者

1. 新建Module支付服务provider8006：`cloud-providerconsul-payment8006`

2. POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-providerconsul-payment8006</artifactId>
   
       <dependencies>
           <!--SpringCloud consul-server -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-consul-discovery</artifactId>
           </dependency>
           <!-- SpringBoot整合Web组件 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--日常通用jar包配置-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   </project>
   ```

3. YML：application.yml

   ```yaml
   ###consul服务端口号
   server:
     port: 8006
   
   spring:
     application:
       name: consul-provider-payment
     ####consul注册中心地址
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           #hostname: 127.0.0.1
           service-name: ${spring.application.name}
   ```

4. 主启动类

   ```java
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/24 1:20
    */
   @SpringBootApplication
   @EnableDiscoveryClient
   public class PaymentMain8006 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentMain8006.class, args);
       }
   }
   ```

5. 业务类Controller

   ```java
   /**
    * @description:
    * @author: xu
    * @date: 2022/12/24 1:22
    */
   @RestController
   public class PaymentController {
       @Value("${server.port}")
       private String serverPort;
   
       @GetMapping("/payment/consul")
       public String paymentInfo() {
           return "SpringCloud With Consul：" + serverPort + "\t"
                   + UUID.randomUUID().toString();
       }
   }
   ```

6. 验证测试

   `http://localhost:8500`：

   ![image-20221224013101291](SpringCloud-尚硅谷.assets/image-20221224013101291.png)

   `http://localhost:8006/payment/consul`：

   ![image-20221224013134195](SpringCloud-尚硅谷.assets/image-20221224013134195.png)



### 4 服务消费者

1. 新建Module消费服务order80：`cloud-consumerconsul-order80`

2. POM

   ```xml
   <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <parent>
           <artifactId>sh-cloud</artifactId>
           <groupId>com.shanhai.springcloud</groupId>
           <version>1.0-SNAPSHOT</version>
       </parent>
       <modelVersion>4.0.0</modelVersion>
   
       <artifactId>cloud-consumerconsul-order80</artifactId>
   
       <dependencies>
           <!--SpringCloud consul-server -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-consul-discovery</artifactId>
           </dependency>
           <!-- SpringBoot整合Web组件 -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
           <!--日常通用jar包配置-->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-devtools</artifactId>
               <scope>runtime</scope>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <optional>true</optional>
           </dependency>
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
       </dependencies>
   </project>
   ```

3. YML：application.yml

   ```yml
   ###consul服务端口号
   server:
     port: 80
   
   spring:
     application:
       name: cloud-consumer-order
     ####consul注册中心地址
     cloud:
       consul:
         host: localhost
         port: 8500
         discovery:
           #hostname: 127.0.0.1
           service-name: ${spring.application.name}
   ```

4. 主启动类

   ```java
   //该注解用于向使用consul或者zookeeper作为注册中心时注册服务
   @EnableDiscoveryClient
   @SpringBootApplication
   public class OrderConsulMain80 {
       public static void main(String[] args) {
           SpringApplication.run(OrderConsulMain80.class, args);
       }
   }
   ```

5. 配置Config

   ```java
   @Configuration
   public class ApplicationContextConfig {
       @Bean
       @LoadBalanced
       public RestTemplate getRestTemplate() {
           return new RestTemplate();
       }
   }
   ```

6. Controller

   ```java
   @RestController
   public class OrderConsulController {
       // consul-provider-payment
       public static final String INVOKE_URL = "http://consul-provider-payment";
   
       @Autowired
       private RestTemplate restTemplate;
   
       @GetMapping(value = "/consumer/payment/consul")
       public String paymentInfo() {
           String result = restTemplate.getForObject(INVOKE_URL + "/payment/consul", String.class);
           System.out.println("消费者调用支付服务(consul)--->result:" + result);
           return result;
       }
   }
   ```

7. 验证测试

   `http://localhost:8500`：

   ![image-20221224015124665](SpringCloud-尚硅谷.assets/image-20221224015124665.png)

   `http://localhost/consumer/payment/consul`：

   ![image-20221224015225329](SpringCloud-尚硅谷.assets/image-20221224015225329.png)



### 5 三个注册中心异同点

| **组件名** | **语言** | **CAP** | **服务健康检查** | **对外暴露接口** | **SpringCloud集成** |
| ---------- | -------- | ------- | ---------------- | ---------------- | ------------------- |
| Eureka     | Java     | AP      | 可配支持         | HTTP             | 已集成              |
| Consul     | Go       | CP      | 支持             | HTTP/DNS         | 已集成              |
| Zookeeper  | Java     | CP      | 支持             | 客户端           | 已集成              |



> CAP：

- C：Consistency（强一致性）
- A：Availability（可用性）
- P：Partition tolerance（分区容错性）
- CAP理论关注粒度是数据，而不是整体系统设计的策略

> 经典CAP图：

*最多只能同时较好的满足两个*。

CAP理论的核心是：*一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求*。

因此，根据 CAP 原理将 NoSQL 数据库分成了**满足 CA 原则**、**满足 CP 原则**和**满足 AP 原则**三大类：

- CA --- 单点集群，满足一致性，可用性的系统，通常在可扩展性上不太强大。
- CP --- 满足一致性，分区容错性的系统，通常性能不是特别高。
- AP --- 满足可用性，分区容错性的系统，通常可能对一致性要求低一些。

![image-20221224020110783](SpringCloud-尚硅谷.assets/image-20221224020110783.png)

> AP(Eureka)

AP架构

当网络分区出现后，为了保证可用性，系统B*可以返回旧值*，保证系统的可用性。

结论：**违背了一致性C的要求，只满足可用性和分区容错，即AP**。

![image-20221224021319927](SpringCloud-尚硅谷.assets/image-20221224021319927.png)



> CP(Zookeeper/Consul)

CP架构

当网络分区出现后，为了保证一致性，就必须拒接请求，否则无法保证一致性。

结论：**违背了可用性A的要求，只满足一致性和分区容错，即CP**。

![image-20221224021851046](SpringCloud-尚硅谷.assets/image-20221224021851046.png)

+++

## 七、Ribbon负载均衡服务调用

### 1 概述

> Ribbon是什么？

Spring Cloud Ribbon是基于Netflix Ribbon实现的一套**客户端负载均衡的工具**。

简单的说，Ribbon是Netflix发布的开源项目，主要功能是提供**客户端的软件负载均衡算法和服务调用**。Ribbon客户端组件提供一系列完善的配置项如连接超时，重试等。简单的说，就是在配置文件中列出Load Balancer（简称LB）后面所有的机器，Ribbon会自动的帮助你基于某种规则（如简单轮询，随机连接等）去连接这些机器。我们很容易使用Ribbon实现自定义的负载均衡算法。



官网资料：https://github.com/Netflix/ribbon/wiki/Getting-Started

Ribbon目前也进入维护模式，

![image-20221224160829403](SpringCloud-尚硅谷.assets/image-20221224160829403.png)

未来替换方案：Spring Cloud loadbalancer



> Ribbon能干嘛？

- **LB（负载均衡）**

  1. *LB负载均衡(Load Balance)是什么？*

     简单的说就是将用户的请求平摊的分配到多个服务上，从而达到系统的HA（高可用）。

     常见的负载均衡有软件Nginx，LVS，硬件 F5等。

  2. *Ribbon本地负载均衡客户端 VS Nginx服务端负载均衡区别？*

     Nginx是服务器负载均衡，客户端所有请求都会交给nginx，然后由nginx实现转发请求。即负载均衡是由服务端实现的。

     Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心上获取注册信息服务列表之后缓存到JVM本地，从而在本地实现RPC远程服务调用技术。

- **集中式LB**

  即在服务的消费方和提供方之间使用独立的LB设施(可以是硬件，如F5, 也可以是软件，如nginx), 由该设施负责把访问请求通过某种策略转发至服务的提供方；

- **进程内LB**

  将LB逻辑集成到消费方，消费方从服务注册中心获知有哪些地址可用，然后自己再从这些地址中选择出一个合适的服务器。

  **Ribbon就属于进程内LB**，它只是一个类库，**集成于消费方进程**，消费方通过它来获取到服务提供方的地址。



前面我们讲解过了80通过轮询负载访问8001/8002

一句话：负载均衡+RestTemplate调用



### 2 Ribbon负载均衡演示

> 架构说明

![image-20221224161845926](SpringCloud-尚硅谷.assets/image-20221224161845926.png)

Ribbon在工作时分成两步：

1. 第一步先选择 EurekaServer，它优先选择在同一个区域内负载较少的server。
2. 第二步再根据用户指定的策略，在从server取到的服务注册列表中选择一个地址。其中Ribbon提供了多种策略：比如轮询、随机和根据响应时间加权。

总结：Ribbon其实就是一个软负载均衡的客户端组件，他可以和其他所需请求的客户端结合使用，和eureka结合只是其中的一个实例。



> POM

之前写样例时候没有引入spring-cloud-starter-ribbon也可以使用ribbon

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-ribbon</artifactId>
</dependency>
```

猜测spring-cloud-starter-netflix-eureka-client自带了spring-cloud-starter-ribbon引用

证明如下：可以看到 spring-cloud-starter-netflix-eureka-client 确实引入了Ribbon

![image-20221224162227985](SpringCloud-尚硅谷.assets/image-20221224162227985.png)



> RestTemplate的使用

官网：https://docs.spring.io/spring-framework/docs/5.2.2.RELEASE/javadoc-api/org/springframework/web/client/RestTemplate.html

![image-20221224162414032](SpringCloud-尚硅谷.assets/image-20221224162414032.png)

1. getForObject方法/getForEntity方法

   **返回对象为响应体中数据转化成的对象，基本上可以理解为Json**

   ![image-20221224162609871](SpringCloud-尚硅谷.assets/image-20221224162609871.png)

   **返回对象为ResponseEntity对象，包含了响应中的一些重要信息，比如响应头、响应状态码、响应体等**

   ![image-20221224162639660](SpringCloud-尚硅谷.assets/image-20221224162639660.png)

2. postForObject/postForEntity

   ![image-20221224162814999](SpringCloud-尚硅谷.assets/image-20221224162814999.png)

3. GET请求方法

   ```java
   <T> T getForObject(String url, Class<T> responseType, Object... uriVariables);
    
   <T> T getForObject(String url, Class<T> responseType, Map<String, ?> uriVariables);
    
   <T> T getForObject(URI url, Class<T> responseType);
    
   <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Object... uriVariables);
    
   <T> ResponseEntity<T> getForEntity(String url, Class<T> responseType, Map<String, ?> uriVariables);
    
   <T> ResponseEntity<T> getForEntity(URI var1, Class<T> responseType);
   
   ```

4. POST请求方法

   ```java
   <T> T postForObject(String url, @Nullable Object request, Class<T> responseType, Object... uriVariables);
    
   <T> T postForObject(String url, @Nullable Object request, Class<T> responseType, Map<String, ?> uriVariables);
    
   <T> T postForObject(URI url, @Nullable Object request, Class<T> responseType);
    
   <T> ResponseEntity<T> postForEntity(String url, @Nullable Object request, Class<T> responseType, Object... uriVariables);
    
   <T> ResponseEntity<T> postForEntity(String url, @Nullable Object request, Class<T> responseType, Map<String, ?> uriVariables);
    
   <T> ResponseEntity<T> postForEntity(URI url, @Nullable Object request, Class<T> responseType);
   ```



### 3 Ribbon核心组件IRule

> IRule：根据特定算法中从服务列表中选取一个要访问的服务

![image-20221224164222858](SpringCloud-尚硅谷.assets/image-20221224164222858.png)

1. com.netflix.loadbalancer.RoundRobinRule：轮询
2. com.netflix.loadbalancer.RandomRule：随机
3. com.netflix.loadbalancer.RetryRule：先按照RoundRobinRule的策略获取服务，如果获取服务失败则在指定时间内会进行重试，获取可用的服务
4. WeightedResponseTimeRule：对RoundRobinRule的扩展，响应速度越快的实例选择权重越大，越容易被选择
5. BestAvailableRule：会先过滤掉由于多次访问故障而处于断路器跳闸状态的服务，然后选择一个并发量最小的服务
6. AvailabilityFilteringRule：先过滤掉故障实例，再选择并发较小的实例
7. ZoneAvoidanceRule：默认规则，复合判断server所在区域的性能和server的可用性选择服务器



> 如何替换负载均衡算法？

1. 修改cloud-consumer-order80

2. 注意配置细节

   *官方文档明确给出了警告*：<font color="gree">这个自定义配置类不能放在@ComponentScan所扫描的当前包下以及子包下，否则我们自定义的这个配置类就会被所有的Ribbon客户端所共享，达不到特殊化定制的目的了。</font>

3. 新建package：com.shanhai.myrule

4. 上面包下新建MySelfRule规则类

   ![image-20221224165117675](SpringCloud-尚硅谷.assets/image-20221224165117675.png)

5. 主启动类添加`@RibbonClient`

   ```java
   package com.shanhai.springcloud;
   
   import com.shanhai.myrule.MySelfRule;
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.netflix.eureka.EnableEurekaClient;
   import org.springframework.cloud.netflix.ribbon.RibbonClient;
   
   // 在启动该微服务的时候就能去加载我们的自定义Ribbon配置类，从而使配置生效，形如：
   @RibbonClient(name = "CLOUD-PAYMENT-SERVICE", configuration = MySelfRule.class)
   @EnableEurekaClient
   @SpringBootApplication
   public class MainApp80 {
       public static void main(String[] args) {
           SpringApplication.run(MainApp80.class,args);
       }
   }
   ```

6. 测试：http://localhost/consumer/payment/get/31

   结果，随机访问8001/8002端口



### 4 Ribbon负载均衡算法

> 原理

**负载均衡算法**：*rest接口第几次请求数 % 服务器集群总数量 = 实际调用服务器位置下标，每次服务重启动后rest接口计数从1开始*。

`List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");`

如：

`List [0] instances = 127.0.0.1:8002`

`List [1] instances = 127.0.0.1:8001`

8001+ 8002 组合成为集群，它们共计2台机器，集群总数为2，按照轮询算法原理：

- 当总请求数为1时：1 % 2 =1 对应下标位置为1，则获得服务地址为127.0.0.1:8001
- 当总请求数位2时：2 % 2 =0 对应下标位置为0，则获得服务地址为127.0.0.1:8002
- 当总请求数位3时：3 % 2 =1 对应下标位置为1，则获得服务地址为127.0.0.1:8001
- 当总请求数位4时：4 % 2 =0 对应下标位置为0，则获得服务地址为127.0.0.1:8002
- 如此类推......



> 手写：自己试着写一个本地负载均衡器试试

- 7001/7002集群启动

- 8001/8002微服务改造，controller

  分别添加一个相同的方法即可

  ```java
  @Value("${server.port}") // 8001 / 8002
  private String serverPort;
  
  @GetMapping(value = "/payment/lb")
  public String getPaymentLB() {
      return serverPort;
  }
  ```

  8001/8002集群启动

- 80订单微服务改造

  1. ApplicationContextConfig去掉注解`@LoadBalanced`

     ```java
     @Configuration
     public class ApplicationContextConfig {
         //@LoadBalanced
         @Bean
         public RestTemplate getRestTemplate() {
             return new RestTemplate();
         }
     }
     ```

  2. LoadBalancer接口

     ```java
     package com.shanhai.springcloud.lb;
     
     import org.springframework.cloud.client.ServiceInstance;
     import java.util.List;
     
     public interface LoadBalancer {
         ServiceInstance instances(List<ServiceInstance> serviceInstances);
     }
     ```

  3. MyLB实现LoadBalancer接口

     ```java
     package com.shanhai.springcloud.lb;
     
     import org.springframework.cloud.client.ServiceInstance;
     import org.springframework.stereotype.Component;
     
     import java.util.List;
     import java.util.concurrent.atomic.AtomicInteger;
     
     @Component
     public class MyLB implements LoadBalancer {
         private AtomicInteger atomicInteger = new AtomicInteger(0);
     
         public final int getAtomicInteger() {
             int current;
             int next;
             do {
                 current = this.atomicInteger.get();
                 next = current >= Integer.MAX_VALUE ? 0 : current + 1;
             } while (!this.atomicInteger.compareAndSet(current, next));
             System.out.println("第几次访问次数next = " + next);
             return next;
         }
     
         @Override
         public ServiceInstance instances(List<ServiceInstance> serviceInstances) {
             int index = getAtomicInteger() % serviceInstances.size();
             return serviceInstances.get(index);
         }
     }
     ```

  4. OrderController

     ```java
     // org.springframework.web.client.RestTemplate;
     @Resource
     private RestTemplate restTemplate;
     
     @Resource
     private LoadBalancer loadBalancer;
     
     // org.springframework.cloud.client.discovery.DiscoveryClient;
     @Resource
     private DiscoveryClient discoveryClient;
     
     @GetMapping("/consumer/payment/lb")
     public String getPaymentLB() {
         List<ServiceInstance> instances = discoveryClient.getInstances("CLOUD-PAYMENT-SERVICE");
         if (instances != null && instances.size() <= 0) {
             return null;
         }
         ServiceInstance serviceInstance = loadBalancer.instances(instances);
         URI uri = serviceInstance.getUri();
         return restTemplate.getForObject(uri + "/payment/lb", String.class);
     }
     ```

- 测试：`http://localhost/consumer/payment/lb`

  结果：轮询访问8001/8002

+++

## 八、OpenFeign服务接口调用

### 1 概述

#### 1.1 OpenFeign是什么？

官网解释：https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/#spring-cloud-openfeign

Feign是一个声明式WebService客户端。使用Feign能让编写Web Service客户端更加简单。

它的使用方法是*定义一个服务接口然后在上面添加注解*。Feign也支持可拔插式的编码器和解码器。Spring Cloud对Feign进行了封装，使其支持了Spring MVC标准注解和HttpMessageConverters。Feign可以与Eureka和Ribbon组合使用以支持负载均衡。

![image-20221224182459281](SpringCloud-尚硅谷.assets/image-20221224182459281.png)



Feign是一个声明式的Web服务客户端，让编写Web服务客户端变得非常容易，**只需创建一个接口并在接口上添加注解即可**。

GitHub：https://github.com/spring-cloud/spring-cloud-openfeign



#### 1.2 OpenFeign能干嘛？

- *Feign能干什么*：

  Feign旨在使编写Java Http客户端变得更容易。

  前面在使用Ribbon+RestTemplate时，利用RestTemplate对http请求的封装处理，形成了一套模版化的调用方法。但是在实际开发中，由于对服务依赖的调用可能不止一处，**往往一个接口会被多处调用，所以通常都会针对每个微服务自行封装一些客户端类来包装这些依赖服务的调用**。所以，Feign在此基础上做了进一步封装，由他来帮助我们定义和实现依赖服务接口的定义。**在Feign的实现下，我们只需创建一个接口并使用注解的方式来配置它(以前是Dao接口上面标注Mapper注解，现在是一个微服务接口上面标注一个Feign注解即可)**，即可完成对服务提供方的接口绑定，简化了使用Spring cloud Ribbon时，自动封装服务调用客户端的开发量。

- *Feign集成了Ribbon*：

  利用Ribbon维护了Payment的服务列表信息，并且通过轮询实现了客户端的负载均衡。而与Ribbon不同的是，**通过feign只需要定义服务绑定接口且以声明式的方法**，优雅而简单的实现了服务调用



#### 1.3 Feign和OpenFeign两者区别

![image-20221224183434006](SpringCloud-尚硅谷.assets/image-20221224183434006.png)



### 2 OpenFeign使用步骤

**接口+注解：微服务调用接口+@FeignClient**

1. 新建Maven子模块：cloud-consumer-feign-order80

   Feign在消费端使用

2. POM

   ```xml
   <dependencies>
       <!--openfeign-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-openfeign</artifactId>
       </dependency>
       <!--eureka client-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <!-- 引入自己定义的api通用包，可以使用Payment支付Entity -->
       <dependency>
           <groupId>com.shanhai.springcloud</groupId>
           <artifactId>cloud-api-commons</artifactId>
           <version>${project.version}</version>
       </dependency>
       <!--web-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <!--一般基础通用配置-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```

3. YML

   ```yaml
   server:
     port: 80
   
   eureka:
     client:
       register-with-eureka: false
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/
   ```

4. 主启动类：加注解`@EnableFeignClients`

   ```java
   package com.shanhai.springcloud;
   
   import org.springframework.boot.SpringApplication;
   import org.springframework.boot.autoconfigure.SpringBootApplication;
   import org.springframework.cloud.openfeign.EnableFeignClients;
   
   @SpringBootApplication
   @EnableFeignClients
   public class OrderFeignMain80 {
       public static void main(String[] args) {
           SpringApplication.run(OrderFeignMain80.class, args);
       }
   }
   ```

5. 业务类

   - 业务逻辑接口+*@FeignClient配置调用provider服务*

   - 新建`PaymentFeignService接口`并*新增注解`@FeignClient`*

     ```java
     package com.shanhai.springcloud.service;
     
     import com.shanhai.springcloud.entities.CommonResult;
     import com.shanhai.springcloud.entities.Payment;
     import org.springframework.cloud.openfeign.FeignClient;
     import org.springframework.stereotype.Component;
     import org.springframework.web.bind.annotation.GetMapping;
     import org.springframework.web.bind.annotation.PathVariable;
     
     @Component
     @FeignClient(value = "CLOUD-PAYMENT-SERVICE")
     public interface PaymentFeignService {
         @GetMapping(value = "/payment/get/{id}")
         public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);
     }
     ```

   - 控制层Controller

     ```java
     @RestController
     @Slf4j
     public class OrderFeignController {
         @Resource
         private PaymentFeignService paymentFeignService;
     
         @GetMapping(value = "/consumer/payment/get/{id}")
         public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
             CommonResult<Payment> commonResult = paymentFeignService.getPaymentById(id);
             log.info("commonResult.Payment" + commonResult.getData());
             return commonResult;
         }
     }
     ```

6. 测试

   先启动2个eureka集群7001/7002

   再启动2个微服务8001/8002

   启动cloud-consumer-feign-order80

   访问：`http://localhost/consumer/payment/get/31`

   Feign自带负载均衡配置项

   ![image-20221224195005765](SpringCloud-尚硅谷.assets/image-20221224195005765.png)

   ![image-20221224195015313](SpringCloud-尚硅谷.assets/image-20221224195015313.png)

7. 小总结

   ![image-20221224195409947](SpringCloud-尚硅谷.assets/image-20221224195409947.png)



### 3 OpenFeign超时控制

超时设置，故意设置超时演示出错情况：

1. 服务提供方8001故意写暂停程序：8001 controller

   ```java
   @GetMapping(value = "/payment/feign/timeout")
   public String paymentFeignTimeOut()
   {
       System.out.println("*****paymentFeignTimeOut from port: " + serverPort);
       //暂停几秒钟线程
       try {
           TimeUnit.SECONDS.sleep(3);
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
       return serverPort;
   }
   ```

2. 服务消费方80添加超时方法PaymentFeignService

   ```java
   @Component
   @FeignClient(value = "CLOUD-PAYMENT-SERVICE")
   public interface PaymentFeignService {
       @GetMapping(value = "/payment/get/{id}")
       CommonResult<Payment> getPaymentById(@PathVariable("id") Long id);
   
       @GetMapping(value = "/payment/feign/timeout")
       String paymentFeignTimeOut();
   }
   ```

3. 服务消费方80添加超时方法OrderFeignController

   ```java
   @RestController
   @Slf4j
   public class OrderFeignController {
       @Resource
       private PaymentFeignService paymentFeignService;
   
       @GetMapping(value = "/consumer/payment/get/{id}")
       public CommonResult<Payment> getPaymentById(@PathVariable("id") Long id) {
           CommonResult<Payment> commonResult = paymentFeignService.getPaymentById(id);
           log.info("commonResult.Payment" + commonResult.getData());
           return commonResult;
       }
   
       @GetMapping(value = "/consumer/payment/feign/timeout")
       public String paymentFeignTimeOut(){
           return paymentFeignService.paymentFeignTimeOut();
       }
   }
   ```

4. 测试：`http://localhost/consumer/payment/feign/timeout`

   错误页面：

   ![image-20221224201822915](SpringCloud-尚硅谷.assets/image-20221224201822915.png)



OpenFeign默认等待1秒钟，超过后报错：默认Feign客户端只等待一秒钟，但是服务端处理需要超过1秒钟，导致Feign客户端不想等待了，直接返回报错。为了避免这样的情况，有时候我们需要设置Feign客户端的超时控制。

yml文件中开启配置

OpenFeign默认支持Ribbon

YML文件里需要开启OpenFeign客户端超时控制

```yaml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/

#设置feign客户端超时时间(OpenFeign默认支持ribbon)
ribbon:
  #指的是建立连接所用的时间，适用于网络状况正常的情况下,两端连接所用的时间
  ReadTimeout: 5000
  #指的是建立连接后从服务器读取到可用资源所用的时间
  ConnectTimeout: 5000
```

测试：测试通过

![image-20221224202109772](SpringCloud-尚硅谷.assets/image-20221224202109772.png)



### 4 OpenFeign日志打印功能

是什么？

Feign 提供了日志打印功能，我们可以通过配置来调整日志级别，从而了解 Feign 中 Http 请求的细节。

说白了就是**对Feign接口的调用情况进行监控和输出**。



日志级别：

- NONE：默认的，不显示任何日志；
- BASIC：仅记录请求方法、URL、响应状态码及执行时间；
- HEADERS：除了 BASIC 中定义的信息之外，还有请求和响应的头信息；
- FULL：除了 HEADERS 中定义的信息之外，还有请求和响应的正文及元数据。



配置日志bean：

```java
package com.shanhai.springcloud.config;

import feign.Logger;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class FeignConfig {
    @Bean
    Logger.Level feignLoggerLevel() {
        return Logger.Level.FULL;
    }
}
```



YML文件里需要开启日志的Feign客户端：

```yaml
server:
  port: 80

eureka:
  client:
    register-with-eureka: false
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/,http://eureka7002.com:7002/eureka/

#设置feign客户端超时时间(OpenFeign默认支持ribbon)
ribbon:
  #指的是建立连接所用的时间，适用于网络状况正常的情况下,两端连接所用的时间
  ReadTimeout: 5000
  #指的是建立连接后从服务器读取到可用资源所用的时间
  ConnectTimeout: 5000

logging:
  level:
    # feign日志以什么级别监控哪个接口
    com.shanhai.springcloud.service.PaymentFeignService: debug
```



测试：后台日志查看

![image-20221224202948935](SpringCloud-尚硅谷.assets/image-20221224202948935.png)

+++

## 九、Hystrix断路器

### 1 概述

> 分布式系统面临的问题

*复杂分布式体系结构中的应用程序有数十个依赖关系，每个依赖关系在某些时候将不可避免地失败*。

![image-20221224204608808](SpringCloud-尚硅谷.assets/image-20221224204608808.png)

**服务雪崩**：

多个微服务之间调用的时候，假设微服务A调用微服务B和微服务C，微服务B和微服务C又调用其它的微服务，这就是所谓的“**扇出**”。如果扇出的链路上某个微服务的调用响应时间过长或者不可用，对微服务A的调用就会占用越来越多的系统资源，进而引起系统崩溃，所谓的“雪崩效应”。

对于高流量的应用来说，单一的后端依赖可能会导致所有服务器上的所有资源都在几秒钟内饱和。比失败更糟糕的是，这些应用程序还可能导致服务之间的延迟增加，备份队列，线程和其他系统资源紧张，导致整个系统发生更多的级联故障。这些都表示需要对故障和延迟进行隔离和管理，以便单个依赖关系的失败，不能取消整个应用程序或系统。

所以，通常当你发现一个模块下的某个实例失败后，这时候这个模块依然还会接收流量，然后这个有问题的模块还调用了其他的模块，这样就会发生级联故障，或者叫雪崩。



> Hystrix是什么？

Hystrix是一个用于处理分布式系统的**延迟**和**容错**的开源库，在分布式系统里，许多依赖不可避免的会调用失败，比如超时、异常等，Hystrix能够保证在一个依赖出问题的情况下，**不会导致整体服务失败，避免级联故障，以提高分布式系统的弹性**。

“断路器”本身是一种开关装置，当某个服务单元发生故障之后，通过断路器的故障监控（类似熔断保险丝），**向调用方返回一个符合预期的、可处理的备选响应（FallBack），而不是长时间的等待或者抛出调用方无法处理的异常**，这样就保证了服务调用方的线程不会被长时间、不必要地占用，从而避免了故障在分布式系统中的蔓延，乃至雪崩。



> Hystrix能干嘛？

- 服务降级
- 服务熔断
- 接近实时的监控
- ......



官网资料：https://github.com/Netflix/Hystrix/wiki/How-To-Use

*Hystrix官宣，停更进维*：https://github.com/Netflix/Hystrix

- 被动修复bugs
- 不再接受合并请求
- 不再发布新版本

![image-20221224205534981](SpringCloud-尚硅谷.assets/image-20221224205534981.png)



### 2 Hystrix重要概念

- **服务降级**

  服务器忙，请稍后再试，不让客户端等待并立刻返回一个友好提示，fallback

  哪些情况会出发降级：

  程序运行异常、超时、服务熔断触发服务降级、线程池/信号量打满也会导致服务降级

- **服务熔断**

  类比保险丝达到最大服务访问后，直接拒绝访问，拉闸限电，然后调用服务降级的方法并返回友好提示

  就是保险丝：服务的降级->进而熔断->恢复调用链路

- **服务限流**

  秒杀高并发等操作，严禁一窝蜂的过来拥挤，大家排队，一秒钟N个，有序进行



### 3 Hystrix案例

#### 3.1 构建提供者支付模块8001

1. 新建Maven子模块：cloud-provider-hystrix-payment8001

2. POM

   ```xml
   <dependencies>
       <!--hystrix-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
       </dependency>
       <!--eureka client-->
       <dependency>
           <groupId>org.springframework.cloud</groupId>
           <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
       </dependency>
       <!--web-->
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-web</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-actuator</artifactId>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-devtools</artifactId>
           <scope>runtime</scope>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.projectlombok</groupId>
           <artifactId>lombok</artifactId>
           <optional>true</optional>
       </dependency>
       <dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-test</artifactId>
           <scope>test</scope>
       </dependency>
   </dependencies>
   ```

3. YML

   ```yaml
   server:
     port: 8001
   
   spring:
     application:
       name: cloud-provider-hystrix-payment
   
   eureka:
     client:
       register-with-eureka: true
       fetch-registry: true
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   @EnableEurekaClient //本服务启动后会自动注册进eureka服务中
   public class PaymentHystrixMain8001 {
       public static void main(String[] args) {
           SpringApplication.run(PaymentHystrixMain8001.class, args);
       }
   }
   ```

5. 业务类

   service

   ```java
   @Service
   public class PaymentService {
       /**
        * 正常访问，一切OK
        * @param id
        * @return
        */
       public String paymentInfo_OK(Integer id) {
           return "线程池：" + Thread.currentThread().getName() +
                   "paymentInfo_OK,id: " + id + "\t" + "O(∩_∩)O";
       }
   
       /**
        * 超时访问，演示降级
        * @param id
        * @return
        */
       public String paymentInfo_TimeOut(Integer id) {
           Integer timeSeconds = 3;
           try { 
               TimeUnit.SECONDS.sleep(timeSeconds); 
           } catch (InterruptedException e) { 
               e.printStackTrace(); }
           return "线程池：" + Thread.currentThread().getName() + "paymentInfo_TimeOut,id: " +
                   id + "\t" + "O(∩_∩)O，耗费" + timeSeconds + "秒";
       }
   }
   ```

   controller

   ```java
   @RestController
   @Slf4j
   public class PaymentController {
       @Resource
       private PaymentService paymentService;
   
       @GetMapping("/payment/hystrix/ok/{id}")
       public String paymentInfo_OK(@PathVariable("id") Integer id) {
           String result = paymentService.paymentInfo_OK(id);
           log.info("****result: "+result);
           return result;
       }
   
       @GetMapping("/payment/hystrix/timeout/{id}")
       public String paymentInfo_TimeOut(@PathVariable("id") Integer id) throws InterruptedException {
           String result = paymentService.paymentInfo_TimeOut(id);
           log.info("****result: "+result);
           return result;
       }
   }
   ```

6. 正常测试

   启动eureka7001

   启动cloud-provider-hystrix-payment8001

   访问:

   success的方法：`http://localhost:8001/payment/hystrix/ok/31`

   每次调用耗费3秒钟：`http://localhost:8001/payment/hystrix/timeout/31`

   ![image-20221224230332291](SpringCloud-尚硅谷.assets/image-20221224230332291.png)

   ![image-20221224230334321](SpringCloud-尚硅谷.assets/image-20221224230334321.png)

   上述module均OK，以上述为根基平台，从正确->错误->降级熔断->恢复



#### 3.2 高并发测试1

上述在非高并发情形下，还能勉强满足，but......

Jmeter压测测试：

- 开启Jmeter，来20000个并发压死8001，20000个请求都去访问paymentInfo_TimeOut服务

  ![image-20221224231345173](SpringCloud-尚硅谷.assets/image-20221224231345173.png)

  ![image-20221224231347219](SpringCloud-尚硅谷.assets/image-20221224231347219.png)

- 再来一个访问：`http://localhost:8001/payment/hystrix/ok/31`
  

- 看演示结果

  两个请求都在自己转圈圈

  为什么会被卡死：tomcat的默认的工作线程数被打满了，没有多余的线程来分解压力和处理。

- Jmeter压测结论

  上面还是**服务提供者8001**自己测试，假如此时外部的消费者80也来访问，那**消费者**只能干等，最终导致消费端80不满意，服务端8001直接被拖死。



#### 3.3 构建消费者模块80

> 看热闹不嫌弃事大，80新建加入

1. 新建Maven子模块：cloud-consumer-feign-hystrix-order80

2. POM

   ```xml
   <dependencies>
     <!--openfeign-->
     <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-openfeign</artifactId>
     </dependency>
     <!--hystrix-->
     <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
     </dependency>
     <!--eureka client-->
     <dependency>
       <groupId>org.springframework.cloud</groupId>
       <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
     </dependency>
     <!--web-->
     <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-web</artifactId>
     </dependency>
     <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-actuator</artifactId>
     </dependency>
     <!--一般基础通用配置-->
     <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-devtools</artifactId>
       <scope>runtime</scope>
       <optional>true</optional>
     </dependency>
     <dependency>
       <groupId>org.projectlombok</groupId>
       <artifactId>lombok</artifactId>
       <optional>true</optional>
     </dependency>
     <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-test</artifactId>
       <scope>test</scope>
     </dependency>
   </dependencies>
   ```

3. YML

   ```yaml
   server:
     port: 80
   
   eureka:
     client:
       register-with-eureka: false
       service-url:
         defaultZone: http://eureka7001.com:7001/eureka/
   ```

4. 主启动类

   ```java
   @SpringBootApplication
   @EnableFeignClients
   public class OrderHystrixMain80 {
       public static void main(String[] args) {
           SpringApplication.run(OrderHystrixMain80.class,args);
       }
   }
   ```

5. 业务类

   service接口

   ```java
   @Component
   @FeignClient(value = "CLOUD-PROVIDER-HYSTRIX-PAYMENT")
   public interface PaymentHystrixService {
       @GetMapping("/payment/hystrix/ok/{id}")
       String paymentInfo_OK(@PathVariable("id") Integer id);
   
       @GetMapping("/payment/hystrix/timeout/{id}")
       String paymentInfo_TimeOut(@PathVariable("id") Integer id);
   }
   ```

   controller

   ```java
   @RestController
   @Slf4j
   public class OrderHystrixController {
       @Resource
       private PaymentHystrixService paymentHystrixService;
   
       @GetMapping("/consumer/payment/hystrix/ok/{id}")
       public String paymentInfo_OK(@PathVariable("id") Integer id) {
           String result = paymentHystrixService.paymentInfo_OK(id);
           log.info("****result: "+result);
           return result;
       }
   
       @GetMapping("/consumer/payment/hystrix/timeout/{id}")
       public String paymentInfo_TimeOut(@PathVariable("id") Integer id) throws InterruptedException {
           String result = paymentHystrixService.paymentInfo_TimeOut(id);
           log.info("****result: "+result);
           return result;
       }
   }
   ```

6. 正常测试：`http://localhost/consumer/payment/hystrix/ok/31`

   正常访问



#### 3.4 高并发测试2

高并发测试：

2W个线程压8001吗，消费端80微服务再去访问正常的Ok微服务8001地址

`http://localhost/consumer/payment/hystrix/ok/32`

消费者80，o(╥﹏╥)o

- 要么转圈圈等待

- 要么消费端报超时错误

  ![image-20221224201822915](SpringCloud-尚硅谷.assets/image-20221224201822915.png)



故障现象和导致原因：

- 8001同一层次的其它接口服务被困死，因为tomcat线程池里面的工作线程已经被挤占完毕
- 80此时调用8001，客户端访问响应缓慢，转圈圈
- 结论：正因为有上述故障或不佳表现，才有我们的*降级*/*熔断*/*限流*等技术诞生



如何解决：

- 超时导致服务器变慢(转圈)：超时不再等待
- 出错(宕机或程序运行出错)：出错要有兜底
- 解决：
  - 对方服务(8001)超时了，调用者(80)不能一直卡死等待，必须有服务降级
  - 对方服务(8001)宕机了，调用者(80)不能一直卡死等待，必须有服务降级
  - 对方服务(8001)OK，调用者(80)自己出故障或有自我要求（自己的等待时间小于服务提供者），自己处理降级



#### 3.5 服务降级
















































































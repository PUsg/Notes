# SpringBoot



## 基础入门

SpringBoot 是一个高级框架 底层是Spring 是一个整合Spring技术栈的一站式框架 是简化Spring技术栈的快速开发脚手架

#### 优点： 

1.创建独立Spring应用

2.内嵌web服务器

3.自动starter依赖 简化构建配置（不需要导入一大堆的依赖

4.自动配置Spring以及第三方功能

5.提供生产级别的监控 健康检查 以及 外部化配置

6.没有代码生成 不用编写Xml

​			

#### 缺点：

1.迭代快

2.封装太深，内部原理复杂 不容易精通

#### 微服务

微服务是一种架构风格 

一个应用拆分成一组小型服务

每个服务运行在自己的进程内 也就是可独立部署和升级

服务之间使用轻量级HTTP交互

服务围绕业务功能拆分

可以由全自动部署机制独立部署

#### 分布式的困难

远程调用 服务发现 负载均衡 服务容错。。。。



### 自动配置原理

#### 1.1依赖管理

```xml
依赖管理    
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
</parent>

他的父项目
 <parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
  </parent>

几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制


```

#### 1.2自定义修改版本号

```xml
<properties>
        <mysql.version></mysql.version>
</properties>
```



开发导入starter场景启动器

```xml
1、见到很多 spring-boot-starter-* ： *就某种场景
2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入
3、SpringBoot所有支持的场景
https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
4、见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。
5、所有场景启动器最底层的依赖
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.3.4.RELEASE</version>
  <scope>compile</scope>
</dependency>
```



#### 1.3自动配置

- 自动配好Tomcat

- - 引入Tomcat依赖。
  - 配置Tomcat

- 自动配好Web常见功能，如：字符编码问题

- - SpringBoot帮我们配置好了所有web开发的常见场景

- 自动配好SpringMVC

- - 引入SpringMVC全套组件
  - 自动配好SpringMVC常用组件（功能）

- 默认的包结构

- - 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来
  - 无需以前的包扫描配置
  - 想要改变扫描路径，@SpringBootApplication(scanBasePackages=**"com.atguigu"**)

- - - 或者@ComponentScan 指定扫描路径 

```java
@SpringBootApplication
等同于
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan("com.atguigu.boot")
```



### 2容器功能

#### 2.1组件添加

@Configuration

- 基本使用
- **Full模式与Lite模式**

- - 示例
  - 最佳实战

- - - 配置 类组件之间无依赖关系用Lite模式加速容器启动过程，减少判断
    - 配置类组件之间有依赖关系，方法会被调用得到之前单实例组件，用Full模式

@Bean、@Component、@Controller、@Service、@Repository

@ComponentScan、@Import

@Conditional

条件装配：满足Conditional指定的条件，则进行组件注入

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602835786727-28b6f936-62f5-4fd6-a6c5-ae690bd1e31d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



#### 2.2 原生配置文件引入

1、@ImportResource

#### 2.3配置绑定 

如何使用Java读取到properties文件中的内容，并且把它封装到JavaBean中，以供随时使用；

1、@ConfigurationProperties

```java
/**
 * 只有在容器中的组件，才会拥有SpringBoot提供的强大功能
 */
@Component
@ConfigurationProperties(prefix = "mycar")
public class Car {

    private String brand;
    private Integer price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Integer getPrice() {
        return price;
    }

    public void setPrice(Integer price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}
```

2、@EnableConfigurationProperties + @ConfigurationProperties 

3、@Component + @ConfigurationProperties

```java
@EnableConfigurationProperties(Car.class)
//1、开启Car配置绑定功能
//2、把这个Car这个组件自动注册到容器中
public class MyConfig {
}
```



### 3.自动配置原理入门（源码分析见语雀）

#### 3.1引导加载自动配置类

1、@SpringBootConfiguration

@Configuration。代表当前是一个配置类



2、@ComponentScan

指定扫描哪些，Spring注解；



3、@EnableAutoConfiguration 



#### 3.4最佳实践

- 引入场景依赖

- - https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

- 查看自动配置了哪些（选做）

- - 自己分析，引入场景对应的自动配置一般都生效了
  - 配置文件中debug=true开启自动配置报告。Negative（不生效）\Positive（生效）

- 是否需要修改

- - 参照文档修改配置项

- - - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
    - 自己分析。xxxxProperties绑定了配置文件的哪些。

- - 自定义加入或者替换组件

- - - @Bean、@Component。。。

- - 自定义器  **XXXXXCustomizer**；
  - .....



### 开发小技巧

LovBok

简化javaBean开发

引入依赖

```
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

```

dev-tools

项目或者页面修改以后：Ctrl+F9；

```java
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
```

Spring Initailizr

## 核心功能

### 配置文件

#### 1.1properties

#### yaml

基本语法 key value 

大小写敏感

缩进表示层级关系

缩进理论上不用tab 只用空格

相同层级的元素左对齐

'#' 表示注释

'' 和 "" 表示字符串内容 会被转义/不转义

```yaml

```







## Web开发

### 1.SpringMVC自动配置概览

Spring Boot provides auto-configuration for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**







### 2.简单功能分析

#### 2.1静态资源访问

1.将静态资源放在类路径下 ：通过当前项目根路径+静态资源名访问

2.静态资源访问前缀：默认五前缀

3.webjar 



#### 2.2欢迎页支持

静态资源路径下：index.html

可以配置静态资源路径 但是不可以配置静态资源的访问前缀 否则导致index.html不能被访问



#### 2.3 自定义 Favicon

favicon.ico 放在静态资源目录下即可



#### 2.4静态资源配置原理（源码）

SpringBoot启动默认加载 xxxAutoConfiguraiton类（自动配置类）



### 3.请求参数处理

#### 3.1.1rest的使用和原理

@xxxMapping

- Rest风格支持（*使用**HTTP**请求方式动词来表示对资源的操作*）

- - *以前：**/getUser*  *获取用户*    */deleteUser* *删除用户*   */editUser*  *修改用户*      */saveUser* *保存用户*
  - *现在： /user*    *GET-**获取用户*    *DELETE-**删除用户*     *PUT-**修改用户*      *POST-**保存用户*

#### 3.1.2请求映射原理（源码）



#### 3.2普通参数与基本注解

3.2.1 注解：

@PathVariable @RequestHeader @ModelAttribute @RequestParam

@MatrixVariable @CookieValue @RequestBody

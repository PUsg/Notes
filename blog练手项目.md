# blog练手项目

## 1.项目搭建 

###  Spring Boot 项目的搭建 

直接使用 SpringBoot Ini 

###  pom文件的配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.6.11</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>com.puc</groupId>
    <artifactId>blog-api</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>blog-api</name>
    <description>blog-api</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.72</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
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
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

</project>

```

## 2.首页文章列表

###  配置类的编写

```java
@Configuration
@MapperScan("com.puc.blog.mapper")
public class MybatisPlusConfig {

    //分页插件
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }

}
```

**跨域配置**

```java
@Configuration
public class WebMVCConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        //跨域配置
        registry.addMapping("/**").allowedOrigins("http://localhost:8080");
        
    }
}
```



2.1.2 创建pojo类

```java
package com.mszlu.blog.dao.pojo;

import lombok.Data;

@Data // Data注解记得加
public class Article {

    public static final int Article_TOP = 1;

    public static final int Article_Common = 0;

    private Long id;

    private String title;

    private String summary;

    private int commentCounts;

    private int viewCounts;

    /**
     * 作者id
     */
    private Long authorId;
    /**
     * 内容id
     */
    private Long bodyId;
    /**
     *类别id
     */
    private Long categoryId;

    /**
     * 置顶
     */
    private int weight = Article_Common;


    /**
     * 创建时间
     */
    private Long createDate;
}
```



```java
package com.puc.blog.dao.pojo;


import lombok.Data;

@Data // Data 注解记得加
public class SysUser {

    private Long id;

    private String account;

    private Integer admin;

    private String avatar;

    private Long createDate;

    private Integer deleted;

    private String email;

    private Long lastLogin;

    private String mobilePhoneNumber;

    private String nickname;

    private String password;

    private String salt;

    private String status;
}

```

```java
package com.puc.blog.dao.pojo;

import lombok.Data;

@Data
public class Tag {

    private Long id;

    private String avatar;

    private String tagName;

}
```

2.1.3 pojo所对应的mapper接口

```java
package com.puc.blog.dao.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.puc.blog.dao.pojo.Article;

public interface ArticleMapper extends BaseMapper<Article> {
}

```

```java
package com.puc.blog.dao.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.puc.blog.dao.pojo.SysUser;

public interface SysUserMapper extends BaseMapper<SysUser> {
}

```

```java
package com.puc.blog.dao.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.puc.blog.dao.pojo.Tag;

public interface TagMapper extends BaseMapper<Tag> {
}

```

2..1.4  controller

```java
package com.puc.blog.controller;


import com.puc.blog.service.ArticleService;
import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.PageParams;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
//json 数据进行交互
@RestController
@RequestMapping("articles")
public class ArticleController {

    @Autowired
    private ArticleService articleService;

    /**
     * 首页 文章列表
     * @param pageParams
     * @return
     */
    @PostMapping
    public Result listArticle(@RequestBody PageParams pageParams){
        return articleService.listArticle(pageParams);
    }

}

```

返回类Result

```java
package com.puc.blog.vo;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor // 自动生成所有的构造方法
public class Result {

    private boolean success;

    private int code;

    private String msg;

    private Object data;

    public static Result success(Object data){
        return new Result(true,200,"success", data);
    }

    public static Result fail(int code, String msg){
        return new Result(false,code,msg, null);
    }
}

```

Page参数类

```java
package com.puc.blog.vo.params;


import lombok.Data;

@Data
public class PageParams {

    private int page = 1;

    private  int page_size = 10;

}

```

2.1.4 service

文章service接口

```java
package com.puc.blog.service;

import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.PageParams;

public interface ArticleService {


    /**
     * 分页查询 文章列表
     * @param pageParams
     * @return
     */
    Result listArticle(PageParams pageParams);


}

```

实现类

```java
package com.puc.blog.service.impl;


import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.puc.blog.dao.mapper.ArticleMapper;
import com.puc.blog.dao.mapper.TagMapper;
import com.puc.blog.dao.pojo.Article;
import com.puc.blog.service.ArticleService;
import com.puc.blog.service.SysUserService;
import com.puc.blog.service.TagsService;
import com.puc.blog.vo.ArticleVo;
import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.PageParams;

import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.joda.time.DateTime;
import java.util.ArrayList;
import java.util.List;

@Service
public class ArticleServiceImpl implements ArticleService {
    @Autowired
    private ArticleMapper articleMapper;
    @Autowired
    private TagsService tagsService;
    @Autowired
    private SysUserService sysUserService;

    @Override
    public Result listArticle(PageParams pageParams) {            // 做首页的文章展示  需要一个参数 来 确定每页有多少条 一共有多少页

        /**
         * 1.分页查询 article 数据表
         */
        Page<Article> page = new Page<>(pageParams.getPage(), pageParams.getPage_size());
        LambdaQueryWrapper<Article> queryWrapper = new LambdaQueryWrapper<>();
        //是否置顶排序
        // 文章根据创建时间进行倒叙排列
        queryWrapper.orderByDesc(Article::getWeight,Article::getCreateDate);  // queryWrapper 来确定查询的规则
        Page<Article> articlePage = articleMapper.selectPage(page, queryWrapper);  //返回的是MybatisPlus自带的Page类
        List<Article> records = articlePage.getRecords(); // 用这个类中的getRecords方法 返回一个List集合

        //不能直接返回 要将数据 转换成 json格式
        //ArticleVo 就是 封装类
        List<ArticleVo> articleVoList = copyList(records,true,true);
        return Result.success(articleVoList);
    }

    private List<ArticleVo> copyList(List<Article> records,boolean isAuthor,boolean isTags) {
        List<ArticleVo> articleVoList = new ArrayList<>();
        for(Article article : records){
            ArticleVo articleVo = copy(article,isAuthor,isTags);
            articleVoList.add(articleVo);
        }
        return articleVoList;
    }

    private ArticleVo copy(Article article, boolean isTag, boolean isAuthor){
        ArticleVo articleVo = new ArticleVo();
        // 把 article 里的属性分成 articleVo
        BeanUtils.copyProperties(article,articleVo);
        //Vo里存不了CreateDate
        articleVo.setCreateDate(new DateTime(article.getCreateDate()).toString("yyyy-MM-dd HH:mm"));

        //并不是所有的接口 都需要标签 作者信息
        if(isTag){
            Long articleId = article.getId();
            articleVo.setTags(tagsService.findTagsByArticleId(articleId));
        }
        if(isAuthor){
            Long authorId = article.getAuthorId();
            articleVo.setAuthor(sysUserService.findUserById(authorId).getNickname());
        }


        //返回 复制后的Vo
        return articleVo;
    }

}

```

SysUserSevice

```java
package com.puc.blog.service;


import com.puc.blog.dao.pojo.SysUser;

public interface SysUserService {

    SysUser findUserById(Long id);
}

```

SysUserSeviceImpl

```java
package com.puc.blog.service.impl;

import com.puc.blog.dao.mapper.SysUserMapper;
import com.puc.blog.dao.pojo.SysUser;
import com.puc.blog.service.SysUserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class SysUserServiceImpl implements SysUserService {

    @Autowired
    private SysUserMapper sysUserMapper;

    @Override
    public SysUser findUserById(Long id) {
        SysUser sysUser = sysUserMapper.selectById(id);
        if(sysUser == null)
        {
            sysUser = new SysUser();
            sysUser.setNickname("puc");
        }
        return sysUser;
    }
}

```



TagsService

```java
package com.puc.blog.service;

import com.puc.blog.vo.TagVo;

import java.util.List;

public interface TagsService {
    /**
     * 根据文章id查询标签列表
     * @param articleId
     * @return
     */
    List<TagVo> findTagsByArticleId(Long articleId);
}

```

TagsServiceImpl

```java
package com.puc.blog.service.impl;


import com.puc.blog.dao.mapper.TagMapper;
import com.puc.blog.dao.pojo.Tag;
import com.puc.blog.service.TagsService;
import com.puc.blog.vo.TagVo;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;

@Service
public class TagsServiceImpl implements TagsService {
    @Autowired
    private TagMapper tagMapper;

    @Override
    public List<TagVo> findTagsByArticleId(Long articleId) {
        //mybatisplus 无法进行多表查询
        List<Tag> tags = tagMapper.findTagsByArticleId(articleId);
        //要返回vo类型 所以要用copyList
        return copyList(tags);
    }

    public TagVo copy(Tag tag){
        TagVo tagVo = new TagVo();
        BeanUtils.copyProperties(tag,tagVo);
        return tagVo;
    }
    public List<TagVo> copyList(List<Tag> tagList){
        List<TagVo> tagVoList = new ArrayList<>();
        for (Tag tag : tagList) {
            tagVoList.add(copy(tag));
        }
        return tagVoList;
    }


}
```



### 首页 最热标签

2.2.1TagsController

```java
@RestController // RestController 代表返回的是一个json数据
@RequestMapping("tags") //路径映射
public class TagsController {

    @Autowired
    private TagsService tagsService;

    // /tags/hot
    @GetMapping("hot")
    public Result hot(){
        int limit = 6;
        return tagsService.hot(limit);
    }

}
```

2.2.2TagService接口

```Java
public interface TagsService {
    /**
     * 根据文章id查询标签列表
     * @param id
     * @return
     */
    List<TagVo> findTagsByArticleId(Long id);

    Result hot(int limit);
}
```

2.2.3TagServiceImpl 实现类

```java
@Service
public class TagsServiceImpl implements TagsService {
    @Autowired
    private TagMapper tagMapper;

    @Override
    public List<TagVo> findTagsByArticleId(Long id) {
        //mybatis plus 无法进行多表查询
        List<Tag> tags = tagMapper.findTagsByArticleId(id);
        //要返回vo类型 所以要用copyList
        return copyList(tags);
    }

    @Override
    public Result hot(int limit) {

        /**
         * 1.标签 所拥有的文章数量最多
         * 2.查询
         */
        List<Long> hotsTagIds = tagMapper.findHotsTagIds(limit);

        //需求的是tagId 和 tag Name
        //这里查询tagName select * from tag where id in(1,2,3)
        if (CollectionUtils.isEmpty(hotsTagIds)) {
            return Result.success(Collections.emptyList());
        }
        List<Tag> tagList = tagMapper.findTagsByTagIds(hotsTagIds);
        return Result.success(tagList);
    }

    public TagVo copy(Tag tag){
        TagVo tagVo = new TagVo();
        BeanUtils.copyProperties(tag,tagVo);
        return tagVo;
    }
    public List<TagVo> copyList(List<Tag> tagList){
        List<TagVo> tagVoList = new ArrayList<>();
        for (Tag tag : tagList) {
            tagVoList.add(copy(tag));
        }
        return tagVoList;
    }
```

2.2.4TagMapper 

```java
@Mapper
public interface TagMapper extends BaseMapper<Tag> {
    /**
     * 根据文章id 查询标签列表
     * @param articleId
     * @return
     */
    List<Tag> findTagsByArticleId(Long articleId);


    /**
     * 查询最热的标签  前n条
     * @param limit
     * @return
     */
    List<Long> findHotsTagIds(int limit);


    /**
     * 根据tagId 查询 tagName
     * @param tagIds
     * @return
     */
    List<Tag> findTagsByTagIds(List<Long> tagIds);
}
```

Mybatis无法使用多表查询 编写xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.puc.blog.dao.mapper.TagMapper">
    <sql id="all">
        id,avatar,tag_name as tagName
    </sql>
    <select id="findTagsByTagIds" parameterType="list" resultType="com.puc.blog.dao.pojo.Tag">
        select <include refid="all" />  from ms_tag where id in
        <foreach collection="tagIds" item="tagId" separator="," open="(" close=")">
            #{tagId}
        </foreach>
    </select>


    <select id="findHotsTagIds" parameterType="int" resultType="long">
        select tag_id from ms_article_tag at group by tag_id order by count(1) desc limit #{size}
    </select>

    <select id="findTagsByArticleId" parameterType="long" resultType="com.puc.blog.dao.pojo.Tag">
        select <include refid="all" />  from ms_tag
        <where>
            id in
            (select tag_id from ms_article_tag where article_id = #{articleId})
        </where>
    </select>
</mapper>
```



### 统一异常处理

不管是 **controller service** 还是 **dao** 层 都有可能报异常 如果是意料之内的异常 可以直接捕获处理如果是意料之外的需要进行统一处理，进行记录，给用户提示相对比较友好的信息

```java
package com.puc.blog.handler;


import com.puc.blog.vo.Result;
import org.springframework.web.bind.annotation.ControllerAdvice;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;


//对加了@ControllerAdvice 注解方法 进行拦截的处理    AOP
@ControllerAdvice
public class AllExceptionHandler {

    //进行异常处理 处理Exception.class的异常
    @ExceptionHandler(Exception.class)
    @ResponseBody //返回json数据
    public Result doException(Exception ex){
        ex.printStackTrace();
        return Result.fail(-990,"系统异常");
    }
}

```

### 首页 最热文章

Controller

```java
@PostMapping("hot")
    public Result hotArticle(){
        int limit = 5;
        return articleService.hotArticle(limit);
    }
```

Service

```java
Result hotArticle(int limit);
```

ServiceImpl

```java
@Override
    public Result hotArticle(int limit) {
        LambdaQueryWrapper<Article> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.orderByDesc(Article::getViewCounts); // 对浏览量进行倒叙排列
        queryWrapper.select(Article::getId,Article::getTitle);
        queryWrapper.last("limit " + limit); // "limit " + limit 要记得加空格
        // select id, title from article order by view_counts desc limit 5
        List<Article> articles = articleMapper.selectList(queryWrapper);

        return Result.success(copyList(articles, false,false,false));
    }
```

###  首页 最新文章

类似 最热文章 排序方法改一下就行





###  首页 文章归档

controller

```java
/**
     * 文章归档
     * @return
     */
    @PostMapping("listArchives")
    public Result listArchives(){
        return articleService.listArchives();
    }
```

Service

```java
 /**
     * 文章归档
     * @return
     */
    Result listArchives();
```

ServiceImpl

```java
@Override
    public Result listArchives() {
        List<Archives> archivesList = articleMapper.listArchives();
        return Result.success(archivesList);
    }
```



ArticleMapper

```java
package com.puc.blog.dao.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.puc.blog.dao.dos.Archives;
import com.puc.blog.dao.pojo.Article;

import java.util.List;


public interface ArticleMapper extends BaseMapper<Article> {
    List<Archives> listArchives();
}

```

ArticleMapper.xml

```java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >

<mapper namespace="com.puc.blog.dao.mapper.ArticleMapper">

   <select id="listArchives" resultType="com.puc.blog.dao.dos.Archives">
       select year(create_date) as year, month(create_date) as month, count(*) as count from ms_article
       group by year,month
   </select>

</mapper>
```





## 3. 登录

### JWT

jwt 可以生成一个加密的token 作为用登录的令牌 当用户登录成功之后 发放给客户端

请求需要登录的资源或者接口的时候 将token携带 后端验证token是否合法

**jwt有三部分组成**

A：Header

B：playload 存放自定义信息 可以被解密 不能存放敏感信息

C：签证 A和B加上密钥 加密而成 只要密钥不丢失 可以认为是安全的 

jwt主要验证c部分是否合法

### 登录功能

#### **接口说明**

```json
url: /login
请求方式: POST

请求参数
参数名称	参数类型	 说明
account		string		账号
password	string		密码

返回数据
{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": "token"
}
```

#### 功能实现

LoginController

```java
package com.puc.blog.controller;


import com.puc.blog.service.LoginService;
import com.puc.blog.service.SysUserService;
import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.LoginParam;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

/**
 * RestController的作用相当于Controller加ResponseBody共同作用的结果，但采用RestController请求方式一般会采用Restful风格的形式。
 *
 * Controller的作用：声明该类是Controller层的Bean，将该类声明进入Spring容器中进行管理
 *
 * ResponseBody的作用：表明该类的所有方法的返回值都直接进行提交而不经过视图解析器，且返回值的数据自动封装为json的数据格式
 *
 * RestController的作用：包含上面两个的作用，且支持Restful风格的数据提交方式
 *
 * Restful风格：
 *
 * get:获取数据时用的请求方式
 *
 * post：增加数据时的请求方式
 *
 * put：更新数据时的请求方式
 */
@RestController
@RequestMapping("login")
public class LoginController {
//    @Autowired
//    private SysUserService sysUserService

    @Autowired
    private LoginService loginService;

    @PostMapping
    public Result login(@RequestBody LoginParam loginParam){
        //登录 验证用户  访问用户表
        return loginService.login(loginParam);
    }
}

```

LoginService

```java
package com.puc.blog.service;

import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.LoginParam;

public interface LoginService {
    /**
     * 登录功能
     * @param loginParam
     * @return
     */
    Result login(LoginParam loginParam);
}

```



LoginServiceImpl

```java
package com.puc.blog.service.impl;

import com.alibaba.fastjson.JSON;
import com.puc.blog.dao.pojo.SysUser;
import com.puc.blog.service.LoginService;
import com.puc.blog.service.SysUserService;
import com.puc.blog.util.JWTUtils;
import com.puc.blog.vo.ErrorCode;
import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.LoginParam;
import io.netty.util.internal.StringUtil;
import org.joda.time.Days;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;
import org.springframework.util.StringUtils;
import org.apache.commons.codec.digest.DigestUtils;
import java.util.concurrent.TimeUnit;

@Service
public class LoginServiceImpl implements LoginService {

    @Autowired
    private SysUserService sysUserService;

    @Autowired
    private RedisTemplate<String, String> redisTemplate;

    private static final String salt = "puc!@###";

//    public static void main(String[] args) {
//        LoginParam loginParam = new LoginParam();
//        loginParam.setAccount("admin");
//        loginParam.setPassword("admin");
//        LoginServiceImpl loginService = new LoginServiceImpl();
//        System.out.println(loginService.login(loginParam));
//    }


    @Override
    public Result login(LoginParam loginParam) {
        /**
         *  1.检查参数是否合法
         *  2.根据用户名和密码去user表中查询
         *  3.不存在 登陆失败
         *  4.存在 使用jwt生成token 返回前端
         *  5.token 放入 redis 当中 token： user信息 设置过期时间
         *  （登录认证的时候 先认证token 字符串是否合法 去 redis 认证是否存在）
         */
        String account = loginParam.getAccount();
        String password = loginParam.getPassword();
        if(StringUtils.isEmpty(account) || StringUtils.isEmpty(password)){
            return Result.fail(ErrorCode.PARAMS_ERROR.getCode(),ErrorCode.PARAMS_ERROR.getMsg());
        }
        password = DigestUtils.md5Hex(password + salt);
        SysUser sysUser = sysUserService.findUser(account,password);
        if(sysUser == null){
            return Result.fail(ErrorCode.ACCOUNT_PWD_NOT_EXIST.getCode(), ErrorCode.ACCOUNT_PWD_NOT_EXIST.getMsg());
        }

        String token = JWTUtils.createToken(sysUser.getId());

        redisTemplate.opsForValue().set("TOKEN_" + token , JSON.toJSONString(sysUser),1, TimeUnit.DAYS);
        return Result.success(token);
    }
}

```



findUser  数据库中查询用户

```java
//findUser 的功能 在SysUserServiceImpl中实现
@Override
    public SysUser findUser(String account, String password) {
        LambdaQueryWrapper<SysUser> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(SysUser::getAccount,account);
        queryWrapper.eq(SysUser::getPassword,password);
        queryWrapper.select(SysUser::getAccount,SysUser::getId,SysUser::getAvatar,SysUser::getNickname);
        queryWrapper.last("limit 1");

        return sysUserMapper.selectOne(queryWrapper);
    }
```





### 获取用户信息

#### 接口说明

```json
url：/user/currentUser
请求方式：POST
请求参数：

参数名称		参数类型		说明
Authorization	string		头部信息(TOKEN)

返回数据：
{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": {
        "id":1,
        "account":"1",
        "nickaname":"1",
        "avatar":"ss"
    }
}
```



#### 功能实现 

UsersController

```java
package com.puc.blog.controller;


import com.puc.blog.service.SysUserService;
import com.puc.blog.vo.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("users")
public class UsersController {

    @Autowired
    private SysUserService sysUserService;

    @GetMapping("currentUser")
    public Result currentUser(@RequestHeader("Authorization") String token){
        return  sysUserService.findUserByToken(token);
    }

}

```

####                                                                                                                                                                                                                                                                                                                                                                                                                                                                

sysUserService 中定义 findUserByToken

```java
Result findUserByToken(String token);
```

实现类

```java
@Override
    public Result findUserByToken(String token) {
        /**
         * 1.token合法性校验
         *      是否为空 解析是否成功 redis 是否存在
         * 2.如果校验失败  返回错误
         * 3.如果校验成功  返回对应结果 LoginUserVo
         */

        SysUser sysUser = loginService.checkToken(token);
        if(sysUser == null){
            return Result.fail(ErrorCode.TOKEN_ERROR.getCode(), ErrorCode.TOKEN_ERROR.getMsg());
        }
        LoginUserVo loginUserVo = new LoginUserVo();
        loginUserVo.setId(sysUser.getId());
        loginUserVo.setNickname(sysUser.getNickname());
        loginUserVo.setAccount(sysUser.getAccount());
        loginUserVo.setAvatar(sysUser.getAvatar());
        return Result.success(loginUserVo);
    }
```



### 退出登录

#### 接口说明

```json
url: /logout
请求方式：GET

请求参数：
参数名称		参数类型	说明
Authorization	string		头部信息(TOKEN)

返回数据：
{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": null
}
```



#### 功能实现

LogoutController

```java
package com.puc.blog.controller;

import com.puc.blog.service.LoginService;
import com.puc.blog.vo.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestHeader;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("logout")
public class LogoutController {

    @Autowired
    private LoginService loginService;

    @GetMapping
    public Result logout(@RequestHeader("Authorization") String token){

        return loginService.logout(token);
    }
}
```

LoginService接口中

```java
Result logout(String token);
```

实现类

```java
 @Override
    public Result logout(String token) {
        redisTemplate.delete("TOKEN_ " + token);
        return Result.success(null);
    }



// TODO  登陆时 需要redis server启动 才能进行登录 优化？
```





### 注册

#### 接口说明

```json
url:/register
请求方式: POST
请求参数：

参数名称	参数类型	说明
account		string		账号
password	string		密码
nickname	string		昵称
返回数据：

{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": "token"
}
```



#### 功能实现

RegisterController

```java
package com.puc.blog.controller;


import com.puc.blog.service.LoginService;
import com.puc.blog.vo.Result;
import com.puc.blog.vo.params.LoginParam;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("register")
public class RegisterController {

    @Autowired
    private LoginService loginService;

    @PostMapping
    //TODO 这里把LoginService换成注册专用的 SSO Service  后期如果把登录注册功能踢出去去 可以独立提供接口服务
    public Result register(@RequestBody LoginParam loginParam){

        return loginService.register(loginParam);
    }
}
```



LoginServiceImpl 中实现方法

```java
/**
     * 注册
     * @param loginParam
     * @return
     */
    Result register(LoginParam loginParam);
```

```java
@Override
    public Result register(LoginParam loginParam) {
        /**
         * 1.判断参数是否合法
         * 2.判断账户是否存在，存在返回账户已经被注册
         * 3.不存在 注册账户
         * 4.生成token
         * 5.存入redis
         * 6.注意 加上事务  一旦中间的任何过程出现问题 注册的用户 需要回滚  使用 @Transactional
         */
        //1.判断参数是否合法
        String account = loginParam.getAccount();
        String password = loginParam.getPassword();
        String nickname = loginParam.getNickname();
        if(StringUtils.isEmpty(account)
            || StringUtils.isEmpty(password)
            || StringUtils.isEmpty(nickname)
        ){
            return Result.fail(ErrorCode.PARAMS_ERROR.getCode(), ErrorCode.PARAMS_ERROR.getMsg());
        }
        //         * 2.判断账户是否存在，存在返回账户已经被注册
        SysUser sysUser = sysUserService.findUserByAccount(account);
        if(sysUser != null){
            return Result.fail(ErrorCode.ACCOUNT_EXIST.getCode(), ErrorCode.ACCOUNT_PWD_NOT_EXIST.getMsg());
        }
        //         * 3.不存在 注册账户
        sysUser = new SysUser();
        sysUser.setNickname(nickname);
        sysUser.setAccount(account);
        sysUser.setPassword(DigestUtils.md5Hex(password + salt));
        sysUser.setCreateDate(System.currentTimeMillis());
        sysUser.setLastLogin(System.currentTimeMillis());
        sysUser.setAvatar("/static/img/logo.b3a48c0.png");
        sysUser.setAdmin(1);
        sysUser.setDeleted(0);
        sysUser.setStatus("");
        sysUser.setSalt("");
        this.sysUserService.save(sysUser);
        String token = JWTUtils.createToken(sysUser.getId());
//        redisTemplate.opsForValue();//操作字符串
//        redisTemplate.opsForHash();//操作hash
//        redisTemplate.opsForList();//操作list
//        redisTemplate.opsForSet();//操作set
//        redisTemplate.opsForZSet();//操作有序set
        //JSON.toJSONString(sysUser) 是将 sysUser 这个HashMap类型的数据返回成json字符串
        //目前为止  只把登录过程中的 token 存到 redis 中
        redisTemplate.opsForValue().set("TOKEN_" + token, JSON.toJSONString(sysUser),1,TimeUnit.DAYS);
        return Result.success(token);
    }
```

SysUserService

```java
/**
 * 保存用户
 * @param sysUser
 */
void save(SysUser sysUser);
```

```java
@Override
    public void save(SysUser sysUser) {
        //保存用户 这id会自动生成
        //这个地方  mybatis plus 是分布式id  雪花啊算法
        sysUserMapper.insert(sysUser);
    }
```





### 登录拦截器

统一进行登录拦截 如果遇到需要登录才能访问的接口 未登录 拦截器直接返回 并跳转登录页面

#### 功能实现

LoginInterceptor

```java
package com.puc.blog.handler;

import com.alibaba.fastjson.JSON;
import com.puc.blog.dao.pojo.SysUser;
import com.puc.blog.service.LoginService;
import com.puc.blog.util.UserThreadLocal;
import com.puc.blog.vo.ErrorCode;
import com.puc.blog.vo.Result;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.method.HandlerMethod;
import org.springframework.web.servlet.HandlerInterceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;


@Component
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {

    @Autowired
    private LoginService loginService;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //在执行controller方法之前执行
        /**
         * 1.需要判断请求的接口路径 是否为 HandlerMethod
         * 2.判断 token 是否为空 如果为空 未登录
         * 3.如果 token 不为空 登录验证 loginService checkToken
         * 4.如果认证成功 放行即可
         */
        if(!(handler instanceof HandlerMethod)){
            //handler 可能是 RequestResourceHandler springboot 程序 访问静态资源 默认去classpath下的static目录去 查询
            return true;
        }
        String token = request.getHeader("Authorization");
        log.info("=================request start===========================");
        String requestURI = request.getRequestURI();
        log.info("request uri:{}",requestURI);
        log.info("request method:{}",request.getMethod());
        log.info("token:{}", token);
        log.info("=================request end===========================");

        if(StringUtils.isEmpty(token)){
            Result result = Result.fail(ErrorCode.NO_LOGIN.getCode(), "未登录");
            response.setContentType("application/json;charset=utf-8");
            response.getWriter().print(JSON.toJSONString(result));
            return false;
        }

        SysUser sysUser = loginService.checkToken(token);
        if(sysUser == null){
            Result result = Result.fail(ErrorCode.NO_LOGIN.getCode(), "未登录");
            response.setContentType("application/json;charset=utf-8");
            response.getWriter().print(JSON.toJSONString(result));
            return false;
        }
        //登录成功 放行
        //我希望在 controller 中 直接获取用户的信息 怎么获取--->> UserThreadLocal
        UserThreadLocal.put(sysUser);
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        //如果不删除ThreadLocal中用完的信息 有内存泄露的风险
        UserThreadLocal.remove();
    }
}

```



### ThreadLocal

#### 功能实现

UserThreadLocal

```java
package com.puc.blog.util;

import com.puc.blog.dao.pojo.SysUser;

public class UserThreadLocal {

    private UserThreadLocal(){}

    //线程变量隔离
    private static final ThreadLocal<SysUser> LOCAL = new ThreadLocal<>();

    public static void put(SysUser sysUser){
        LOCAL.set(sysUser);
    }

    public static SysUser get(){
        return LOCAL.get();
    }

    public static void remove(){
        LOCAL.remove();
    }

}

```



拦截器中 最后要删除 ThreadLocal 中用完的信息

```java
@Override
public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
    //如果不删除ThreadLocal中用完的信息 有内存泄露的风险
    UserThreadLocal.remove();
}
```



#### 内存泄漏原理分析

![image-20221215171315841](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20221215171315841.png)





## 4.文章

### 文章详情



#### 接口说明

```json
接口url：/articles/view/{id}

请求方式：POST

请求参数：

参数名称	参数类型		说明
id			long		文章id（路径参数）
返回数据：

{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": {
        "id": 1,
        "title": "springboot介绍以及入门案例",
        "summary": "介绍",
        "commentCounts": 0,
        "viewCounts": 60,
        "weight": 0,
        "createDate": "2609-06-26 15:58",
        "author": "李四",
        "body": {
            "content": "111"
        },
        "tags": [
            {
                "id": 5,
                "tagName": "springboot"
            },
            {
                "id": 7,
                "tagName": "springmvc"
            },
            {
                "id": 8,
                "tagName": "11"
            }
        ],
        "category": {
            "id": 2,
            "avatar": "/category/back.png",
            "categoryName": "后端"
        }
    }
}
```



#### 功能实现

ArticleController

```java
@PostMapping("view/{id}")
public Result findArticleById(@PathVariable("id") Long articleId){
     return articleService.findArticleById(articleId);
}
```



ArticleService

```java
/**
     * 查看文章详情
     * @param articleId
     * @return
     */
    Result findArticleById(Long articleId);
```



ArticleServiceImpl

```java
@Override
    public Result findArticleById(Long articleId) {
        /**
         * 1.根据Id 查询文章信息
         * 2.根据Body Id 和 categoryId 去做关联查询
         */

        Article article = articleMapper.selectById(articleId);
        ArticleVo articleVo= copy(article,true,true,true,true);
        //copy 方法需要修改
        return Result.success(articleVo);
    }
```



copy

```java
public ArticleVo copy(Article article,boolean isAuthor,boolean isBody,boolean isTags,boolean isCategory){
        ArticleVo articleVo = new ArticleVo();
        BeanUtils.copyProperties(article, articleVo);
//        articleVo.setCreateDate(new DateTime(article.getCreateDate()).toString("yyyy-MM-dd HH:mm"));  把数据库中的create_date 从Long 换成Date 不需要这行代码

        //并不是所有接口 都需要标签，作者信息
        if (isAuthor) {
            SysUser sysUser = sysUserService.findSysUserById(article.getAuthorId());
            articleVo.setAuthor(sysUser.getNickname());
        }
        if (isTags){
            List<TagVo> tags = tagsService.findTagsByArticleId(article.getId());
            articleVo.setTags(tags);
        }
        if(isBody){
            Long bodyId = article.getBodyId();
            articleVo.setBody(findArticleBodyById(bodyId));
        }
        if(isCategory){
            Long categoryId =article.getCategoryId();
            articleVo.setCategory(categoryService.finCategoryById(categoryId));
        }
        return articleVo;
    }


//------------------------------------------------------------------------------------------
//copyList 方法 重载
private List<ArticleVo> copyList(List<Article> records,boolean isAuthor,boolean isBody,boolean isTags) {
        List<ArticleVo> articleVoList = new ArrayList<>();
        for (Article article : records) {
            ArticleVo articleVo = copy(article,isAuthor,isBody,isTags,false);
            articleVoList.add(articleVo);
        }
        return articleVoList;
    }
    private List<ArticleVo> copyList(List<Article> records,boolean isAuthor,boolean isBody,boolean isTags,boolean isCategory) {
        List<ArticleVo> articleVoList = new ArrayList<>();
        for (Article article : records) {
            ArticleVo articleVo = copy(article,isAuthor,isBody,isTags,isCategory);
            articleVoList.add(articleVo);
        }
        return articleVoList;
    }
```



copy方法中所需要的方法

```java
@Autowired
    ArticleBodyMapper articleBodyMapper;

    private ArticleBodyVo findArticleBodyById(Long bodyId) {
        ArticleBody articleBody = articleBodyMapper.selectById(bodyId);
        ArticleBodyVo articleBodyVo = new ArticleBodyVo();
        articleBodyVo.setContent(articleBody.getContent());
        return articleBodyVo;
    }
```

Category 相关的业务逻辑 并不在 Article 当中 

所以单独写一个CategoryService

并将其注入到ArticleServiceImpl中

```java
public interface CategoryService {
    CategoryVo finCategoryById(Long categoryId);
}
```



CategoryServiceImpl 

```java
@Service
public class CategoryServiceImpl implements CategoryService {

    @Autowired
    private CategoryMapper categoryMapper;
    @Override
    public CategoryVo finCategoryById(Long categoryId) {
        Category category = categoryMapper.selectById(categoryId);
        CategoryVo categoryVo = new CategoryVo();
        BeanUtils.copyProperties(category,categoryVo);
        return categoryVo;
    }
}
```



CategoryMapper

```java
public interface CategoryMapper extends BaseMapper<Category> {
}
```

ArticleBodyMapper

```java
public interface ArticleBodyMapper extends BaseMapper<ArticleBody> {
}
```







### 使用线程池 更新阅读次数

#### 线程池配置

```java                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
@Configuration
@EnableAsync   // 开启多线程
public class ThreadPoolConfig {

    @Bean("taskExecutor")
    public Executor asyncServiceExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        // 设置核心线程数
        executor.setCorePoolSize(5);
        // 设置最大线程数
        executor.setMaxPoolSize(20);
        //配置队列大小
        executor.setQueueCapacity(Integer.MAX_VALUE);
        // 设置线程活跃时间（秒）
        executor.setKeepAliveSeconds(60);
        // 设置默认线程名称
        executor.setThreadNamePrefix("码神之路博客项目");
        // 等待所有任务结束后再关闭线程池
        executor.setWaitForTasksToCompleteOnShutdown(true);
        //执行初始化
        executor.initialize();
        return executor;
    }
}
```



#### 使用线程池

```java
@Autowired
    private ThreadService threadService;

    @Override
    public Result findArticleById(Long articleId) {
        /**
         * 1.根据Id 查询文章信息
         * 2.根据Body Id 和 categoryId 去做关联查询
         */

        Article article = articleMapper.selectById(articleId);
        ArticleVo articleVo= copy(article,true,true,true,true);

        //查看完文章之后 本应该直接返回数据了， 这时候做了一个更新操作，更新时加写锁，阻塞其他的读操作，性能就会比较低 没办法优化
        //更新 增加了 此次接口的耗时 一旦更新出问题不能影响查看文章的操作
        //线程池 可以把更新操作 扔到线程池中去执行  和主线程就不相关了
        threadService.updateArticleViewCount(articleMapper,article);
        return Result.success(articleVo);
    }
```

```java
@Component
public class ThreadService {

    //期望此操作在线程池执行 不会影响原有的主线程
    @Async("taskExecutor")
    public void updateArticleViewCount(ArticleMapper articleMapper, Article article) {

        int viewCounts = article.getViewCounts();
        Article articleUpdate = new Article();
        articleUpdate.setViewCounts(viewCounts + 1);
        LambdaUpdateWrapper<Article> updateWrapper = new LambdaUpdateWrapper<>();
        updateWrapper.eq(Article::getId,article.getId());
        //设置一个 为了在多线程的环境下 保障线程安全
        updateWrapper.eq(Article::getViewCounts,viewCounts);
        //update article set view_count=100 where view_count=99 and id=11
        articleMapper.update(articleUpdate,updateWrapper);

        try {
            Thread.sleep(5000);
            System.out.println("更新完成了......");
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

```



## 5.评论

### 评论列表

####  接口说明

```json
接口url：/comments/article/{id}

请求方式：GET

请求参数：

参数名称	参数类型	说明
id			long		文章id（路径参数）
返回数据：

{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": [
        {
            "id": 53,
            "author": {
                "nickname": "李四",
                "avatar": "http://localhost:8080/static/img/logo.b3a48c0.png",
                "id": 1
            },
            "content": "写的好",
            "childrens": [
                {
                    "id": 54,
                    "author": {
                        "nickname": "李四",
                        "avatar": "http://localhost:8080/static/img/logo.b3a48c0.png",
                        "id": 1
                    },
                    "content": "111",
                    "childrens": [],
                    "createDate": "1973-11-26 08:52",
                    "level": 2,
                    "toUser": {
                        "nickname": "李四",
                        "avatar": "http://localhost:8080/static/img/logo.b3a48c0.png",
                        "id": 1
                    }
                }
            ],
            "createDate": "1973-11-27 09:53",
            "level": 1,
            "toUser": null
        }
    ]
}
```





#### 功能实现

CommentsController

```java
@RestController
@RequestMapping("comments")
public class CommentsController {
    @Autowired
    private CommentsService commentsService;

    @GetMapping("article/{id}")
    public Result comments(@PathVariable("id") Long id){
        return commentsService.commentByArticleId(id);
    }

}

```

CommentsService

```java
public interface CommentsService {
    /**
     * 根据文章id 查询所有的评论列表
     * @param id
     * @return
     */
    Result commentByArticleId(Long id);
}
```

CommentsServiceImpl

```java
@Service
public class CommentsServiceImpl implements CommentsService {

    @Autowired
    private CommentMapper commentMapper;

    @Autowired
    private SysUserService sysUserService;

    @Override
    public Result commentByArticleId(Long id) {

        /**
         * 1.根据文章id 查询评论列表
         * 2.根据作责的id 查询作者的信息
         * 3.判断 如果 level = 1 要去查询他有没有子评论
         * 4.如果有 根据评论id 进行查询 （parent_id）
         */
        LambdaQueryWrapper<Comment> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(Comment::getArticleId,id);
        queryWrapper.eq(Comment::getLevel,1);

        List<Comment> comments = commentMapper.selectList(queryWrapper);
        List<CommentVo> commentVos = copyList(comments);
        return Result.success(commentVos);
    }

    private List<CommentVo> copyList(List<Comment> comments) {

        List<CommentVo> commentVos = new ArrayList<>();
        for(Comment comment :comments){
            commentVos.add(copy(comment));
        }
        return commentVos;
    }

    private CommentVo copy(Comment comment) {

        CommentVo commentVo = new CommentVo();
        BeanUtils.copyProperties(comment,commentVo);
        //作者信息
        Long authorId = comment.getAuthorId();
        UserVo userVo = this.sysUserService.findUserVoById(authorId);
        commentVo.setAuthor(userVo);
        //子评论
        Integer level = comment.getLevel();
        if(level == 1){
            Long id = comment.getId();
            List<CommentVo> commentVoList = finCommentsByParentId(id);
            commentVo.setChildrens(commentVoList);
        }
        //to User
        if(level > 1){
            Long toUid = comment.getToUid();
            UserVo toUserVo = this.sysUserService.findUserVoById(authorId);
            commentVo.setAuthor(toUserVo);
        }

        return commentVo;
    }

    private List<CommentVo> finCommentsByParentId(Long id) {
        LambdaQueryWrapper<Comment> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(Comment::getParentId,id);
        queryWrapper.eq(Comment::getLevel,2);
        return copyList(commentMapper.selectList(queryWrapper));
    }
}
```

CommentMapper

```java
public interface CommentMapper extends BaseMapper<Comment> {
}

```







### 评论

#### 接口说明

```json
url: /commetnts/create/change
POST

请求参数：

参数名称	参数类型	   说明
articleId	long		文章id
content		string		评论内容
parent		long		父评论id
toUserId	long		被评论的用户id

{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": null
}
```

#### 功能实现

Controller

```java
 @PostMapping("create/change")
    public Result comment(@RequestBody CommentParam commentParam){
        return commentsService.comment(commentParam);
    }
```

Service

```java
/**
     * 评论
     * @param commentParam
     * @return
     */
    Result comment(CommentParam commentParam);
```

ServiceImpl

```java
@Override
    public Result comment(CommentParam commentParam) {
        //通过线程池 获取现在 已经登陆用户的信息
        SysUser sysUser = UserThreadLocal.get();
        Comment comment = new Comment();
        comment.setArticleId(commentParam.getArticleId());
        comment.setAuthorId(sysUser.getId());
        comment.setContent(commentParam.getContent());
        comment.setCreateDate(System.currentTimeMillis());
        //获取这条评论 是不是子评论
        Long parent = commentParam.getParent();
        if(parent == null || parent == 0){
            comment.setLevel(1);
        } else {
            comment.setLevel(2);
        }
        comment.setParentId(parent == null ? 0 : parent);
        Long toUserId = commentParam.getToUserId();
        comment.setToUid(toUserId == null ? 0 : toUserId);
        this.commentMapper.insert(comment);
        return Result.success(null);
    }

    private List<CommentVo> copyList(List<Comment> comments) {

        List<CommentVo> commentVos = new ArrayList<>();
        for(Comment comment :comments){
            commentVos.add(copy(comment));
        }
        return commentVos;
    }

    private CommentVo copy(Comment comment) {

        CommentVo commentVo = new CommentVo();
        BeanUtils.copyProperties(comment,commentVo);
        //作者信息
        Long authorId = comment.getAuthorId();
        UserVo userVo = this.sysUserService.findUserVoById(authorId);
        commentVo.setAuthor(userVo);
        //子评论
        Integer level = comment.getLevel();
        if(level == 1){
            Long id = comment.getId();
            List<CommentVo> commentVoList = findCommentsByParentId(id);
            commentVo.setChildrens(commentVoList);
        }
        //to User
        if(level > 1){
            Long toUid = comment.getToUid();
            UserVo toUserVo = this.sysUserService.findUserVoById(authorId);
            commentVo.setAuthor(toUserVo);
        }

        return commentVo;
    }

    private List<CommentVo> findCommentsByParentId(Long id) {
        LambdaQueryWrapper<Comment> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(Comment::getParentId,id);
        queryWrapper.eq(Comment::getLevel,2);
        return copyList(commentMapper.selectList(queryWrapper));
    }
```

Mapper

```java
public interface CommentMapper extends BaseMapper<Comment> {
}
```

## BUG

这里对一条评论进行评论之后 该评论和子评论都显示不出来

但是 后端的数据是成功传到前端的

感觉是前端的问题

![image-20221217121408553](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20221217121408553.png)





## 6.写文章

### 所有文章分类

#### 接口说明

```json
接口url：/categorys

请求方式：GET

请求参数：

参数名称	参数类型	说明
返回数据：

{
    "success":true,
 	"code":200,
    "msg":"success",
    "data":
    [
        {"id":1,"avatar":"/category/front.png","categoryName":"前端"},	
        {"id":2,"avatar":"/category/back.png","categoryName":"后端"},
        {"id":3,"avatar":"/category/lift.jpg","categoryName":"生活"},
        {"id":4,"avatar":"/category/database.png","categoryName":"数据库"},
        {"id":5,"avatar":"/category/language.png","categoryName":"编程语言"}
    ]
}

```



#### 功能实现

Controller

```java
@RestController
@RequestMapping("categorys")
public class CategoryController {

    @Autowired
    CategoryService categoryService;

    @GetMapping
    public Result categories(){
        return categoryService.findAll();
    }

}
```

Service

```java
public interface CategoryService {
    CategoryVo finCategoryById(Long categoryId);

    Result findAll();
}
```



ServiceImpl

```java
@Override
    public Result findAll() {
        List<Category> categories = categoryMapper.selectList(new LambdaQueryWrapper<>());
        return Result.success(copyList(categories));
    }
	public CategoryVo copy(Category category){
        CategoryVo categoryVo = new CategoryVo();
        BeanUtils.copyProperties(category,categoryVo);
        return categoryVo;
    }
    public List<CategoryVo> copyList(List<Category> categoryList){
        List<CategoryVo> categoryVoList = new ArrayList<>();
        for (Category category : categoryList) {
            categoryVoList.add(copy(category));
        }
        return categoryVoList;
    }
```



### 所有文章标签

#### 接口说明

```java
接口url：/tags

请求方式：GET

请求参数：

参数名称	参数类型	说明
返回数据：

{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": [
        {
            "id": 5,
            "tagName": "springboot"
        },
        {
            "id": 6,
            "tagName": "spring"
        },
        {
            "id": 7,
            "tagName": "springmvc"
        },
        {
            "id": 8,
            "tagName": "11"
        }
    ]
}
```



#### 功能实现

Controller

```java
@GetMapping
    public Result findAll(){
        return tagsService.findAll();
    }
```

Service

```java
Result findAll();
```

ServiceImpl

```java
@Override
    public Result findAll() {
        List<Tag> tags = this.tagMapper.selectList(new LambdaQueryWrapper<>());
        return Result.success(copyList(tags));
    }

    public TagVo copy(Tag tag){
        TagVo tagVo = new TagVo();
        BeanUtils.copyProperties(tag,tagVo);
        return tagVo;
    }
    public List<TagVo> copyList(List<Tag> tagList){
        List<TagVo> tagVoList = new ArrayList<>();
        for (Tag tag : tagList) {
            tagVoList.add(copy(tag));
        }
        return tagVoList;
    }
```



### 发布文章

#### 接口说明

```json
url：/articles/publish
POST
请求参数：

参数名称	参数类型													  说明
title	   string													    文章标题
id		   long														 文章id（编辑有值）
body	   object（{content: "ww", contentHtml: "ww↵"}）					 文章内容
category   {id: 2, avatar: "/category/back.png", categoryName: "后端"}	   文章类别
summary	   string														文章概述
tags	   [{id: 5}, {id: 6}]	                                             文章标签

返回数据：

{
    "success": true,
    "code": 200,
    "msg": "success",
    "data": {"id":12232323}
}

```



#### 功能实现

Param

```java
package com.puc.blog.vo.params;

import com.puc.blog.vo.CategoryVo;
import com.puc.blog.vo.TagVo;
import lombok.Data;

import java.util.List;

@Data
public class ArticleParam {

    private Long id;

    private ArticleBodyParam body;

    private CategoryVo category;

    private String summary;

    private List<TagVo> tags;

    private String title;
}
```

```java
package com.puc.blog.vo.params;

import lombok.Data;

@Data
public class ArticleBodyParam {

    private String content;

    private String contentHtml;

}
```

Controller

```java
@PostMapping("publish")
    public Result publish(@RequestBody ArticleParam articleParam){
        return articleService.publish(articleParam);
    }
```

Service

```java
/**
     * 文章发布服务
     * @param articleParam
     * @return
     */
    Result publish(ArticleParam articleParam);
```

ServiceImpl

```java
@Override
    @Transactional
    public Result publish(ArticleParam articleParam) {
        /**
         * 1.发布文章 目的是构建article对象
         * 2.作者id 当前的登录用户
         * 3.标签 要将标签加入到关联列表当中
         * 4.body 内容存储
         */
        SysUser sysUser = UserThreadLocal.get();
        Article article = new Article();
        article.setAuthorId(sysUser.getId());
        article.setWeight(Article.Article_Common);
        article.setViewCounts(0);
        article.setTitle(articleParam.getTitle());
        article.setSummary(articleParam.getSummary());
        article.setCommentCounts(0);
        article.setCreateDate(new Date());
        article.setCategoryId(articleParam.getCategory().getId());
        //插入之后会生成一个文章id
        this.articleMapper.insert(article);
        //tag
        List<TagVo> tags = articleParam.getTags();
        if(tags != null){
            for(TagVo tag : tags){
                Long articleId = article.getId();
                ArticleTag articleTag = new ArticleTag();
                articleTag.setTagId(tag.getId());
                articleTag.setArticleId(articleId);
                articleTagMapper.insert(articleTag);
            }
        }
        //body
        ArticleBody articleBody = new ArticleBody();
        articleBody.setArticleId(article.getId());
        articleBody.setContent(articleParam.getBody().getContent());
        articleBody.setContentHtml(articleParam.getBody().getContentHtml());
        articleBodyMapper.insert(articleBody);

        article.setBodyId(articleBody.getId());
        articleMapper.updateById(article);
        Map<String,String> map = new HashMap<>();
        map.put("id",article.getId().toString());
        return Result.success(map);
    }
```



### AOP日志

注解

```java
import java.lang.annotation.*;

@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface LogAnnotation {

    String module() default "";

    String operation() default "";

}
```

Aspect

```java
@Component
@Aspect
@Slf4j
public class LogAspect {
    @Pointcut("@annotation(com.puc.blog.common.aop.LogAnnotation)")
    public void pt(){}

    @Around("pt()")
    public Object log(ProceedingJoinPoint joinPoint) throws Throwable{
        long beginTime = System.currentTimeMillis();
        //执行方法
        Object result = joinPoint.proceed();
        //执行时间
        long time = System.currentTimeMillis() - beginTime;

        //保存日志
        recordLog(joinPoint,time);
        return result;
    }

    private void recordLog(ProceedingJoinPoint joinPoint, long time) {
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();
        Method method = signature.getMethod();
        LogAnnotation logAnnotation = method.getAnnotation(LogAnnotation.class);
        log.info("=====================log start================================");
        log.info("module:{}",logAnnotation.module());
        log.info("operation:{}",logAnnotation.operation());

        //请求的方法名
        String className = joinPoint.getTarget().getClass().getName();
        String methodName = signature.getName();
        log.info("request method:{}",className + "." + methodName + "()");

        //请求的参数
        Object[] args = joinPoint.getArgs();
        String params = JSON.toJSONString(args[0]);
        log.info("params:{}",params);

        //获取request 设置IP地址
        HttpServletRequest request = HttpContextUtils.getHttpServletRequest();
        log.info("ip:{}", IPUtils.getIpAddr(request));


        log.info("excute time : {} ms",time);
        log.info("=====================log end================================");
    }

}
```





## 7.图片上传+分类



### 文章图片上传

#### 接口说明

```json
接口url：/upload

请求方式：POST

请求参数：

参数名称	参数类型	说明
image		file	上传的文件名称
返回数据：

{
    "success":true,
 	"code":200,
    "msg":"success",
    "data":"https://static.mszlu.com/aa.png"
}
```

#### 功能实现

Controller

```java
@RestController
@RequestMapping("upload")
public class UploadController {

    @Autowired
    private QiniuUtils qiniuUtils;

    @PostMapping
    public Result upload(@RequestParam("image")MultipartFile file){
        //原始文件名称
        String originalFilename = file.getOriginalFilename();
        //唯一文件名称
        String fileName = UUID.randomUUID().toString() + "." + StringUtils.substringAfterLast(originalFilename, ".");
        //上传文件 上传到那呢
        boolean upload = qiniuUtils.upload(file, fileName);
        if(upload){
            return Result.success(QiniuUtils.url + fileName);
        }
        return Result.fail(20001,"上传失败");
    }
}
```

QiniuUtils

```java
@Component
//需要修改四个地方 两个密匙 url 以及 bucket（设置为自己的空间名） 如果申请空间不是华北地区还需要对应改Configuration(Region.huabei())

public class QiniuUtils {

    public static final String url = "http://rn4qlnwf8.hn-bkt.clouddn.com/";

    @Value("${qiniu.accessKey}")
    private  String accessKey;
    @Value("${qiniu.accessSecretKey}")
    private  String accessSecretKey;

    public boolean upload(MultipartFile file,String fileName){

        //构造一个带指定 Region 对象的配置类
        Configuration cfg = new Configuration(Region.huanan());
        //...其他参数参考类注释
        UploadManager uploadManager = new UploadManager(cfg);
        //...生成上传凭证，然后准备上传
        String bucket = "puc";
        //默认不指定key的情况下，以文件内容的hash值作为文件名
        try {
            byte[] uploadBytes = file.getBytes();
            Auth auth = Auth.create(accessKey, accessSecretKey);
            String upToken = auth.uploadToken(bucket);
            Response response = uploadManager.put(uploadBytes, fileName, upToken);
            //解析上传成功的结果
            DefaultPutRet putRet = JSON.parseObject(response.bodyString(), DefaultPutRet.class);
            return true;
        } catch (Exception ex) {
            ex.printStackTrace();
        }
        return false;
    }
}
```

配置文件

```properties
## 上传文件总的最大值
spring.servlet.multipart.max-request-size=20MB
## 单个文件的最大值
spring.servlet.multipart.max-file-size=2MB

qiniu.accessKey=n_u7CaMdJnIvCO8A34ko84q_ZyZNrrDTYFmFb2mW
qiniu.accessSecretKey=AlyxKMheYQeGih-r2GNLDE3fmLlFffGg19fXkuHL
```

依赖

```xml
<dependency>
            <groupId>com.qiniu</groupId>
            <artifactId>qiniu-java-sdk</artifactId>
            <version>[7.7.0, 7.10.99]</version>
        </dependency>
```









### 导航 文章分类

#### 接口说明

```json
url:/categorys/detail
GET
请求参数：
无
返回数据：

{
    "success": true, 
    "code": 200, 
    "msg": "success", 
    "data": [
        {
            "id": 1, 
            "avatar": "/static/category/front.png", 
            "categoryName": "前端", 
            "description": "前端是什么，大前端"
        }, 
        {
            "id": 2, 
            "avatar": "/static/category/back.png", 
            "categoryName": "后端", 
            "description": "后端最牛叉"
        }, 
        {
            "id": 3, 
            "avatar": "/static/category/lift.jpg", 
            "categoryName": "生活", 
            "description": "生活趣事"
        }, 
        {
            "id": 4, 
            "avatar": "/static/category/database.png", 
            "categoryName": "数据库", 
            "description": "没数据库，啥也不管用"
        }, 
        {
            "id": 5, 
            "avatar": "/static/category/language.png", 
            "categoryName": "编程语言", 
            "description": "好多语言，该学哪个？"
        }
    ]
}
```

#### 功能实现

##### **查询所有的文章分类**

Controller

```java
@GetMapping("detail")
    public Result categoriesDetail(){
        return categoryService.findAllDetail();
    }
```

Service

```java
@Override
    public Result findAllDetail() {
        List<Category> categories = categoryMapper.selectList(new LambdaQueryWrapper<>());
        //页面交互的对象
        return Result.success(copyList(categories));
    }
```





##### **查询所有的标签分类**

接口说明

```json
接口url：/tags/detail

请求方式：GET
{
    "success": true, 
    "code": 200, 
    "msg": "success", 
    "data": [
        {
            "id": 5, 
            "tagName": "springboot", 
            "avatar": "/static/tag/java.png"
        }, 
        {
            "id": 6, 
            "tagName": "spring", 
            "avatar": "/static/tag/java.png"
        }, 
        {
            "id": 7, 
            "tagName": "springmvc", 
            "avatar": "/static/tag/java.png"
        }, 
        {
            "id": 8, 
            "tagName": "11", 
            "avatar": "/static/tag/css.png"
        }
    ]
}
```



功能实现

Controller

```java
@GetMapping("detail")
    public Result findAllDetail(){
        return tagService.findAllDetail();
    }
```

Service

```java
@Override
    public Result findAllDetail() {
        List<Tag> tags = this.tagMapper.selectList(new LambdaQueryWrapper<Tag>());
        return Result.success(copyList(tags));
    }
```





### 分类文章列表

#### 接口说明

```json
接口url：/category/detail/{id}

请求方式：GET

请求参数：

参数名称	参数类型	说明
id			分类id	路径参数
返回数据：

{
    "success": true, 
    "code": 200, 
    "msg": "success", 
    "data": 
        {
            "id": 1, 
            "avatar": "/static/category/front.png", 
            "categoryName": "前端", 
            "description": "前端是什么，大前端"
        }
}
```

#### 功能实现



Controller

```java
@GetMapping("detail/{id}")
    public Result categoriesDetailById(@PathVariable("id") Long id){
        return categoryService.categoriesDetailById(id);
    }
```



Service

```java
@Override
    public Result categoriesDetailById(Long id) {
        Category category = categoryMapper.selectById(id);
        CategoryVo categoryVo = copy(category);
        return Result.success(categoryVo);
    }
```



```
//查询文章的参数 加上分类id，判断不为空 加上分类条件  
if (pageParams.getCategoryId() != null) {
            queryWrapper.eq(Article::getCategoryId,pageParams.getCategoryId());
        }
```

PageParam修改

```java
package com.mszlu.blog.vo.params;

import lombok.Data;

@Data
public class PageParams {

    private int page = 1;

    private int pageSize = 10;

    private Long categoryId;

    private Long tagId;
}
```



### 标签文章列表

#### 接口说明

```java
接口url：/tags/detail/{id}

请求方式：GET

请求参数：

参数名称	参数类型	说明
id			标签id	路径参数
返回数据：

{
    "success": true, 
    "code": 200, 
    "msg": "success", 
    "data": 
        {
            "id": 5, 
            "tagName": "springboot", 
            "avatar": "/static/tag/java.png"
        }
}

```



#### 功能实现



Controller

```java
@GetMapping("detail/{id}")
    public Result findDetailById(@PathVariable("id") Long id){
        return tagsService.findDetailById(id);
    }
```

Service

```java
@Override
    public Result findDetailById(Long id) {
        Tag tag = tagMapper.selectById(id);
        return Result.success(copy(tag));
    }
```





### 归档文章列表

#### 接口说明

```java
接口url：/articles

请求方式：POST

请求参数：

参数名称	参数类型	说明
year		string		年
month		string		月
返回数据：

{
    "success": true, 
    "code": 200, 
    "msg": "success", 
    "data": [文章列表，数据同之前的文章列表接口]
        
}
```





#### 功能实现                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

Params修改

```java
package com.mszlu.blog.vo.params;

import lombok.Data;

@Data
public class PageParams {

    private int page = 1;

    private int pageSize = 10;

    private Long categoryId;

    private Long tagId;

    private String year;

    private String month;

    public String getMonth(){
        if (this.month != null && this.month.length() == 1){
            return "0"+this.month;
        }
        return this.month;
    }
}
```

因为要单独使用 creat_date中的 year 和 month  mybatis_plus不行 

所以使用自定义sql实现文章列表

```java
@Override
    public Result listArticle(PageParams pageParams) {
        Page<Article> page = new Page<>(pageParams.getPage(),pageParams.getPageSize());
        IPage<Article> articleIPage = this.articleMapper.listArticle(page,pageParams.getCategoryId(),pageParams.getTagId(),pageParams.getYear(),pageParams.getMonth());
        return Result.success(copyList(articleIPage.getRecords(),true,true));
    }
```

ArticleMapper

```java
IPage<Article> listArticle(Page<Article> page, Long categoryId, Long tagId, String year, String month);
```

ArticleMapper.xml

```xml
<resultMap id="articleMap" type="com.mszlu.blog.dao.pojo.Article">
        <id column="id" property="id" />
        <result column="author_id" property="authorId"/>
        <result column="comment_counts" property="commentCounts"/>
        <result column="create_date" property="createDate"/>
        <result column="summary" property="summary"/>
        <result column="title" property="title"/>
        <result column="view_counts" property="viewCounts"/>
        <result column="weight" property="weight"/>
        <result column="body_id" property="bodyId"/>
        <result column="category_id" property="categoryId"/>
    </resultMap>
    
    <select id="listArticle"  resultMap="articleMap">
        select * from ms_article
        <where>
            1 = 1
            <if test="categoryId != null">
                and  category_id = #{categoryId}
            </if>
            <!-- 这里数据库中的create_date类型是Date 不是Long  所以要自己修改-->
            <if test="year != null and year.length>0 and month != null and month.length>0">
                and ( YEAR(create_date) = #{year} and Month(create_date) = #{month} )
            </if>
            <if test="tagId != null">
                and id in (select article_id from ms_article_tag where tag_id=#{tagId})
            </if>
        </where>
        order by weight desc,create_date desc
    </select>
```





### 统一缓存处理（优化）

内存的访问速度 远远大于 磁盘的访问速度（1000倍起）

注解

```java
package com.puc.blog.common.cache;


import java.lang.annotation.*;

@Target({ElementType.METHOD, ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Cache {
    long expire() default 1 * 60 * 1000;
    //缓存表示 key
    String name() default "";
}

```

切面   就当工具类使用

```java
package com.puc.blog.common.cache;


import com.alibaba.fastjson.JSON;
import com.puc.blog.vo.Result;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.codec.digest.DigestUtils;
import org.apache.commons.lang3.StringUtils;
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Pointcut;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.lang.reflect.Method;
import java.time.Duration;

//aop  定义一个切面 切面定义了切点和通知的关系
@Component
@Slf4j
@Aspect
public class CacheAspect {

    @Autowired
    private RedisTemplate<String,String> redisTemplate;

    @Pointcut("@annotation(com.puc.blog.common.cache.Cache)")
    public void pt(){}

    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp){
        try {
            Signature signature = pjp.getSignature();
            //类名
            String className = pjp.getTarget().getClass().getSimpleName();
            //调用的方法名
            String methodName = signature.getName();


            Class[] parameterTypes = new Class[pjp.getArgs().length];
            Object[] args = pjp.getArgs();
            //参数
            String params = "";
            for(int i=0; i<args.length; i++) {
                if(args[i] != null) {
                    params += JSON.toJSONString(args[i]);
                    parameterTypes[i] = args[i].getClass();
                }else {
                    parameterTypes[i] = null;
                }
            }
            if (StringUtils.isNotEmpty(params)) {
                //加密 以防出现key过长以及字符转义获取不到的情况
                params = DigestUtils.md5Hex(params);
            }
            Method method = pjp.getSignature().getDeclaringType().getMethod(methodName, parameterTypes);
            //获取Cache注解
            Cache annotation = method.getAnnotation(Cache.class);
            //缓存过期时间
            long expire = annotation.expire();
            //缓存名称
            String name = annotation.name();
            //先从redis获取
            String redisKey = name + "::" + className+"::"+methodName+"::"+params;
            String redisValue = redisTemplate.opsForValue().get(redisKey);
            if (StringUtils.isNotEmpty(redisValue)){
                log.info("走了缓存~~~,{},{}",className,methodName);
                return JSON.parseObject(redisValue, Result.class);
            }
            Object proceed = pjp.proceed();
            redisTemplate.opsForValue().set(redisKey,JSON.toJSONString(proceed), Duration.ofMillis(expire));
            log.info("存入缓存~~~ {},{}",className,methodName);
            return proceed;
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
        return Result.fail(-999,"系统错误");
    }


}

```

使用

```java
/**
     * 首页 文章列表
     * @param pageParams
     * @return
     */
    @PostMapping
    //加上此注解 代表要对此接口记录日志
    @LogAnnotation(module="文章",operation="获取文章列表")
    @Cache(expire = 5 * 60 * 1000, name = "listArticle")
    public Result listArticle(@RequestBody PageParams pageParams){
        return articleService.listArticle(pageParams);
    }
```



### 思考别的优化

1.文章可以放入es当中，便于后续中文分词搜索 、

2.评论数据 可以讨论放入mongodb当中 电商系统当中 评论数据放入mongodb

3.阅读数 和评论数 考虑把增加的时候放入redis  incr自增 使用定时任务 把数据固化到数据库当中

4.为了加快访问速度 部署的时候 可以把图片 jss，css 放入七牛云存储中 加快网站访问速度

做一个后台 用springsecurity 做一个权限系统 对工作帮助较大



## 







## 管理后台

## [#](https://www.mszlu.com/java/blog/10/10.html#_1-搭建项目)1. 搭建项目

### [#](https://www.mszlu.com/java/blog/10/10.html#_1-1-新建maven工程-blog-admin)1.1 新建maven工程 blog-admin

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>blog-parent2</artifactId>
        <groupId>com.mszlu</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>blog-admin</artifactId>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
            <!-- 排除 默认使用的logback  -->
            <exclusions>
                <exclusion>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-starter-logging</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <!-- log4j2 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-log4j2</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-aop</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-mail</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.76</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>

        <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
        </dependency>

        <dependency>
            <groupId>commons-collections</groupId>
            <artifactId>commons-collections</artifactId>
            <version>3.2.2</version>
        </dependency>
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.3</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
        <dependency>
            <groupId>joda-time</groupId>
            <artifactId>joda-time</artifactId>
            <version>2.10.10</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
    </dependencies>
</project>
```

### [#](https://www.mszlu.com/java/blog/10/10.html#_1-2-配置)1.2 配置

application.properties:

```properties
server.port=8889
spring.application.name=mszlu_admin_blog

##数据库的配置
## datasource
spring.datasource.url=jdbc:mysql://localhost:3306/blog?useUnicode=true&characterEncoding=UTF-8&serverTimeZone=UTC
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

##mybatis-plus
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
mybatis-plus.global-config.db-config.table-prefix=ms_

## 上传文件总的最大值
spring.servlet.multipart.max-request-size=20MB
## 单个文件的最大值
spring.servlet.multipart.max-file-size=2MB
```

mybatis-plus配置：

```java
package com.mszlu.blog.admin.config;

import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("com.mszlu.blog.admin.mapper")
public class MybatisPlusConfig {

    //分页插件
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
        return interceptor;
    }
}
```

### [#](https://www.mszlu.com/java/blog/10/10.html#_1-3-启动类)1.3 启动类

```java
package com.mszlu.blog.admin;

import com.alibaba.fastjson.annotation.JSONField;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class AdminApp {

    public static void main(String[] args) {
        SpringApplication.run(AdminApp.class,args);
    }
}
```



### [#](https://www.mszlu.com/java/blog/10/10.html#_1-4-导入前端工程)1.4 导入前端工程

放入resources下的static目录中，前端工程在资料中有

### [#](https://www.mszlu.com/java/blog/10/10.html#_1-5-新建表)1.5 新建表

```sql
CREATE TABLE `blog`.`ms_admin`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `username` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `password` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;
```



```sql
CREATE TABLE `blog`.`ms_permission`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `path` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  `description` varchar(255) CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;
```



```sql
CREATE TABLE `blog`.`ms_admin_permission`  (
  `id` bigint(0) NOT NULL AUTO_INCREMENT,
  `admin_id` bigint(0) NOT NULL,
  `permission_id` bigint(0) NOT NULL,
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci ROW_FORMAT = Dynamic;
```

## [#](https://www.mszlu.com/java/blog/10/10.html#_2-权限管理)2. 权限管理

### [#](https://www.mszlu.com/java/blog/10/10.html#_2-1-controller)2.1 Controller

```java
package com.mszlu.blog.admin.controller;

import com.mszlu.blog.admin.model.params.PageParam;
import com.mszlu.blog.admin.pojo.Permission;
import com.mszlu.blog.admin.service.PermissionService;
import com.mszlu.blog.admin.vo.Result;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("admin")
public class AdminController {

    @Autowired
    private PermissionService permissionService;

    @PostMapping("permission/permissionList")
    public Result permissionList(@RequestBody PageParam pageParam){
        return permissionService.listPermission(pageParam);
    }

    @PostMapping("permission/add")
    public Result add(@RequestBody Permission permission){
        return permissionService.add(permission);
    }

    @PostMapping("permission/update")
    public Result update(@RequestBody Permission permission){
        return permissionService.update(permission);
    }

    @GetMapping("permission/delete/{id}")
    public Result delete(@PathVariable("id") Long id){
        return permissionService.delete(id);
    }
}
```

```java
package com.mszlu.blog.admin.model.params;

import lombok.Data;

@Data
public class PageParam {

    private Integer currentPage;

    private Integer pageSize;

    private String queryString;
}
```

```java
package com.mszlu.blog.admin.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.Data;

@Data
public class Permission {

    @TableId(type = IdType.AUTO)
    private Long id;

    private String name;

    private String path;

    private String description;
}
```

### [#](https://www.mszlu.com/java/blog/10/10.html#_2-2-service)2.2 Service

```java
package com.mszlu.blog.admin.service;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.mszlu.blog.admin.mapper.PermissionMapper;
import com.mszlu.blog.admin.model.params.PageParam;
import com.mszlu.blog.admin.pojo.Permission;
import com.mszlu.blog.admin.vo.PageResult;
import com.mszlu.blog.admin.vo.Result;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class PermissionService {

    @Autowired
    private PermissionMapper permissionMapper;

    public Result listPermission(PageParam pageParam){
        Page<Permission> page = new Page<>(pageParam.getCurrentPage(),pageParam.getPageSize());
        LambdaQueryWrapper<Permission> queryWrapper = new LambdaQueryWrapper<>();
        if (StringUtils.isNotBlank(pageParam.getQueryString())) {
            queryWrapper.eq(Permission::getName,pageParam.getQueryString());
        }
        Page<Permission> permissionPage = this.permissionMapper.selectPage(page, queryWrapper);
        PageResult<Permission> pageResult = new PageResult<>();
        pageResult.setList(permissionPage.getRecords());
        pageResult.setTotal(permissionPage.getTotal());
        return Result.success(pageResult);
    }

    public Result add(Permission permission) {
        this.permissionMapper.insert(permission);
        return Result.success(null);
    }

    public Result update(Permission permission) {
        this.permissionMapper.updateById(permission);
        return Result.success(null);
    }

    public Result delete(Long id) {
        this.permissionMapper.deleteById(id);
        return Result.success(null);
    }
}
```

```java
package com.mszlu.blog.admin.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.mszlu.blog.admin.pojo.Permission;

import java.util.List;

public interface PermissionMapper extends BaseMapper<Permission> {

    
}
```



```java
package com.mszlu.blog.admin.vo;

import lombok.AllArgsConstructor;
import lombok.Data;

@Data
@AllArgsConstructor
public class Result {

    private boolean success;

    private int code;

    private String msg;

    private Object data;


    public static Result success(Object data){
        return new Result(true,200,"success",data);
    }

    public static Result fail(int code, String msg){
        return new Result(false,code,msg,null);
    }
}
```

```java
package com.mszlu.blog.admin.vo;

import lombok.Data;

import java.util.List;

@Data
public class PageResult<T> {

    private List<T> list;

    private Long total;
}
```

### [#](https://www.mszlu.com/java/blog/10/10.html#_2-3-测试)2.3 测试

## [#](https://www.mszlu.com/java/blog/10/10.html#_3-security集成)3. Security集成

### [#](https://www.mszlu.com/java/blog/10/10.html#_3-1-添加依赖)3.1 添加依赖

```xml
 <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
```

### [#](https://www.mszlu.com/java/blog/10/10.html#_3-2-配置)3.2 配置

```java
package com.mszlu.blog.admin.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
import org.springframework.security.config.annotation.method.configuration.EnableGlobalMethodSecurity;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.builders.WebSecurity;
import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.web.authentication.AuthenticationFailureHandler;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.security.web.authentication.logout.LogoutSuccessHandler;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@Configuration
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder(){
        return new BCryptPasswordEncoder();
    }

    public static void main(String[] args) {
        //加密策略 MD5 不安全 彩虹表  MD5 加盐
        String mszlu = new BCryptPasswordEncoder().encode("mszlu");
        System.out.println(mszlu);
    }
    @Override
    public void configure(WebSecurity web) throws Exception {
        super.configure(web);
    }
    @Override
    protected void configure(HttpSecurity http) throws Exception {

        http.authorizeRequests() //开启登录认证
//                .antMatchers("/user/findAll").hasRole("admin") //访问接口需要admin的角色
                .antMatchers("/css/**").permitAll()
                .antMatchers("/img/**").permitAll()
                .antMatchers("/js/**").permitAll()
                .antMatchers("/plugins/**").permitAll()
                .antMatchers("/admin/**").access("@authService.auth(request,authentication)") //自定义service 来去实现实时的权限认证
                .antMatchers("/pages/**").authenticated()
                .and().formLogin()
                .loginPage("/login.html") //自定义的登录页面
                .loginProcessingUrl("/login") //登录处理接口
                .usernameParameter("username") //定义登录时的用户名的key 默认为username
                .passwordParameter("password") //定义登录时的密码key，默认是password
                .defaultSuccessUrl("/pages/main.html")
                .failureUrl("/login.html")
                .permitAll() //通过 不拦截，更加前面配的路径决定，这是指和登录表单相关的接口 都通过
                .and().logout() //退出登录配置
                .logoutUrl("/logout") //退出登录接口
                .logoutSuccessUrl("/login.html")
                .permitAll() //退出登录的接口放行
                .and()
                .httpBasic()
                .and()
                .csrf().disable() //csrf关闭 如果自定义登录 需要关闭
                .headers().frameOptions().sameOrigin();
    }
}
```



### [#](https://www.mszlu.com/java/blog/10/10.html#_3-3-登录认证)3.3 登录认证

```java
package com.mszlu.blog.admin.pojo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.annotation.TableId;
import lombok.Data;

@Data
public class Admin {

    @TableId(type = IdType.AUTO)
    private Long id;

    private String username;

    private String password;
}
```

```java
package com.mszlu.blog.admin.service;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.mszlu.blog.admin.mapper.AdminMapper;
import com.mszlu.blog.admin.pojo.Admin;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.core.userdetails.UsernameNotFoundException;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.stereotype.Component;

import java.util.ArrayList;

@Component
@Slf4j
public class SecurityUserService implements UserDetailsService {
    @Autowired
    private AdminService adminService;
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        log.info("username:{}",username);
        //当用户登录的时候，springSecurity 就会将请求 转发到此
        //根据用户名 查找用户，不存在 抛出异常，存在 将用户名，密码，授权列表 组装成springSecurity的User对象 并返回
        Admin adminUser = adminService.findAdminByUserName(username);
        if (adminUser == null){
            throw new UsernameNotFoundException("用户名不存在");
        }
        ArrayList<GrantedAuthority> authorities = new ArrayList<>();
        UserDetails userDetails = new User(username,adminUser.getPassword(), authorities);
        //剩下的认证 就由框架帮我们完成
        return userDetails;
    }

    public static void main(String[] args) {
        System.out.println(new BCryptPasswordEncoder().encode("123456"));
    }
}
```



```java
package com.mszlu.blog.admin.service;

import com.baomidou.mybatisplus.core.conditions.query.LambdaQueryWrapper;
import com.mszlu.blog.admin.mapper.AdminMapper;
import com.mszlu.blog.admin.mapper.PermissionMapper;
import com.mszlu.blog.admin.pojo.Admin;
import com.mszlu.blog.admin.pojo.Permission;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class AdminService {

    @Autowired
    private AdminMapper adminMapper;
    @Autowired
    private PermissionMapper permissionMapper;

    public Admin findAdminByUserName(String username){
        LambdaQueryWrapper<Admin> queryWrapper = new LambdaQueryWrapper<>();
        queryWrapper.eq(Admin::getUsername,username).last("limit 1");
        Admin adminUser = adminMapper.selectOne(queryWrapper);
        return adminUser;
    }

    public List<Permission> findPermissionsByAdminId(Long adminId){
        return permissionMapper.findPermissionsByAdminId(adminId);
    }

}
```



```java
package com.mszlu.blog.admin.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.mszlu.blog.admin.pojo.Admin;

public interface AdminMapper extends BaseMapper<Admin> {
}
```



```java
package com.mszlu.blog.admin.mapper;

import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.mszlu.blog.admin.pojo.Permission;

import java.util.List;

public interface PermissionMapper extends BaseMapper<Permission> {

    List<Permission> findPermissionsByAdminId(Long adminId);
}
```



### [#](https://www.mszlu.com/java/blog/10/10.html#_3-4-权限认证)3.4 权限认证

```java
package com.mszlu.blog.admin.service;

import com.mszlu.blog.admin.mapper.AdminMapper;
import com.mszlu.blog.admin.pojo.Admin;
import com.mszlu.blog.admin.pojo.Permission;
import lombok.extern.slf4j.Slf4j;
import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Service;

import javax.servlet.http.HttpServletRequest;
import java.util.Collection;
import java.util.List;

@Service
@Slf4j
public class AuthService {

    @Autowired
    private AdminService adminService;

    public boolean auth(HttpServletRequest request, Authentication authentication){
        String requestURI = request.getRequestURI();
        log.info("request url:{}", requestURI);
        //true代表放行 false 代表拦截
        Object principal = authentication.getPrincipal();
        if (principal == null || "anonymousUser".equals(principal)){
            //未登录
            return false;
        }
        UserDetails userDetails = (UserDetails) principal;
        String username = userDetails.getUsername();
        Admin admin = adminService.findAdminByUserName(username);
        if (admin == null){
            return false;
        }
        if (admin.getId() == 1){
            //认为是超级管理员
            return true;
        }
        List<Permission> permissions = adminService.findPermissionsByAdminId(admin.getId());
        requestURI = StringUtils.split(requestURI,'?')[0];
        for (Permission permission : permissions) {
            if (requestURI.equals(permission.getPath())){
                log.info("权限通过");
                return true;
            }
        }
        return false;
    }
}
```



```java
package com.mszlu.blog.admin.service;

import org.springframework.security.core.GrantedAuthority;

public class MySimpleGrantedAuthority implements GrantedAuthority {
    private String authority;
    private String path;

    public MySimpleGrantedAuthority(){}

    public MySimpleGrantedAuthority(String authority){
        this.authority = authority;
    }

    public MySimpleGrantedAuthority(String authority,String path){
        this.authority = authority;
        this.path = path;
    }

    @Override
    public String getAuthority() {
        return authority;
    }

    public String getPath() {
        return path;
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--MyBatis配置文件-->
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.mszlu.blog.admin.mapper.PermissionMapper">

    <select id="findPermissionsByAdminId" parameterType="long" resultType="com.mszlu.blog.admin.pojo.Permission">
        select * from ms_permission where id in (select permission_id from ms_admin_permission where admin_id=##{adminId})
    </select>
</mapper>
```

## 5. 总结

1. jwt + redis

   token令牌的登录方式，访问认证速度快，session共享，安全性

   redis做了 令牌和 用户信息的对应管理，1. 进一步增加了安全性 2. 登录用户做了缓存 3. 灵活控制用户的过期（续期，踢掉线等）

2. threadLocal 使用了保存用户信息，请求的线程之内，可以随时获取登录的用户，做了线程隔离

3. 在使用完ThreadLocal之后，做了value的删除，防止了内存泄漏

4. 线程安全- update table set value = newValue where id=1 and value=oldValue

5. 线程池 应用非常广，面试 7个核心参数 （对当前的主业务流程 无影响的操作，放入线程池执行）

   1. 登录 ，记录日志

6. 权限系统 重点内容

7. 统一日志记录，统一缓存处理

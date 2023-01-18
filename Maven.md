# **Maven**

## 	功能

​        1.管理jar文件
 	   2.管理jar的直接依赖
​	    3.管理jar版本
​		4.编译程序  测试 打包 和 部署

## 	核心概念

### 			1.pom

​			一个文件，项目对象模型，可以控制maven构建项目的过程，管理jar依赖

​			modelVersion: Maven模型的版本

​			groupId:  组织id 一般是公司域名的倒写

​			artifactId: 自定义的项目名称 

​			version: 自定义的版本号

### 			2.约定的目录结构

### 			3.坐标(gav)

​			是一个唯一的字符串，用来表示资源的,由上面说的三个值构成groupId，artifactId，version

### 			4.依赖管理

​			管理你的项目可以使用jar文件

​			相当于是java中的import

​			<dependencies>

### 			5.仓库管理

​	本地仓库

​	远程仓库：又分中央仓库 ， 中央仓库的镜像 和私服（局域网中使用）

​	仓库的使用不需要人为参与

### 			6.生命周期

​	单元测试：用的是junit，一个专门测试的框架，测试的是类中的方法

​	使用步骤：1.加入依赖，在pom.xml加入单元测试依赖 (要先搜索 找到junit的gav)

​					   2.在maven项目中的src/test/java目录下，创建测试程序

​	maven命令：

​	mvn compile 编译主程序

​	mvn test

### 			7.插件和目标

### 			8.继承



疑问：mvn compile: 编译 main - java 下的所有java文件

1.为什么要下载：下载jar文件





# IDEA中使用Maven

idea中有一个自带的maven


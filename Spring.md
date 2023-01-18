# Spring

## 	Spring快速入门

### Spring的开发步骤

1.导入坐标

![image-20220806112546243](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20220806112546243.png)

2.创建bean

3.创建配置文件applicationContext.xml

4.配置文件中进行配置

![image-20220806112719425](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20220806112719425.png)

5.创建ApplicationContext对象获取Bean

![image-20220806112810283](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20220806112810283.png)

### Spring相关的配置文件

1.bean标签的基本配置 用于配置对象交由Spring来构建

​	基本属性：id:bean实例在Spring容器的唯一标识

​					  class：Bean的全限定名称

2.Bean标签的范围配置

​	scope:默认singleton  ：Bean的实例化时机：当Spring 的核心文件被加载时，实例化配置的Bean实例

​				protype：多例的 ：Bean的实例化时机：调用getBean()；

![image-20220810105833384](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20220810105833384.png)

![image-20220810113119880](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20220810113119880.png)

3.Bean的生命周期配置 

init_method 和 destroy_method

4.Bean实例化的三种方式

无参构造  工厂静态  工厂实例

7.Bean的依赖注入方式

9.引入其他配置文件（分模块开发）

![image-20221001102438386](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20221001102438386.png)

### 知识要点

![image-20221001102709546](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20221001102709546.png)

### Spring 相关的API

![image-20221001103212305](C:\Users\垃圾电脑\AppData\Roaming\Typora\typora-user-images\image-20221001103212305.png)

### Spring配置数据源

1.数据源（连接池）的作用  ： 提高程序性能

1.1 开发步骤 ：① 导入数据源的坐标 和数据库驱动坐标  ②创建数据源对象 ③ 设置数据源的基本链接数据 ④ 使用数据源获取链接资源和归还资源

1.2 数据源的手动创建

### 什么是springboot
我们都知道spring是当下盛行的Java EE企业级开发框架，它通过Ioc容器和Aop编程框架简化了Java开发。但是在spring中我们需要做大量的配置，这些配置多数情况下都是差不多的。

**为了解放程序员的双手，Springboot横空出世了，它遵循了“约定优于配置”的核心思想，可以说是对spring自身的一次简化。**

使用Springboot可以快速构建起一个工程，这让Springboot成为了构建微服务应用的必备神器！，微服务现在这么火，作为Java程序员，Springboot已然成为了我们求生的必备技能。

看下官网的这张图就明白了，**“Build Anything”**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226223047420.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)
### 使用IDEA创建一个springboot工程
使用IDEA构建第一个helloword程序

#### 新建一个maven项目
1、新建一个maven工程
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122622352275.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

这里我们不勾选archetype（项目骨架，其实就是maven项目模板），然后下一步

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226223644222.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

2、输入maven的groupid、artifactid、version信息，然后 下一步

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122622403746.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

3、选择好项目的位置，单击finish按钮完成即可

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226224222511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

上面的3个步骤其实和springboot工程没有关系，这个是IDEA中新建maven项目一般流程。看下建好的工厂结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226224530545.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

#### 添加Springboot依赖
将上面创建的maven工程变为Springboot工程其实很简单，**只需要在pom.xml文件中增加  `spring-boot-starter-parent`  作为parent即可。**

也就是在pom.xml文件中加入下面这段。

```java
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.8.RELEASE</version>
    </parent>
```

这里我们在引入一个web模块，即增加一个web模块的starter进来。

```java
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

```

可以看到，在spring中我们配置依赖的时候需要version，这里已经不需要配置version，因为，我们继承了父工程 **`spring-boot-starter-parent`**，Springboot帮我们管理好了版本了。

#### 准备就绪，开始写个hello工程
写一个HelloController控制器

```java
@RestController
public class HelloController {

    @GetMapping("/hello123")
    public String hello(){
        return "Springboot Build Anything !!!";
    }
}
```

写Springboot工程启动类，也叫程序入口类。

```java
@SpringBootApplication
public class SpringbootDemo {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootDemo.class,args);
    }
}
```

代码解释：

 - @SpringBootApplication是一个复合注解，使用该注解告诉springboot启用自动配置和组件扫描功能
 - Springboot内置了tomcat容器，因此直接可以通过main的形式启动一个web工程

看下最终的工程结构：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226234320676.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

### 运行一个Springboot工程
要运行上面的 **`springboot-demo-simple`** 工程，我们只需要到启动类 **`SpringbootDemo`** 运行main方法即可。

启动成功后
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226234706397.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3OTY1MDE4,size_16,color_FFFFFF,t_70)

浏览器地址栏输入 **“http://localhost:8080/hello123”**，可以看到运行结果如下。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191226234914701.png)


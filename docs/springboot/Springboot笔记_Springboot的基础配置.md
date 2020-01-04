### 添加spring-boot-dependencies依赖的方式
一般地，在pom中我们使用<parent>元素，通过继承spring-boot-starter-parent的方式，创建一个Springboot的项目。但是在企业级开发中<parent>元素可能被用来继承公司内的一些基础工程了。此时，我们可以通过添加spring-boot-dependencies依赖的方式让我们的工程编程一个Springboot的工程。

即将如下代码添加到pom.xml文件中：

```java
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>2.1.8.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>

```

但是此时，需要另外配置Java的编译版本，和编码格式。

### Springboot的核心注解@SpringBootApplication
Springboot通常会有一个*Application的入口类，该类中有一个main方法，在main方法中，我们通过SpringApplication.run来启动Springboot应用。
例如：

```java
@SpringBootApplication
public class SpringbootDemo {
    public static void main(String[] args) {
        SpringApplication.run(SpringbootDemo.class,args);
    }
}
```

@SpringBootApplication是Springboot的核心注解，它是一个组合注解。看下源码：

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};
}
```
@SpringBootConfiguration表明这是一个spring的配置类。我们看到了熟悉的@Configuration注解。

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Configuration
public @interface SpringBootConfiguration {
}
```

第二个@EnableAutoConfiguration注解表示开启自动配置。springboot会依据类路径中的jar包依赖为当前项目进行自动配置。

比如添加了spring-boot-starter-web依赖，会自动添加Tomcat和spring mvc相关的依赖包。

第三个@ComponentScan注解，就是spring中的注解，完成包扫描的功能。

### Springboot的配置文件
在Springboot中，配置文件不再是applicationContext*.xml了，而是application.properties属性文件。同时还支持yaml语言的位置文件application.yml。

一般地，Springboot的全局配置文件application.properties或application.yml放置在src/main/resources目录或者类路径下的/config目录下。

通过application.properties配置文件我们可以对springboot的一些默认配置做一些修改。比如我们要改下tomcat的端口号，就可以在application.properties文件中添加如下内容：

```java
server.port=8081
server.servlet.context-path=/happy
```

### 基于properties类型安全的配置
通过使用@ConfigurationProperties将properties属性和一个Bean及其属性关联，从而实现类型安全的配置。

1、我们在配置文件中添加如下内容：

```java
book.bookName=java
book.author=happy123
```

2、新建一个bean，并将属性注入。

```java
@Component
@ConfigurationProperties(prefix = "book")
public class Book {
    private String bookName;

    private String author;

    public String getBookName() {
        return bookName;
    }

    public void setBookName(String bookName) {
        this.bookName = bookName;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }
}
```

3、新建一个BookController测试一下。

```java
@RestController
public class BookController {

    @Autowired
    private Book book;

    @GetMapping("/book")
    public String book(){
        return book.toString();
    }

}
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200101193318138.png)
### Profile的配置
profile的机制是spring用来根据不同的环境可以灵活切换对应配置的。
在springboot中约定了不同环境的配置文件名规则：

```java
application-{profile}.properties
```

其中profile占位符表示了当前的环境。

然后在全局的配置文件application.properties中，通过设置spring.profiles.active={profile}属性达到不同环境切换不同配置的能力。




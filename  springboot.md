# springboot

# Mixed

```
gs-accessing-data-mysql
mysql> create database db_example; -- Create the new database
mysql> create user 'springuser'@'localhost' identified by 'ThePassword'; -- Creates the user
mysql> grant all on db_example.* to 'springuser'@'localhost'; -- Gives all the privileges to the new user on the newly created database
application.properties
spring.jpa.hibernate.ddl-auto=create
spring.datasource.url=jdbc:mysql://localhost:3306/db_example
spring.datasource.username=springuser
spring.datasource.password=ThePassword

$ curl 'localhost:8080/demo/add?name=First&email=someemail@someemailprovider.com'
mysql> revoke all on db_example.* from 'springuser'@'localhost';
mysql> grant select, insert, delete, update on db_example.* to 'springuser'@'localhost';

```

```
在Maven里使用dependency插件的tree目标也能获得相似的依赖树。 
$ mvn dependency:tree
```

### run springboot app
```
bogon:address_jpa hmh$ pwd
/Users/hmh/IdeaProjects/address_jpa

bogon:address_jpa hmh$ mvn spring-boot:run

bogon:address_jpa hmh$ java -jar target/address_jpa-0.0.1-SNAPSHOT.jar 

```

### 覆盖起步依赖引入的传递依赖
```
MAVEN 排除传递性依赖
在Maven里，可以用<exclusions>元素来排除传递依赖。下面这个引入Spring Boot的 build.gradle的<dependency>增加了<exclusions>元素去除Jackson:
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-web</artifactId>
      <exclusions>
        <exclusion>
          <groupId>com.fasterxml.jackson.core</groupId>
        </exclusion>
      </exclusions>
    </dependency>
另一方面，也许项目需要Jackson，但你需要用另一个版本的Jackson来进行构建，而不是Web 起步依赖里的那个。假设Web起步依赖引用了Jackson 2.3.4，但你需要使用2.4.31。在Maven里， 你可以直接在pom.xml中表达诉求，就像这样:
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.4.3</version>
    </dependency>
Maven总是会用最近的依赖，也就是说，你在项目的构建说明文件里增加的这个依赖，会覆 盖传递依赖引入的另一个依赖。
```

```
package readinglist;
import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext; import org.springframework.core.type.AnnotatedTypeMetadata;
    public class JdbcTemplateCondition implements Condition {
      @Override
      public boolean matches(ConditionContext context,
                             AnnotatedTypeMetadata metadata) {
        try {
          context.getClassLoader().loadClass(
                 "org.springframework.jdbc.core.JdbcTemplate");
          return true;
        } catch (Exception e) {
          return false;
} }
}
当你用Java来声明Bean的时候，可以使用这个自定义条件类:
    @Conditional(JdbcTemplateCondition.class)
    public MyService myService() {
... }
在这个例子里，只有当JdbcTemplateCondition类的条件成立时才会创建MyService这个 Bean。也就是说MyService Bean创建的条件是Classpath里有JdbcTemplate。否则，这个Bean
的声明就会被忽略掉。
```

## 自定义配置
### 通过覆盖springboot自动配置
### 通过属性文件处置配置
```
@Controller
@RequestMapping("/")
@ConfigurationProperties(prefix="amazon")
public class ReadingListController {
  private String associateId;
  private ReadingListRepository readingListRepository;
  @Autowired
  public ReadingListController(
        ReadingListRepository readingListRepository) {
    this.readingListRepository = readingListRepository;
}
  public void setAssociateId(String associateId) {
    this.associateId = associateId;
}
... ...

application.yml
amazon: 
      associatedId: mh #associated_id || associated-id

... ...
```

```
代码清单3-5 在一个Bean里加载配置属性 
package readinglist;
    import org.springframework.boot.context.properties.
                                       ConfigurationProperties;
import org.springframework.stereotype.Component;
@Component
@ConfigurationProperties("amazon")//注入带amazon 前缀的属性
public class AmazonProperties {
  private String associateId;
  public void setAssociateId(String associateId) {//associateId的 setter方法
    this.associateId = associateId;
}
  public String getAssociateId() {
    return associateId;
} }
```

```
@RequestMapping("/")
public class ReadingListController {
  private ReadingListRepository readingListRepository;
  private AmazonProperties amazonProperties;
  @Autowired
  public ReadingListController(
      ReadingListRepository readingListRepository,
      AmazonProperties amazonProperties) {
    this.readingListRepository = readingListRepository;
    this.amazonProperties = amazonProperties;//注入 AmazonProperties
}
  @RequestMapping(method=RequestMethod.GET)
  public String readersBooks(Reader reader, Model model) {
    List<Book> readingList =
        readingListRepository.findByReader(reader);

 if (readingList != null) {
model.addAttribute("books", readingList); model.addAttribute("reader", reader); model.addAttribute("amazonID", amazonProperties.getAssociateId());
}return "readingList";
  @RequestMapping(method=RequestMethod.POST)
  public String addToReadingList(Reader reader, Book book) {
    book.setReader(reader);
    readingListRepository.save(book);
    return "redirect:/";
} }
```

### application.yml
```
spring:
      datasource:
        url: jdbc:mysql://localhost/readinglist
        username: dbuser
        password: dbpass
        driver-class-name: com.mysql.jdbc.Driver # 一般可以根据url自动识别
```

### 使用命令参数 或 配置文件 激活Profile
```
$ java -jar readinglist-0.0.1-SNAPSHOT.jar --
         spring.profiles.active=production

spring:
      profiles:
        active: production
```

### 1. 使用特定于Profile的属性文件
```
application.properties  #common
application-development.properties #cfg in dev
application-production.properties #cfg in prod
激活其中的一个配置文件
```
### 2. 使用多Profile YAML文件进行配置
```
logging:
  level:
    root: INFO
---
spring:
  profiles: development
logging:
  level:
    root: DEBUG
---
spring:
  profiles: production
logging:
  path: /tmp/
  file: BookWorm.log
  level:
root: WARN
```

### 模拟 springMVC
```
package com.mh.springboot.address;

import org.hamcrest.Matchers;
import org.junit.Before;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;
import org.springframework.test.web.servlet.request.MockMvcRequestBuilders;
import org.springframework.test.web.servlet.result.MockMvcResultMatchers;
import org.springframework.test.web.servlet.setup.MockMvcBuilders;
import org.springframework.web.context.WebApplicationContext;

/**
 * Created by hmh on 20/10/2017.
 */
@RunWith( SpringRunner.class )
@SpringBootTest
public class MockMvcTest {
    @Autowired
    private WebApplicationContext context;

    private MockMvc mockMvc;

    @Test
    public void contextLoads( ) {

    }

    @Before
    public void setupMockMvc( ) {
        mockMvc = MockMvcBuilders.webAppContextSetup( context ).build( );
    }

    @Test
    public void homePage( ) throws Exception {
        mockMvc.perform( MockMvcRequestBuilders.get( "/lists" ) )
                .andExpect( MockMvcResultMatchers.status( ).isOk( ) )
                .andExpect( MockMvcResultMatchers.view( ).name( "personLists" ) )
                .andExpect( MockMvcResultMatchers.model( ).attributeExists( "personList" ) )
                .andExpect( MockMvcResultMatchers.model( ).attribute( "personList", Matchers.not( Matchers.empty( ) ) ) );
    }


}
```

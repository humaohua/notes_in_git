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

# 覆盖起步依赖引入的传递依赖
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

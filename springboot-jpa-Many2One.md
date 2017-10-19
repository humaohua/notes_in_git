# springboot-jpa-多对一关联例子(Person group)

# group
```
package com.mh.springboot.address.pojo;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

/**
 * Created by hmh on 09/10/2017.
 */
@Entity
@Table( name = "t_group" )//group default error
//exp(jpa com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException)
//default table name will be group, key word of sql syntax.
public class Group {
    @Id
    @GeneratedValue( strategy = GenerationType.AUTO )
    private Long gid;

    /*
    4. 设置字段类型
通过@Column注解设置，包含的可设置属性如下
    .name：字段名
.unique：是否唯一
.nullable：是否可以为空
.inserttable：是否可以插入
.updateable：是否可以更新
.columnDefinition: 定义建表时创建此列的DDL
.secondaryTable: 从表名。如果此列不建在主表上（默认建在主表），该属性定义该列所在从表的名字。

@Column(name = "user_code", nullable = false, length=32)//设置属性userCode对应的字段为user_code，长度为32，非空
private String userCode;
@Column(name = "user_wages", nullable = true, precision=12, scale=2)//设置属性wages对应的字段为user_wages，12位数字可保留两位小数，可以为空
private double wages;
@Temporal(TemporalType.DATE)//设置为时间类型
private Date joinDate;
     */
    @Column(nullable = false,length = 32)
    private String name;

    public Group( ) {}

    public Group( String name ) {
        this.name = name;
    }

    public Long getGid() {
        return gid;
    }

    public void setGid(Long gid) {
        this.gid = gid;
    }

    public String getName( ) {
        return name;
    }

    public void setName( String name ) {
        this.name = name;
    }

    @Override
    public String toString( ) {
        return "Group{" + "gid=" + gid + ", name='" + name + '\'' + '}';
    }
}
```

# person
```
package com.mh.springboot.address.pojo;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.ManyToOne;
import javax.persistence.Table;

/**
 * Created by hmh on 09/10/2017.
 */
@Entity
@Table( name = "t_person" )
public class Person {

     @GeneratedValue( strategy = GenerationType.AUTO )
    @Id
//  @GeneratedValue( generator = "system-uuid" )
//  @GenericGenerator( name = "system-uuid", strategy = "uuid" )
    private Long pid;

    @ManyToOne
    @JoinColumn(name = "gid")
    private Group group;
    private String name;
    private String mobile;

    public Person( ) {}

    public Person( Group group, String name, String mobile ) {
        this.group = group;
        this.name = name;
        this.mobile = mobile;
    }


    public Long getPid() {
        return pid;
    }

    public void setPid(Long pid) {
        this.pid = pid;
    }

    public Group getGroup() {
        return group;
    }

    public void setGroup(Group group) {
        this.group = group;
    }

    public String getName( ) {
        return name;
    }

    public void setName( String name ) {
        this.name = name;
    }

    public String getMobile( ) {
        return mobile;
    }

    public void setMobile( String mobile ) {
        this.mobile = mobile;
    }

    @Override
    public String toString() {
        return "Person{" +
                "pid=" + pid +
                ", group=" + group +
                ", name='" + name + '\'' +
                ", mobile='" + mobile + '\'' +
                '}';
    }
}
```

# repository
```
package com.mh.springboot.address.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;

import com.mh.springboot.address.pojo.Group;

/**
 * Created by hmh on 09/10/2017.
 */
public interface GroupRepository extends JpaRepository<Group, Long> {
    List<Group> findByName( String name );
}
```

```
package com.mh.springboot.address.repository;

import java.util.List;

import org.springframework.data.jpa.repository.JpaRepository;

import com.mh.springboot.address.pojo.Person;

/**
 * Created by hmh on 10/10/2017.
 */
public interface PersonRepository extends JpaRepository<Person, Long> {
    List<Person> findByName( String name );
}
```

# test
```
package com.mh.springboot.address;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import com.mh.springboot.address.pojo.Group;
import com.mh.springboot.address.pojo.Person;
import com.mh.springboot.address.repository.GroupRepository;
import com.mh.springboot.address.repository.PersonRepository;

@SpringBootApplication
public class AddressJpaApplication {
    private static final Logger log = LoggerFactory.getLogger( AddressJpaApplication.class );

    @Autowired
    PersonRepository personRepository;

    @Autowired
    GroupRepository groupRepository;

    public static void main( String[] args ) {
        SpringApplication.run( AddressJpaApplication.class, args );
    }

//  @Bean
//  public CommandLineRunner groupDemo( GroupRepository repository ) {
//      return ( args ) -> {
//          // save a couple of groups
////            repository.save( new Group( "family" ) );
////            repository.save( new Group( "friends" ) );
//
//
//          // fetch all groups
//          log.info( "Groups found with findAll():" );
//          log.info( "-------------------------------" );
//          for( Group group : repository.findAll( ) ) {
//              log.info( group.toString( ) );
//          }
//          log.info( "" );
//
//          // fetch an individual group by ID
//          Group group1 = repository.findOne( 1L );
//          log.info( "Group found with findOne(1L):" );
//          log.info( "--------------------------------" );
//          log.info( group1.toString( ) );
//          log.info( "" );
//
//          // fetch group by name
//          log.info( "Group found with findByName('family'):" );
//          log.info( "--------------------------------------------" );
//          for( Group group : repository.findByName( "family" ) ) {
//              log.info( group.toString( ) );
//          }
//          log.info( "" );
//      };
//  }

    @Bean
    public CommandLineRunner personDemo( GroupRepository groupRepository1, PersonRepository personRepository1 ) {
        return ( args ) -> {
            Group group = new Group("py");
            Person person = new Person(group,"z3","13333333333");
            groupRepository.save(group);
            personRepository.save(person);

            // fetch all customers
            log.info( "Persons found with findAll():" );
            log.info( "-------------------------------" );
            for( Person p : personRepository.findAll( ) ) {
                log.info( p.toString( ) );
            }
            log.info(personRepository.findByName("z3").toString());
            log.info( "" );
        };
    }
}

```

# output info
```
 Hibernate: insert into t_group (name) values (?)
Hibernate: insert into t_person (gid, mobile, name) values (?, ?, ?)
2017-10-19 10:13:43.784  INFO 12533 --- [           main] c.m.s.address.AddressJpaApplication      : Persons found with findAll():
2017-10-19 10:13:43.784  INFO 12533 --- [           main] c.m.s.address.AddressJpaApplication      : -------------------------------
2017-10-19 10:13:43.818  INFO 12533 --- [           main] o.h.h.i.QueryTranslatorFactoryInitiator  : HHH000397: Using ASTQueryTranslatorFactory
Hibernate: select person0_.pid as pid1_1_, person0_.gid as gid4_1_, person0_.mobile as mobile2_1_, person0_.name as name3_1_ from t_person person0_
Hibernate: select group0_.gid as gid1_0_0_, group0_.name as name2_0_0_ from t_group group0_ where group0_.gid=?
2017-10-19 10:13:43.966  INFO 12533 --- [           main] c.m.s.address.AddressJpaApplication      : Person{pid=1, group=Group{gid=1, name='py'}, name='z3', mobile='13333333333'}
Hibernate: select person0_.pid as pid1_1_, person0_.gid as gid4_1_, person0_.mobile as mobile2_1_, person0_.name as name3_1_ from t_person person0_ where person0_.name=?
Hibernate: select group0_.gid as gid1_0_0_, group0_.name as name2_0_0_ from t_group group0_ where group0_.gid=?
2017-10-19 10:13:43.989  INFO 12533 --- [           main] c.m.s.address.AddressJpaApplication      : [Person{pid=1, group=Group{gid=1, name='py'}, name='z3', mobile='13333333333'}]
2017-10-19 10:13:43.989  INFO 12533 --- [           main] c.m.s.address.AddressJpaApplication      : 
2017-10-19 10:13:43.990  INFO 12533 --- [           main] c.m.s.address.AddressJpaApplication      : Started AddressJpaApplication in 10.533 seconds (JVM running for 11.185)

```


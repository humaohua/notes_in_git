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

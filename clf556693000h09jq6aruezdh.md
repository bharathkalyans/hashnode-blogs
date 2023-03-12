---
title: "How to Configure Multiple Databases in Spring Boot?"
datePublished: Sun Mar 12 2023 08:36:38 GMT+0000 (Coordinated Universal Time)
cuid: clf556693000h09jq6aruezdh
slug: how-to-configure-multiple-databases-in-spring-boot
tags: postgresql, java, databases, springboot

---

* Configuring a single database source in Spring Boot isn't that tough, but things get tricky when we try to configure multiple data sources.
    
* This blog will show you how to configure PostgreSQL and MySQL.You can use any database you wish.
    

### Follow the below steps :

* Head to [https://start.spring.io/](https://start.spring.io/) and select the dependencies shown in the picture, download the zip file, and extract it to your favorite editor (please use IntelliJ 🙂).
    
* The blog uses PostgreSQL and MySQL dependencies, you must use the dependencies for the Databases you are going to work with.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678547318482/7de610c5-5749-422a-a4f0-af2cc3554cae.png align="center")

* Once your project is configured, add the below code to your application.properties file or application.yaml file and change the credentials accordingly.
    

```yaml
#Database 1
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/demo
    username: root
    password: root


  jpa:
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect
    show-sql: true
    hibernate:
      ddl-auto: update


#Database 2
second:
  datasource:
    driver-class-name: org.postgresql.Driver
    url: jdbc:postgresql://localhost:5432/personal
    username: postgres
    password:


  jpa:
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
    show-sql: true
    hibernate:
      ddl-auto: update
```

* <mark>We must ensure that each database's entities, repositories and config files must be in different packages.</mark>
    
* To get a better idea, look at the following picture.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1678548640090/0e7bc593-1ac7-4d01-825c-a3ab4f9d2797.png align="center")

* Open your configuration files and paste the code given below.
    

```java
package com.bharathkalyans.multipledb.db.mysql.config;


import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.core.env.Environment;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.JpaVendorAdapter;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;


@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        entityManagerFactoryRef = "entityManagerFactoryBean",
        basePackages = {"com.bharathkalyans.multipledb.db.mysql.repository"},
        transactionManagerRef = "firstTransactionManager"
)
public class MySQLDbConfig {


    private final Environment environment;

    @Autowired
    public MySQLDbConfig(Environment environment) {
        this.environment = environment;
    }

    @Bean
    @Primary
    public DataSource dataSource() {

        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setUrl(environment.getProperty("spring.datasource.url"));
        dataSource.setDriverClassName(environment.getProperty("spring.datasource.driver-class-name"));
        dataSource.setUsername(environment.getProperty("spring.datasource.username"));
        dataSource.setPassword(environment.getProperty("spring.datasource.password"));

        return dataSource;
    }


    @Bean(name = "entityManagerFactoryBean")
    @Primary
    public LocalContainerEntityManagerFactoryBean entityManagerFactoryBean() {
        LocalContainerEntityManagerFactoryBean bean = new LocalContainerEntityManagerFactoryBean();

        bean.setDataSource(dataSource());

        JpaVendorAdapter adapter = new HibernateJpaVendorAdapter();
        bean.setJpaVendorAdapter(adapter);

        Map<String, String> props = new HashMap<>();
        props.put("hibernate.dialect", "org.hibernate.dialect.MySQLDialect");
        props.put("hibernate.show_sql", "true");
        props.put("hibernate.hbm2ddl.auto", "update");

        bean.setJpaPropertyMap(props);

        bean.setPackagesToScan("com.bharathkalyans.multipledb.db.mysql.entities");


        return bean;
    }


    @Bean(name = "firstTransactionManager")
    @Primary
    public PlatformTransactionManager platformTransactionManager() {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(entityManagerFactoryBean().getObject());
        return transactionManager;
    }


}
```

* Do the same for the PostgresDbConfig file as well.
    

```java
package com.bharathkalyans.multipledb.db.postgres.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Primary;
import org.springframework.core.env.Environment;
import org.springframework.data.jpa.repository.config.EnableJpaRepositories;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import org.springframework.orm.jpa.JpaTransactionManager;
import org.springframework.orm.jpa.JpaVendorAdapter;
import org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean;
import org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter;
import org.springframework.transaction.PlatformTransactionManager;
import org.springframework.transaction.annotation.EnableTransactionManagement;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;

@Configuration
@EnableTransactionManagement
@EnableJpaRepositories(
        entityManagerFactoryRef = "secondEntityManagerFactoryBean",
        basePackages = {"com.bharathkalyans.multipledb.db.postgres.repository"},
        transactionManagerRef = "secondTransactionManager"
)
public class PostgresDbConfig {

    @Autowired
    private Environment environment;

    @Bean(name = "secondDataSource")
    @Primary
    public DataSource dataSource() {

        DriverManagerDataSource dataSource = new DriverManagerDataSource();
        dataSource.setUrl(environment.getProperty("second.datasource.url"));
        dataSource.setDriverClassName(environment.getProperty("second.datasource.driver-class-name"));
        dataSource.setUsername(environment.getProperty("second.datasource.username"));
        dataSource.setPassword(environment.getProperty("second.datasource.password"));

        return dataSource;
    }


    @Bean(name = "secondEntityManagerFactoryBean")
    @Primary
    public LocalContainerEntityManagerFactoryBean entityManagerFactoryBean() {
        LocalContainerEntityManagerFactoryBean bean = new LocalContainerEntityManagerFactoryBean();

        bean.setDataSource(dataSource());

        JpaVendorAdapter adapter = new HibernateJpaVendorAdapter();
        bean.setJpaVendorAdapter(adapter);

        Map<String, String> props = new HashMap<>();
        props.put("hibernate.dialect", "org.hibernate.dialect.PostgreSQLDialect");
        props.put("hibernate.show_sql", "true");
        props.put("hibernate.hbm2ddl.auto", "create");

        bean.setJpaPropertyMap(props);
        bean.setPackagesToScan("com.bharathkalyans.multipledb.db.postgres.entities");


        return bean;
    }

    @Bean(name = "secondTransactionManager")
    @Primary
    public PlatformTransactionManager platformTransactionManager() {
        JpaTransactionManager transactionManager = new JpaTransactionManager();
        transactionManager.setEntityManagerFactory(entityManagerFactoryBean().getObject());
        return transactionManager;
    }
}
```

* Once everything is configured, you can paste the following Test to verify if everything is working perfectly.
    

```java
package com.bharathkalyans.multipledb;

import com.bharathkalyans.multipledb.db.mysql.entities.User;
import com.bharathkalyans.multipledb.db.mysql.repository.UserRepository;
import com.bharathkalyans.multipledb.db.postgres.entities.Manager;
import com.bharathkalyans.multipledb.db.postgres.repository.ManagerRepository;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class MultipleDbApplicationTests {


    @Autowired
    private ManagerRepository managerRepository;

    @Autowired
    private UserRepository userRepository;

    @Test
    void dbTest() {

        User user = User.builder()
                .userName("Bharath")
                .address("KA")
                .phoneNumber("7996132372")
                .userId(100L)
                .build();

        Manager manager = Manager.builder()
                .userName("Ramesh")
                .address("AP")
                .phoneNumber("7996132372")
                .userId(1212L)
                .build();

        managerRepository.save(manager);
        userRepository.save(user);

    }

}
```

* This is how you configure multiple databases in Spring Boot. To add another database all you need to do is to create another package and follow the above steps.
    

[Source Code](https://github.com/bharathkalyans/configure-multiple-databases-springboot)

---

Hope you liked this blog 😀!
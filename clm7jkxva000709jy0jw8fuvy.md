---
title: "Implement Swagger UI for Spring Boot 3.x"
datePublished: Wed Sep 06 2023 09:33:30 GMT+0000 (Coordinated Universal Time)
cuid: clm7jkxva000709jy0jw8fuvy
slug: implement-swagger-ui-for-spring-boot-3x
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693992696820/0f07158c-dee2-4095-85c3-5d09f439d973.jpeg
tags: java, documentation, apis, swagger

---

* From Spring Boot 3.x onwards we can't use the old dependencies like Spring Fox Swagger & Spring Fox Swagger UI.
    
* We have an [OpenAPI](https://springdoc.org/) Swagger in place of this and add the following dependency to your pom.xml file.
    

```swift
   <dependency>
      <groupId>org.springdoc</groupId>
      <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
      <version>2.2.0</version>
   </dependency>
```

* Once you add this, you can access the swagger on
    
    \`[http://localhost:{port-number}/swagger-ui/index.html](http://localhost:8080/swagger-ui/index.html)\`.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693992173699/8b17fa0e-ae03-4267-bf92-aa19e24ae931.png align="center")

* To add details, for example, to change the definition, versions, etc. we must hardcode these values inside the spring boot application.
    
* If we are using Spring Security, we must add a Swagger configuration to access this Swagger doc.
    

If you want to override this, if you are using spring security, you can add the following dependency. (Use it only for testing)

```java
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
</dependency>
```

### OpenApi Configuration

* Let's configure the OpenApi details, which I have previously mentioned above.
    
* Create a new Java class called OpenApiConfig inside any package(use config).
    
* Use the following code and change accordingly.
    

```javascript

import io.swagger.v3.oas.annotations.OpenAPIDefinition;
import io.swagger.v3.oas.annotations.info.Contact;
import io.swagger.v3.oas.annotations.info.Info;
import io.swagger.v3.oas.annotations.info.License;
import io.swagger.v3.oas.annotations.servers.Server;

@OpenAPIDefinition(
        info = @Info(
                contact = @Contact(
                        name = "Bharath Kalyan S",
                        email = "sbharath25@gmail.com",
                        url = "https://twitter.com/bharathkalyans"
                ),
                description = "OpenApi Documentation for Personal Project 😄",
                title = "Service for Fetching and Persisting Employee Data",
                version = "1.0",
                summary = "Add summary of your Service",

                license = @License(name = "MIT-License")
        ),
        servers = {
                @Server(
                        description = "Local Dev",
                        url = "http://localhost:8080"
                ),
                @Server(
                        description = "Prod Env",
                        url = "http://prod-env:8080"
                )
        }
)
public class OpenApiConfig {
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1693999226271/51c95b00-3220-4645-bb07-fd31e1f7c0d5.png align="center")

* You can change the details as per your requirements.
    

---

* I hope you liked this blog, will be writing a blog on how to use Swagger with Spring Security as well.
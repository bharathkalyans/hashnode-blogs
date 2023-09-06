---
title: "Spring Boot 3.x + Swagger (OpenAPI 3)"
datePublished: Wed Sep 06 2023 09:33:30 GMT+0000 (Coordinated Universal Time)
cuid: clm7jkxva000709jy0jw8fuvy
slug: spring-boot-3x-swagger-openapi-3
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1693992696820/0f07158c-dee2-4095-85c3-5d09f439d973.jpeg
tags: java, documentation, apis, swagger

---

* From Spring Boot 3.x onwards we can't use the old dependencies like Spring Fox Swagger & Spring Fox Swagger UI.
    
* We have an [OpenAPI](https://springdoc.org/) Swagger in place of this and to get started, add the following dependency to your pom.xml file.
    

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

* To customize details, for example, to change the definition, versions, etc. we must hardcode these values inside the spring boot application using annotations.
    

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
    

### Customizing Controller Names and Endpoints

* To customize a particular controller, we will be using **@Tag** and **@Operation** for renaming and adding summary, description, etc. for the endpoints.
    

```javascript
@RestController
@RequestMapping(value = "api/v1/")
@Tag(name = "Employee")
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    @GetMapping("/employees")
    @Operation(
            description = "Retrieve details of Employees",
            summary = "All the details of employees will be fetched from the database.",
            responses = {
                    @ApiResponse(
                            responseCode = "200",
                            description = "Success"
                    ),
                    @ApiResponse(
                            responseCode = "404",
                            description = "Unable to Employees Fetch / Invalid EndPoint "
                    )
            }
    )
    public List<Employee> getAllEmployees() {
        return employeeService.getAllEmployees();
    }
}
```

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694013933877/74a5463e-f846-48f4-b01f-75636ac315c4.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694014106393/7760f5d5-5a4c-4034-8d4f-eae31157c112.png align="center")

### Hiding Controllers or Endpoints

* To hide any controller or any endpoint, we have to use **@Hidden** annotation.
    
* To hide the controller, annotate it above the class name, and to hide any endpoint annotate it above the method name.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694014321386/cdc741c9-2db1-4bf7-870c-c746bdb160d8.png align="center")

Both put and delete mappings will be hidden to the client/user.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1694014385385/22fc8c31-f3ec-4d72-8b1e-19b9848fb62f.png align="center")

* Swagger is a great tool when you are integrating these REST APIs into your front end. It helps the frontend team have a cleaner look and get good insights about the APIs.
    

Play around with it by referring to the official documentation [https://springdoc.org/](https://springdoc.org/)

---

* I hope you liked this blog, will be writing a blog on how to use Swagger with Spring Security as well which will be a continuation of this blog.
# Create a REST API in Spring Boot under 30 minutes!

## What is REST API?

*   People usually think REST API is technology or a framework, which is wrong.
    
*   REST stands for `Representation State Transfer` which is a way for computers to communicate with each other efficiently.
    
*   There are 4 basic HTTP verbs we use in requests to interact with resources in a REST system:
    
    *   GET — retrieve a specific resource (by id) or a collection of resources
        
    *   POST — add a new resource
        
    *   PUT — update a specific resource (by id)
        
    *   DELETE — remove a specific resource by id
        
*   For a quick demo, open postman or any browser and type this [https://api.github.com/users/bharathkalyans](https://api.github.com/users/bharathkalyans) or
    
    \`[https://api.github.com/users/](https://api.github.com/users/bharathkalyans)&lt;*github-username&gt;*\`
    
*   The above URI is a \`***GET***\` request where &lt;github-username&gt; is your unique id.
    
*   You will see a JSON response that contains all the details of that user's GitHub details which are public. To access private repository information you have to pass an API authorization token.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671642994140/gXZNLUz6K.png align="center")
    

REST API is simply beautiful.

Below is the list of items you must need before starting:

*   IDE (IntelliJ \[preferred\], Spring STS, Eclipse, etc.)
    
*   Browser or PostMan for testing our API.
    
*   PostgreSQL or any SQL to fetch the live data from the database.
    

### Let's start building our REST API (MicroService)

*   Go to [https://start.spring.io/](https://start.spring.io/) and select the following as shown in the below figure.
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671643360776/P67VOQH23.png align="center")
    
*   You can change the group id, artifact id and even description as well. Add these 4 dependencies and generate the zip.
    
    > You can choose MySQL driver if you are working with MySQL.
    
*   Unzip and open the project in \`IntelliJ\` and let the IDE download the required maven dependencies.
    
*   After that rename your application.properties to application.yml and paste the code given below.
    

```yaml
server:
  error:
    include-message: always
    include-binding-errors: always
  port: 8080
# Change your port if it is already in use.
spring:
  main:
    banner-mode:
  datasource:
    url: jdbc:postgresql://localhost:5432/databasname
    username: postgres
    password:
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
        format_sql: true
```

*   Once you have done this, you can start the server and you must see your console screen something like this. (Make sure you create a database before running the application, else it might throw an error database not found.)
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671644196840/8bTETh4Wf.png align="left")
    
*   Create a package model and create a class called Author.
    
    ```java
    package com.bharathkalyans.myrestapi.model;
    
    import jakarta.persistence.*;
    
    @Entity 
    public class Author {
    
        @Id 
        @SequenceGenerator(name = "author_sequence_generator",            sequenceName = "author_sequence_generator", allocationSize = 1)                @GeneratedValue(strategy = GenerationType.IDENTITY) 
        private Long authorId; 
        private String authorName;
    
        public Author() { }
    
        public Author(String authorName) { this.authorName = authorName; }
    
        public Long getAuthorId() { return authorId; }
    
        public void setAuthorId(Long authorId) { this.authorId = authorId; }
    
        public String getAuthorName() { return authorName; }
    
        public void setAuthorName(String authorName) { 
               this.authorName = authorName; 
        } 
    }
    ```
    
*   Annotations do your job of creating tables. This is handled by Spring JPA.
    
*   To handle any incoming requests we must annotate a class with \`@RestController\`.This class is going to handle our incoming requests.
    
*   Create a package controller and create a new class AuthorController inside it.
    
    ```java
    package com.bharathkalyans.myrestapi.controller;
    
    import com.bharathkalyans.myrestapi.model.Author; 
    import com.bharathkalyans.myrestapi.service.AuthorService; 
    import org.springframework.beans.factory.annotation.Autowired; import org.springframework.web.bind.annotation.GetMapping; 
    import org.springframework.web.bind.annotation.RequestMapping; import org.springframework.web.bind.annotation.RestController;
    
    import java.util.List;
    
    @RestController @RequestMapping("api/v1") 
    public class AuthorController {
    
        @Autowired 
        AuthorService authorService;
    
        @GetMapping("/persons") 
        public List getAuthors() { 
                return authorService.getAllAutors(); 
        } 
    }
    ```
    
*   @RequestMapping annotation is added to the class to create a URL path for the request.
    
*   @Autowired is used for dependency injection. It creates an object whenever it is required and this is handled by the spring.
    
*   Create a package service and create an interface AuthorService and annotate it with @Service.
    
    ```java
    
    package com.bharathkalyans.myrestapi.service;
    
    import com.bharathkalyans.myrestapi.model.Author;
    import org.springframework.stereotype.Service;
    
    import java.util.List;
    
    @Service
    public interface AuthorService {
        public List<Author> getAllAuthors();
    
    }
    ```
    
*   Create a package inside a service called impl and create a class called AuthorServiceImpl and implement the AuthorService.
    
    ```java
    package com.bharathkalyans.myrestapi.service.impl;
    
    import com.bharathkalyans.myrestapi.dao.AuthorRepository; 
    import com.bharathkalyans.myrestapi.model.Author; 
    import com.bharathkalyans.myrestapi.service.AuthorService; 
    import org.springframework.beans.factory.annotation.Autowired; import org.springframework.stereotype.Component;
    
    import java.util.List;
    
    @Component 
    public class AuthorServiceImpl implements AuthorService{
    
        @Autowired 
        private AuthorRepository authorRepository;
    
        @Override 
        public List getAllAuthors() { 
                return authorRepository.findAll(); 
        } 
    }
    ```
    
*   Create a package dao and create an interface AuthorRepository and implement JpaRepository&lt;ModeName, ID DataType&gt;.
    
    ```java
    package com.bharathkalyans.myrestapi.dao;
    
    import com.bharathkalyans.myrestapi.model.Author; 
    import org.springframework.data.jpa.repository.JpaRepository;
    
    public interface AuthorRepository 
                extends JpaRepository<Author, Long> { }
    ```
    
*   Make sure that the ID data type must match the Model ID.
    
*   And that's it, you don't need to create any query to fetch the data from the database.
    
*   You can always create your custom query in this interface using @Query Annotation.
    
*   After all the packaging structure must look as shown below
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671685230619/EZMMOdHR-.png align="center")
    
*   Once you are done with this you can start your application and if there are no errors you can fetch the data from the endpoint specified.
    
*   I have manually added data to the database.
    
*   Visit [https://www.mockaroo.com/](https://www.mockaroo.com/) for getting dummy data.
    
*   Open postman and hit this endpoint: ***http://localhost:8080/api/v1/persons***
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1671685439143/h-inMvN7O.png align="center")

*   And congrats 👏🏻 🥳 you have created your first REST API.
    
*   I only created the READ operation, if you want me to create it for other CRUD\[CREATE READ UPDATE DELETE\] operations let me know in the comments.
    

[Project Repository Link](https://github.com/bharathkalyans/Myrestapi)

### Happy Coding 🫡
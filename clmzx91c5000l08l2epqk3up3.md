---
title: "Docker + PostgreSQL +  pgAdmin"
datePublished: Tue Sep 26 2023 06:13:42 GMT+0000 (Coordinated Universal Time)
cuid: clmzx91c5000l08l2epqk3up3
slug: docker-postgresql-pgadmin
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1695708721734/27a41e30-8069-4e57-9a73-502625daefd9.png
tags: postgresql, docker, docker-images, pgadmin, wemakedevs

---

In this blog, you will learn how to connect your PostgreSQL to pgAdmin, which both run on docker.

### Installation of Software

* To get started first, we have to install Docker.
    
* Follow this blog [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/) to install Docker.
    

### Download Docker Images

* Let us pull the official docker images of PostgreSQL and pgAdmin.
    
    * **PostgreSQL**
        
        ```bash
        docker pull postgres:alpine
        ```
        
        * I will be using the Alpine version (smaller image) for this blog.
            
        * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695633316790/1ba0bc25-f5a8-4355-b3bc-dcc38880c96a.png align="center")
            
    * **PgAdmin**
        
        ```bash
        docker pull dpage/pgadmin4
        ```
        
        * ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695633534191/9b3e505a-89e9-400a-b586-20da168af2c6.png align="center")
            
    * I have both docker images already downloaded on my machine.
        

### Starting Containers

* Let's first start our PostgreSQL container by running the following command.
    
    ```bash
    docker run -p 5432:5432 \
    -e POSTGRES_USER=postgres \
    -e POSTGRES_PASSWORD=root \
    -d postgres:alpine
    ```
    
* The above command runs our container in a detached mode.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695634218049/3e398c4f-9e8c-4570-94b5-bd21dff3ce75.png align="center")
    
    Using the command `docker ps -a` to check our container status.
    
* ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695634254384/b4386c4d-5ef4-40b7-8380-9cde5eb4249d.png align="center")
    
* Starting the pgAdmin container using the below command
    
    ```bash
    docker run -p 80:80 \
        -e 'PGADMIN_DEFAULT_EMAIL=sbharath35@gmail.com' \
        -e 'PGADMIN_DEFAULT_PASSWORD=1234' \
        -d dpage/pgadmin4
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695703151501/d8d3967f-c139-4000-802a-97c83830021c.png align="center")
    
* To see the containers running use the following command
    
    ```bash
    docker ps -a
    ```
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695703240801/6cf81527-5bcc-4ff9-a4d1-52910cb45945.png align="center")
    
* Our container's Postgres is running on port 5432 and pgAdmin is running on port 80.
    

### Connecting Postgres to pgAdmin

* Once both containers are up and running, open your browser and open [http://localhost:8080/](http://localhost:8080/) , enter your details, and log in.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695703647248/ab0d1b64-5bec-418c-81fb-6b2d074a3eaf.png align="center")

* Click on Servers -&gt; Register -&gt;Server and a new dialog box will open.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695707075050/36a548f8-1bcc-4df7-8f05-43fd8e80f262.png align="center")

* Give a name and click on Connection to add additional parameters.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695707142768/1144eebb-0a11-4f19-ab2c-b0842c336335.png align="center")

* As our Postgres database is running inside a docker, we have to use "host.docker.internal" instead of "localhost". If you want to connect to a local Postgres database that is running on a host machine, use localhost.
    
* Use the port number, username, and password we mentioned while running the Postgres image and click on save. If all goes well, you will see the below dashboard.
    

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1695707413738/c2b5c644-5fcc-43bb-b0cf-cc07f221d69f.png align="center")

---

This is how you use pgAdmin without downloading any software. Hope you liked this blog 😀, see you soon, till then you can follow me on [@twitter](https://twitter.com/bharathkalyans) & [@github](https://github.com/bharathkalyans/)
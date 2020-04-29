# Creating a full-microservice application with Docker Compose


We've been discussing Docker, containers and microservices on this blog for some time already. So far we already reviewed how to create a Docker image for our ASP.NET application, push it to Azure Container Registry. But no application runs by itself. So let's build on top of those examples and create a minimal yet complex application consisting of 4 microservices and deploying it on our current environment with Docker Compose.

On this post we will build:
    * An asp.net app frontend to receives requests for newsletter subscriptions
    * A dotnet core backend that processes the requests, saves the data to a MongoDB database and sends a confirmation email
    * a Docker compose script to build and run the application locally
    * init and configure a mongodb database

## What's Docker Compose
TODO


## How to use Docker Compose
* Docker-compose.yml: This is the baseDocker-compose file used to define the collection of images to be built and run with Docker-compose build/run.



TODO ----------------------

## What we are going to build
We want to make a service that will be able to serve as a good example but not be too complicated, to show what a real-word example of a service might probably look like. For this use case, we will make a container grouping that can do two things behind basic HTTP authentication:

    Show a simple landing page with a email signup form
    Entering the email sends the data to the app service that
        stores on the db and
        Sends a notification via a sendgrid email notification service

Source code
https://github.com/sgnn7/deploying_with_docker

Introduction

In general, most of the simpler services that get utilized out there will look something similar to what is shown in this high-level diagram:
Our project
web server <-> Database <-> app service 


Web server
Will be a simple kestrel asp.net core service


Application service

 The application server is your main service logic, generally wrapped up in some web-accessible endpoints or a queue-consuming daemon. This piece could be used as follows:

    The main website framework
    Data manipulation API logic
    Some sort of data transformation layer
    Data aggregation framework

The main distinction between an application server versus a web server is that the web server generally operates on static data and makes generally rigid decisions in a flow, while the application server does almost all of the dynamic data processing in a non-linear fashion.
The reason why we defer as much of the work as we can to the web server instead of the application server is that due to the framework overhead, an application server is generally extremely slow and thus unsuitable to do simple, small, and repetitive tasks that a web server could chew through without breaking a sweat

Access to the database
How the App service accesses the database

In nodejs, accessing the db could be done with:

const DB_HOST = process.env.DB_HOST || 'localhost:27017';

Which s the same on other languages: by using environment variables

Remember when we previously mentioned that much of image configuration should be done through environment variables before? That is exactly what we are doing here! If an environment variable DB_HOST is set (as we expect it to be when running as a container), we will use it as the hostname, but if none is provided (as we expect it when running locally), it will assume that the database is running locally on the standard MongoDB port. This provides the flexibility of being configurable as a container and being able to be tested locally by a developer outside of Docker.
Passing the dbhost env var to the container is done when running the container with -e as a param:

docker run --rm 
             -d 
             -e DB_HOST=172.17.0.1:27000 
             -p 8000:8000 
             application_server

The magic IP (docker networking)
If you look at the above command, you'll realize that we hardcoded the Ip. 

When the Docker service is started on a machine, a number of networking iptables rules are added to your machine in order to allow the container to connect to the world through forwarding and vice versa. Effectively, your machine becomes an Internet router for all containers started. On top of this, each new container is assigned a virtual address (most likely in the range of 172.17.0.2+) and any communication it does will be normally invisible to the other containers unless a software-defined network is created, so connecting multiple container on the same machine is actually a really tricky task to do manually without helper software that is in the Docker infrastructure called Service Discovery.





The database

Once we have this logic and static file processing down, they are sadly mostly useless without the actual data to transform and pass around. As with any software that uses data, this is done with a backing database. Since we want to be able to scale any piece of the system and isolate discrete components, the database gets its own section. In the pre-container world, though, we were dependent on big, monolithic databases that provided us with Atomicity, Consistency, Isolation, and Durability (ACID) properties, and they did their job well. However, in the container world, we absolutely do not want this type of architecture as it is neither as resilient nor as horizontally scalable as databases that are shardable and able to be clustered. 

With these new-style databases, though, you generally do not get the same assurance that your data is treated in the same manner as the old-style ones, and it is an important distinction to have. What you get with most container-friendly databases instead of ACID is Basically Available, Soft state, Eventual consistency (BASE), which pretty much means that data will eventually be correct, but between the update initially being sent and the final state, the data may be in various states of intermediate values

Mongodb

For our project we'll use mongodb!

Volumes - persisting db data

The only thing we should consider when we run it is to make sure that the database storage volume from the container (/var/lib/mongodb) is mounted from the host into the container so that we preserve it if the container stops

This process of mounting (sometimes called mapping) a directory into the container is actually relatively easy to do when we start it if our volume is a named volume stored within Docker internals:

$ docker run --rm -d -v local_storage:/data/db -p 27000:27017 database

What this will do is create a named volume in Docker's local storage called local_storage, which will be seamlessly mounted on /data/db in the container (the place where the MongoDB image stores its data in the images from Docker Hub). If the container dies or anything happens to it, you can mount this volume onto a different container and retain the data.
And specifying the container on run should be as simple as simple as: 

docker run --rm 
             -d 
             -v local_storage:/data/db 
             -p 27000:27017 
             database
16c72859da1b6f5fbe75aa735b539303c5c14442d8b64b733eca257dc31a2722

Passing config to our containers 
If you include a build argument or a baked-in environment variable, anyone with access to the image can read it. Also, if you pass in the credentials through an environment variable during container creation, anyone that has docker CLI access can read it so you're mostly left with mounting of volumes with credentials to the container.

The solution is injecting files via volumes:

$ docker run --rm 
             -v $HOME/test_htpasswd:/srv/www/html/.htpasswd 
             -p 8080:80 web_server


Advanced security features
There are a few other ways of passing credentials securely, though they are a bit outside of the scope of this exercise such as env variables that contain hashed passwords, using a broker secrets-sharing service, using cloud-specific roles mechanisms (that is, AWS, IAM Role, user-data), and a few others, but the important part for this section is to understand which things you should try not to do when handling authentication data.


Project tree

tree .
.
├── application_server
│   ├── Dockerfile
│   ├── index.js
│   ├── package.json
│   └── views
│       └── index.pug
├── database
│   └── Dockerfile
└── web_server
    ├── Dockerfile
    └── nginx_main_site.conf


Building multiple microservices
Building these microservices
for dir in *; do cd $dir; docker build -t $dir .; cd ..; done


Running our microservices
We should run our services respecting the dependencies:  db, app svc, web svc

docker run --rm 
             -d 
             -p 27000:27017 
             database
3baec5d1ceb6ec277a87c46bcf32f3600084ca47e0edf26209ca94c974694009

$ docker run --rm 
             -d 
             -e DB_HOST=172.17.0.1:27000 
             -p 8000:8000 
             application_server
dad98a02ab6fff63a2f4096f4e285f350f084b844ddb5d10ea3c8f5b7d1cb24b

$ docker run --rm 
             -d 
             -p 8080:80 
             web_server
3ba3d1c2a25f26273592a9446fc6ee2a876904d0773aea295a06ed3d664eca5d

$ # Verify that all containers are running
$ docker ps --format "table {{.Image}}	{{.Status}}	{{.ID}}	{{.Ports}}"


Next

    Service discovery: avoid hardcoding ips, etc 
    Cloud Security features











# Summary
The microservice architectural style being distributed by design gives us better options to protect valuable business critical system. Traditional .NET-based authentication and authorization techniques are not sufficient and cannot be applied to the microservice world. We also saw why secure-token-based approaches, such as OAuth 2.0 and OpenID Connect 1.0, are becoming de facto standards for microservice authorization and authentication. If you want to have more general information related to security, do visit Open Web Application Security Project (OWASP) at http://www.owasp.org and Microsoft Security development life cycle at https://www.microsoft.com/en-us/sdl/. Azure AD can very well support OAuth 2.0 and OpenID Connect 1.0. Azure API Management can also act as an API gateway in microservices' implementation and also provide nifty security features, such as policies.

Azure AD and Azure API management provide quite a few powerful capabilities to monitor and log the requests received. This will be quite useful, not only for security but also for tracing and troubleshooting scenarios. We will see logging, monitoring, and the overall instrumentation around troubleshooting of microservices in the next chapter.

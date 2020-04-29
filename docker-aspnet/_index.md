# How to create a ASP.NET website with Docker
Docker is the most used technologies on the market today. We already discussed its benefits, how to install it and even listed technical details every developer should know.

On this post:
    * create a simple web app
    * build container
    * run app as a local container

## Requirements
For this post, I'll ask you to make sure that you have the following requirements installed:
    * .NET Core 3.1
    * Docker Desktop (if you're running on Windows or Mac)


# Containers in ASP.NET world
For our ASP.NET containers, we'll use Microsoft's official base image based on ASP.NET core 3.1. This is probably the last version of .NET Core before .NET Core and .NET Framework merge as .NET 5.0. In order to pull the image locally, we can run 

docker pull mcr.microsoft.com/dotnet/core/sdk:3.1

```s
C:\src\_blog\aspnet-docker\webapp1>docker pull mcr.microsoft.com/dotnet/core/sdk:3.1
3.1: Pulling from dotnet/core/aspnet
c499e6d256d6: Pull complete
251bcd0af921: Pull complete
852994ba072a: Pull complete
f64c6405f94b: Pull complete
9347e53e1c3a: Pull complete
Digest: sha256:31355469835e6df7538dbf5a4100c095338b51cbe52154aa23ae79d87585d404
Status: Downloaded newer image for mcr.microsoft.com/dotnet/core/aspnet:3.1
mcr.microsoft.com/dotnet/core/aspnet:3.1
```
TIP: This is an optional step. As we'll see real when we build our image, Docker pulls the image from the remote host if it doesn't exist locally.

To confirm our image sits in our local repo, run:
docker image ls




## Creating our App
As always, we'll use the CLI to create our MVC web app. Open a terminal and type:
dotnet new mvc -o webapp1
```s
C:\src\_blog\aspnet-docker>dotnet new mvc -o mvcapp
The template "ASP.NET Core Web App (Model-View-Controller)" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore/3.1-third-party-notices for details.

Processing post-creation actions...
Running 'dotnet restore' on webapp1\webapp1.csproj...
  Restore completed in 110.69 ms for C:\src\_blog\mvcapp\mvcapp.csproj.

Restore succeeded.
```

If you want, you can test it with `dotnet run`:
```s
C:\src\_blog\aspnet-docker\webapp1>dotnet run
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\src\_blog\mvcapp\mvcapp
```


# Containerizing our web application
Let's than containerize our application. We'll see this in two different ways:
    1 - publishing and building an image from the published project (simpler)
    2 - having Docker publish and run for us (more complicated)
    
Learning this is a required step for those looking to get into microservices. And containers are the new deployment unit.

On this exercise we'll break the procedure to build images in two steps (1) building our app using `dotnet` and (2) building the image using `docker build`


## Creating our first Dockerfile
Dockerfile is the standard used by Docker to perform tasks to build our images. Think of it as a script containig a series of operations (and configurations) Docker will use. For our super-simple web app, ours Dockerfile should be similar to:

```s
FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
WORKDIR /source

COPY mvcapp/. ./mvcapp/
WORKDIR /source/mvcapp
RUN dotnet restore
RUN dotnet publish -c release -o /app --no-restore

# final stage/image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build /app ./
ENTRYPOINT ["dotnet", "mvcapp.dll"]
```

## Building our first image
Let's build our image. The simplest command we can run is:
`docker build -t mvcapp .`

```s
C:\src\_blog\aspnet-docker\webapp1>docker build . -t mvcapp

Sending build context to Docker daemon  4.391MB
Step 1/8 : FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build
 ---> fc3ec13a2fac
Step 2/8 : WORKDIR /source
 ---> Using cache
 ---> d37458dbd2e7
Step 3/8 : COPY *.* /source
When using COPY with more than one source file, the destination must be a directory and end with a /
```

### About our first Dockerfile
What are we doing up there? Essentially, on line:
    1. we're telling docker to use the base aspnet3.1 core image
    2. setting the workdir to `/app`. This is internal to our image and is the location our local files will be copied to
    3. the command to run when the container starts (`dotnet mvcapp.dll`)
    4. the action to copy all files from the rel folder into the container 

### Building our image
All good! Let's create our image with `docker build -t mvcapp .`. This essentially tells docker to build an image based on the contents of the local folder `.` and tag it as webapp1. It could be anything we want but let's keep it consistent, shall we?

### Validating our build
Let's validate our build before running with:
`docker image ls`

### Running our image
Okay, now the grand moment! Let's run our image. Run it with:
`docker run -d -P webapp1`

About the params:
    * -d: run in detached moded (in the background)
    * -P: expose the same ports as specified by the image


### Validating our container
If the image ran successfully, we'd now have a running container. You can list active containers with:
`docker container ls`

If you don't see anything there, it probably failed. List all containers with the command below and copy its name (it's a fancy name auto-generated by docker located on the last column):
`docker container ls -a`

Then, let's review the logs with:
`docker container logs <name-of-your-container>

```s
C:\src\_blog\aspnet-docker\webapp1>docker container logs hardcore_gagarin
  It was not possible to find any installed .NET Core SDKs
  Did you mean to run .NET Core SDK commands? Install a .NET Core SDK from:
      https://aka.ms/dotnet-download
```









## Understanding the previous operations
Let' now take a look at what each of these commands do:
    1. FROM tells Docker that to pull the base image on the existing image, call microsoft/aspnetcore:1.0.1. This image already contains all the dependencies for running the ASP.NET Core on Linux, so we don't have to set it.
    2. COPY and WORKDIR copy the current directory's contents to a new directory inside the called/app container and set it to the working directory for subsequent instructions.
    3. EXPOSE tells Docker to expose the product catalog service on port 80 of the container.
    4. ENTRYPOINT specifies the command to execute when the container starts up. In this case, it's .NET.


## Running the Image
Let's run the image now. The simplest command we can run is:
dotnet run 



## Environment variables
Environments
ASP.NET Core reads the environment variable ASPNETCORE_ENVIRONMENT at app startup and stores the value in IWebHostEnvironment.EnvironmentName. ASPNETCORE_ENVIRONMENT can be set to any value, but three values are provided by the framework:

SOURCE: https://docs.microsoft.com/en-us/aspnet/core/fundamentals/environments?view=aspnetcore-3.1




# References
    * Donet Core Official Image | DockerHub https://hub.docker.com/_/microsoft-dotnet-core
    * Dockerize an ASP.NET Application | Docker https://docs.docker.com/engine/examples/dotnetcore/#introduction
    * Host ASP.NET Core in Docker containers | Microsoft https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/?view=aspnetcore-3.1
    * Donet run | Microsoft https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-run
    * Dockerfile reference | Docker https://docs.docker.com/engine/reference/builder/
    * Host ASP.NET Core in Docker containers | Microsoft https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/docker/?view=aspnetcore-3.1
    * Dotnet Docker | Github https://github.com/dotnet/dotnet-docker
    * 
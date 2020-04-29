# Blog - Microservices Project

What we'll build:
    * Frontend: asp net mvc + react:
        GET Home: subscribe to newsletter
        POST subscribe: send msg backend
        
    * Backend: web api + mongodb


# Build and run


## Running Locally


## Changing Configuration
On the command line:
dotnet run DbSettings:ConnStr="mongodb://brunobruno:12345"

With env varialbes:
TODO


    Docker compose, run local
    Services:
        Frontend
        Backend
        Mongodb
        RabbitMQ

Deployment

    Azure app services + docker compose
    Azure AKS

Posts

    Building a full microservice application using asp.net core, mongodb and RabbitMQ 
    Deploying microservices to Azure app services using docker compose
    Deploying microservices to Azure AKS






# References
* Create a web API with ASP.NET Core and MongoDB | https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mongo-app?view=aspnetcore-3.1&tabs=visual-studio
* Create a web API with ASP.NET Core and MongoDB
https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mongo-app?view=aspnetcore-3.1&tabs=visual-studio-code 
* RabbitMQ  .NET/C# Client API Guide | https://www.rabbitmq.com/dotnet-api-guide.html
* RabbitMQ .NET Hello World | https://www.rabbitmq.com/tutorials/tutorial-one-dotnet.html
* RabbitMQ at DockerHub | https://registry.hub.docker.com/_/rabbitmq/
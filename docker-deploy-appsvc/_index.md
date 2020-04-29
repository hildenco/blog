# Deploying Docker images to Azure App Services



We've been discussing Docker, containers and microservices for some time on the blog. On previous posts we learn how to create our own ASP.NET Docker images and learned how to push them to Azure Container Registry. Today we'll learn how to deploy these same Docker images on Azure App Services.

On this post we will:
Review how to create an Azure App Service


## Requirements
As requirements, please make sure you have:
git installed
Docker Desktop (Windows or Mac) or Docker-CE (Linux)
An Azure Account
Images already pushed to your Azure Container Registry (check this post to learn how)

## Azure App Services
Azure developers are quite familiar with Azure App Services. App services are []:
    HTTP-based services for hosting web applications, REST APIs, and mobile back ends. You can develop in your favorite language, be it .NET, .NET Core, Java, Ruby, Node.js, PHP, or Python. Applications run and scale with ease on both Windows and Linux-based environments. For Linux-based environments, see App Service on Linux.

    App Service not only adds the power of Microsoft Azure to your application, such as security, load balancing, autoscaling, and automated management. You can also take advantage of its DevOps capabilities, such as continuous deployment from Azure DevOps, GitHub, Docker Hub, and other sources, package management, staging environments, custom domain, and SSL certificates.

### Why use App Services
Essentially because App Services:
support multiple languages and frameworks: such as ASP.NET, Java, Ruby, Python and Node.js
can be easily plugged into your CI/CD pipelines
offer global scaling
can be used as serverless
can be managed and accessed in multiple ways from Azure 

## Creating our App Service
While this shouldn't be new to anyone, I'd like to review the workflow so readers understand the step-by-step. To create your App Service, in Azure, click Create -> App Service:

IMG-01

On this screen, make sure you select: 
Publish: Docker Container 
OS: Linux
Free plan: by clicking on Change plan.

Next, we specify Docker information. On this step we will choose:
Options: Single Container
Image Source: Azure Container Registry
Registry: Choose yours

Warning: on this part, if your repo doesn't have admin setup, you'll have to enable it.


### How to enable Admin in your Azure Container Registry
This is simple. Just go to your registry and on the 



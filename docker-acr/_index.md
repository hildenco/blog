Pushing .NET Docker images to Azure Container Registry




On a previous post we reviewed how to create our own ASP.NET containes using Docker. Turns out that that's just the beginning. Pushing our images to Azure Container Registry allows us to share our images with co-workers and use our images in deployments including Azure App Services and the new Azure Container Instances.

On this post we will:
learn how to create our custom container registry
push and pull our Docker images to/from it

Requirements
For this post you'll need:
* git
* Docker Desktop
* Azure CLI [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
* An Azure account
* A small image on your local Docker repository to push

Azure Container Registry (ACR)
If you recall, I recently reviewed 5 alternatives to Docker Hub and Azure Container Registry (ACR) was one of those. ACR together with Amazon XXXX, Red Hat's Quay and Google Container Registry (GCR), all provide valuable, equivalent and good alternatives to Docker Hub. Google has [a good definition for what a container registry is](https://cloud.google.com/container-registry/):
    A Container Registry is a single place for your team to manage Docker images, perform vulnerability analysis, and decide who can access what with fine-grained access control. Existing CI/CD integrations let you set up fully automated Docker pipelines to get fast feedback.

The main advantages for creating our own container registry are:
have our own private repository
use a cloud-hosted service, alleviating our ops team
have automated vulnerability scans for our own images
use it as a source of deployment for our artifacts

Creating our Azure Container Registry
There are essentially two ways to create our own container registries within Azure: using the Azure CLI and using the Azure Portal. As the Azure CLI alternative is very well documented [todo::addlink], let's review how to do it using the portal today.

As expected, it's very simple to create our own container registry from the portal. Click New Resource, type Container Registry and you should see:
IMG01

Click Create and enter the required information:
IMG02

Review and confirm, the deployment should take a few seconds:
IMG03

Owr Container Registry
After the deployment finishes, you're brought to your own container registry. Here's what it looks like, with my custom url highlighted in yellow:


TIP: I like pinning resources by project on a custom Dashboard. Fore more information, [check this link](https://docs.microsoft.com/en-us/azure/azure-portal/azure-portal-dashboards).


Log in to registry
Before pushing and pulling our images it will be necessary to login to our ACR instance using the Azure CLI [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli]. Open a Windows terminal and type

az acr login --name <your-acr-name>

TIP: for me, az acr login worked without entering username/password.

Building our images
Let's now push our first image to our registry. For that, you'll need an image on your local repo.

Pulling a sample .NET image
If you followed my previous post, you probably have our webapp ASP.NET Core 3.1 image on your Docker repository. If not, please run:

git clone https://github.com/hd9/aspnet-docker
cd aspnet-docker
docker build . -t webapp

Then type the below command to confirm your ASP.NET Core website image called webapp is there:
docker image ls 
IMG05

Pushing our image
Time to push our image. The first thing to do is to tag our image with the repo name and add a reasonable version to it:
docker tag webapp <acrname>.azurecr.io/webapp:v1

TIP: you have to tag the image with the full domain name (ie, include .azurecr.io) in the name else, Docker will try to push it to Docker Hub.

Confirm your image is correctly tagged with:
IMG06

Then, push the image to the remote repo:
docker push <acrname>.azurecr.io/webapp:v1
IMG07

If all went well, you should see on the Services/Repositories tab, webapp as a repository:
IMG08

And, clicking on our repo, we see info about our image:
IMG09


Pulling and running our image
Let's then proceed to the last step of this tutorial: pulling and our image. As you might expect, pulling and running this image on our local environment is a no-brainer:
1. simply pull the remote image `docker pull hildenco.azurecr.io/webapp:v1` and 
2. run the image afterwards with `docker run ...`.

However, I'd like to do this differently. I'll pull and run this image on a CentOS VM. If you want to follow along, please install in our CentOS VM:
* Docker [https://www.linuxtechi.com/install-docker-ce-centos-8-rhel-8/]
* the Azure CLI [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-yum?view=azure-cli-latest]
* Optional: add your user to the docker group with `sudo usermod -aG <username>`

Authenticating with Azure 
If you recall, we had to login to our repo from our development box. Turns out that our CentOS VM is another host so we'll have to login again first with the main az login:
az login

Then, on ACR:
az acr login --name <your-acr-name>

The image below shows both steps:


Pulling our Image from CentOS
Pulling the image from our CentOS should be:
docker pull hildenco.azurecr.io/webapp:v1

Then, we can run it with:
docker run --rm -d -p 8080:80 hildenco.azurecr.io/webapp:v1

Where:
--rm: remove container when stopped
-d: run in detached (background) mode
-p 8080:80: map port 80 on container to external 8080 on host

Conclusion
On this article we described how to host our images using Azure Container Registry. We reviewed how to create our own container registry on Azure and pushed a Docker image previously created (and available on GitHub) based on an ASP.NET Core 3.1 project to it. Lastly, instead of running the image locally, we pulled and ran it from a local CentOS VM hosted on Hyper-V but that could be deployed on the cloud of your choice.

Source Code
As always, the source code for this post is available on GitHub.

References
Create a private container registry using the Azure portal | Microsoft https://docs.microsoft.com/en-us/azure/container-registry/container-registry-get-started-portal
Build a custom image and run in App Service from a private registry https://docs.microsoft.com/en-us/azure/app-service/containers/tutorial-custom-docker-image 
Azure CLI [https://docs.microsoft.com/en-us/cli/azure/install-azure-cli
Install Azure CLI with yum | https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-yum?view=azure-cli-latest
AZ ACR CLI | Microsoft https://docs.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest
How to Install Docker CE on CentOS 8 / RHEL 8 https://www.linuxtechi.com/install-docker-ce-centos-8-rhel-8/

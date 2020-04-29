# Docker for dotnet developers
Much is being said about docker, containers, microservices and container orchestration these days. On this post, let's try to understand how all of that affects us, .NET developers and how we can leverage this tools today and use them to create better, scalable systems tomorrow.


# Installation
So, the first thing to do is installing Docker. On this post, I will cover how to install it on Linux (Ubuntu, Fedora/CentOS) and Windows. Mac users, feel free to follow the installation guidelines on the official page[]

## Installing on Windows


## Installing on Ubuntu/Mint/PopOS


## Installing on Fedora




# Docker Service
Docker is composed of two essential tools named... docker. One runs a a service (daemon on Unix terminology) and the other is the client (what we use to issue commands to the backend service). It's important to know that because sometimes the client will error while the problem lies on the service. So let's learn how to engage with the service:


## Managing the Docker service on Linux 
To manage the docker service on Linux (distros running systemd), do:
sudo systemctl status docker

The equivalents to stop/start the service is:
sudo systemctl start docker
sudo systemctl stop docker

## Managing the Docker service on Windows
On windows, open your services (quick: Start -> Run -> services.msc), right-click Docker Desktop Service to start/stop/reinitialize, etc.

## Managing Docker service on Alpine Linux
sudo service status docker                         # prints the status of the service
sudo service start docker                          # start the service
sudo service stop docker                           # stop the service

# Our first example
So let's run our first example: running a simple hello-world provided by Docker. Open a terminal and type:
docker run hello-world

So let's quickly review what happened with the command above:
1. docker checked if the hello-world image was available on your local repository
2. if not, it downloaded it from the default remote repo (Docker Hub)
3. after downloaded, it ran it
4. the container (not image) printed some test data for us

# Real world examples
While that was cool in the first time, it doesn't teach us much so let's do something else. What about running MongoDb? Simple:
docker run mongodb-latest -d -p 8080:8080 


Notes about the above command line


# Images





# Images x Containers
In Docker, it's alsi 



# Conclusion
This post was an attempt to provide a quick  (re)introduction about Docker for developers. As explained on a previous post, Docker is not the only option for containerized applications (neither the best) but is still a very mature, capable and mature technology which I quite like. For more information about alternatives to Docker please click here.

We will keep exploring Docker, Containers, Cloud and orchestration techologies (such as Swarm and Kubernetes) on this blog on the next posts so keep tuned!

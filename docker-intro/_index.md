# Containers and Microservices: A (re)introduction

But before we start, let's quickly review container history.

## Container History
A decade ago, Solomon Hykes’ invention of Docker containers had an analogous effect: With a dab of packaging, any Linux app could plug into any Docker container on any Linux OS, no fussy installation required. Better yet, multiple containerized apps could plug into a single instance of the OS, with each app safely isolated from the other, talking only to the OS through the Docker API.

## Why Containers
That shared model yielded a much lighter weight stack than the VM (virtual machine), the conventional vehicle for deploying and scaling applications in cloudlike fashion across physical computers. So lightweight and portable, in fact, that developers could work on multiple containerized apps on a laptop and upload them to the platform of their choice for testing and deployment. Plus, containerized apps start in the blink of an eye, as opposed to VMs, which typically take the better part of a minute to boot.

To grasp the real impact of containers, though, you need to understand the microservices model of application architecture. Many applications benefit from being broken down into small, single-purpose services that communicate with each other through APIs, so that each microservice can be updated or scaled independently (versus traditional monolithic applications, where changes require you to bring down and tinker with the whole deal). As it turns out, microservices and containers are a perfect fit.

## How's adoption of containers?
A few years ago, if you'd heard of Docker or containers, you'd thought of shipping containers. My how things have changed! In the latest Cloud Native Computing Foundation (CNCF) survey, it found 84% of companies are using containers in production this year -- up from 23% in the first survey in 2016. And what are they using to manage them? The vast majority (78%) are using Kubernetes.

The Docker Index [https://www.docker.com/blog/introducing-the-docker-index/] also provides very important statistics. According to it, just from Docker Hub we had 130 Billion pulls. On the Docker Desktop side, 69% of users run Mac, 39% are running Windows.

TODO::SHOW THIS IMAGE: https://i2.wp.com/www.docker.com/blog/wp-content/uploads/2020/01/Docker-Stat-Graphic-012920-V1_Blog-Mainstream-and-Growing-scaled.png?resize=1110%2C740&ssl=1


## Is Docker the only option?
This is another big misconception. Container != Docker. Docker is just another framework that implements OCI, the open container initiative. It's important to register two things: 
1. the main types of containers
2. alternatvites to docker

## More complex architectures
But how do you get containerized microservices to work in concert as an application? That’s where, at least for larger microservices applications, Kubernetes comes in. This open source orchestration engine enables you to deploy, manage, scale, and ensure the availability of a microservices-based application – and move it all of a piece across platforms if you need to.

## A new standard
Regardless if we need Kubernetes or not [https://www.infoworld.com/article/3531449/containers-march-into-the-mainstream.html], the microservices era is upon us and the ability to scale or swap in new services on the fly is essential for a big swath of modern applications. No matter how those services are managed, containers have established themselves as their standardized, streamlined receptacles.

## Toolchains
In shipping code-to-cloud, developers want the freedom to select their own tools for each stage of their app delivery toolchains, and there are rich breadth and depth of innovative products from which to select. But integrating together multiple point products across the toolchain stages of source code management, build/CI, deployment, and others can be challenging. Often, it results in custom, one-off scripts that subsequently need to be maintained, lossy hand-off of app state between delivery stages, and subpar developer experiences.

## CaaS (containers-as-a-service)
At the other end of that spectrum are the CaaS (containers-as-a-service) offerings from cloud providers, perhaps more accurately described as Kubernetes-as-a-service solutions. Amazon Web Services, Google Cloud Platform, and Microsoft Azure all offer their own CaaS flavors.

## From local to the cloud
Also, it's important to factor that going from code to cloud is complex. There are many choices across packaging, inner loop, packaging, registry, CI, security, CD, and public cloud runtimes. We will cover some of them in the future but I'd like you to realize that as with any new technology, there are barriers to surpass.

## Orchestration
With time, we'll see that the number of services will grow. Just as an idea, it's estimated that company like Google runs billions of containers. How in the world is that managed? The answer comes with orchestration technologies such as Kubernetes and Docker Swarm. Remember, Docker allows us to containerize and run our apps. Kubernetes (and equivalents) are a platform to orchestrate and manage these containers. 

## Monitoring
With that said, our services will not only grow in number but also get more complex. So how do we monitor all of that? On top of techs such as Kubernetes, there are also others that facilitate monitoring and logging. Plus, the business will ask for insights on how the services are performing.

At the moment, the leading open-source tools are:
Prometheus https://prometheus.io/
Grafana https://grafana.com/
Elastic Stack https://www.elastic.co/products/
Sensu Go https://sensu.io/
Sysdig Inspect https://sysdig.com/opensource/inspect/
Jaeger https://www.jaegertracing.io/

## Better Software
better software that can be updated and improved at a faster pace.

## Immutable operating systems 
I hear a trend about Windows supporting containerized applications. Excellent news for Windows users but, let's remember that Linux (and BSD) users had these features for a loooooong time already. Currently deployed and used are:
    * Flatpak, Snap
    * Fedora Silverlight
    * CoreOS
    * Windows 10X - Microsoft has introduced a novel type of container that ensures legacy applications run properly on the innovative Windows 10X operating system for dual-screen devices


## When should you use containers
While containerization should be the end goal in most cases from an operations perspective and offers huge dividends with minimal effort when injected into the development process, turning deployment machines into a containerized platform is a pretty tricky process, and if you will not gain tangible benefits from it, you might as well dedicate this time to something that will bring real and tangible value to your services.


If your services as a whole can completely fit and run well on a relatively small or medium virtual machine or a bare-metal host and you don't anticipate sudden scaling needs, containers may not be a good idea due to the complexity of the new process. For example,  you could scale up your services and save dev efforts by avoiding the transition. 


With this all, what are the clear signs that you need to get containers into your workflow as soon as you can? There can be many subtle hints here but the following list covers the ones that should immediately bring the containers topic up for discussion if the answer is yes, as the benefits greatly outweigh the time investment into your service platform:

    Do you have more than 10 unique, discrete, and interconnected services in your deployment?
    Do you have three or more programming languages you need to support on the hosts?
    Are your ops resources constantly deploying and upgrading services?
    Do any of your services require "four 9s" (99.99%) or better availability?
    Do you have a recurring pattern of services breaking in deployments because developers are not considerate of the environment that the services will run in?
    Do you have a talented Dev or Ops team that's sitting idle?

## The ideal enviroment
I'd like to propose some 
    Developers should be able to deploy a new service without any need for ops resources
    The system can auto-discover new instances of services running
    The system is flexibly scalable both up and down
    On desired code commits, the new code will automatically get deployed without Dev or Ops intervention
    You can seamlessly handle degraded nodes and services without interruption
    You are capable of using the full extent of the resources available on hosts (RAM, CPUs, and so on)
    Nodes should almost never need to be accessed individually by developers

## Advantages of containers
With Docker, you can: 

    have a container for each version of the OS that you can test the service against in order to ensure that you are not going to get any surprises. 
    you can automate a test suite runner to launch each one of the OS version containers one by one
    you can locally create Docker recipes (Dockerfiles), with the exact set of steps needed to get your service running to a fully capable of running service
    automated configuration management (CM) system, such as Ansible, Salt, Puppet, or Chef, to ensure that the hosts will have the exact same setup
    can also be used throughout the process to standardize your environments 
    Can increase the efficiency of almost every part of the deployment pipeline
    Easier to onboard new developers
    Simple to replicate complex environment with multiple dependencies (ex, elastic, redis,etc)
    Easier 

## Disadvantages of Containers

    Complex architecture
    More difficult to deploy
    Complex networking
    Complex dependency chain
    Overkill for simpler projects



## The ideal Docker deployment
ideal requirements to work with containers are:

    Developers should be able to deploy a new service without any need for ops resources
    The system can auto-discover new instances of services running
    The system is flexibly scalable both up and down
    On desired code commits, the new code will automatically get deployed without Dev or Ops intervention
    You can seamlessly handle degraded nodes and services without interruption
    You are capable of using the full extent of the resources available on hosts (RAM, CPUs, and so on)
    Nodes should almost never need to be accessed individually by developers

## Advantages of containers
With Docker, you can: 

    have a container for each version of the OS that you can test the service against in order to ensure that you are not going to get any surprises. 
    you can automate a test suite runner to launch each one of the OS version containers one by one
    you can locally create Docker recipes (Dockerfiles), with the exact set of steps needed to get your service running to a fully capable of running service
    automated configuration management (CM) system, such as Ansible, Salt, Puppet, or Chef, to ensure that the hosts will have the exact same setup
    can also be used throughout the process to standardize your environments 
    Can increase the efficiency of almost every part of the deployment pipeline
    Easier to onboard new developers
    Simple to replicate complex environment with multiple dependencies (ex, elastic, redis,etc)
    Easier 


## Docker for dotnet devs
What .NET devs should know about Docler:
Docker is a linux tool so some linux is required (apt, bash, etc)
Some scripting is necessary
Networking for more complex setups 
Understand your architecture
Volumes 
Clustering
Scaling 
Good to understand your deployment
Git
Dev workflow
Good support in azure
Runs okay in windows
Ansible,chef would be good to know



# Next steps
So, to finish up this very theoretical post, I'd like to list a roadmap of what we're discussin next:
An unnofficial (re)introduction to docker (installation, alternatives, requirements, simple tests, etc)
Docker for dotnet developers
Creating a asp.net core container (newsletter registration service)
Creating a dotnet core backend container using mongodb
Creating a sendgrid email notification container  
How to push images to azure container registry
How to deploy Docker images to azure containers
How to deploy deploy Docker images to AKS
Build a full microservice-based application leveraging all the above.
How to install docker
How to run a simple ASP.NET container


Sounds exciting? Sure! So keep tuned.

# Conclusion
On this post we 

# Ref
Kubernetes jumps in popularity | ZDNet https://www.zdnet.com/article/kubernetes-jumps-in-popularity/
Containers march into the mainstream https://www.infoworld.com/article/3531449/containers-march-into-the-mainstream.html
Do you really need Kubernetes? https://www.infoworld.com/article/3527217/do-you-really-need-kubernetes.html
Helping You and Your Development Team Build and Ship Faster https://www.docker.com/blog/docker-strategy-helping-devs-build-and-ship-faster/
Top Six Open Source Tools for Monitoring Kubernetes and Docker https://devops.com/top-six-open-source-tools-for-monitoring-kubernetes-and-docker/

# See Also


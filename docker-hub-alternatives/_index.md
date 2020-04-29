

Everyone today has used Docker Hub. Docker Hub is a container registries. Did you know that there are many free and reasonabily cheap alternatives to it? On this post let's learn about them.

 


Container Registries

Container Registries allow developers to store their Docker (and OCI-equivalent images) on a central repository. Container registries build, store, secure, scan, replicate, and manage container images and artifacts with a fully managed, geo-replicated instance of OCI distribution.

 
What are managed Container Registries?

Managed container registries are nothing more than  XXXX hosted on the cloud. However, they provide a lot of benefits, are usually not expensive and very much recommended.
Why use a Managed Container Registry

As with any other cloud services, there are benefits in using a managed (cloud-based) container registry because managed container registries are:

    Fully-managed: by using fully managed registries, you can release your ops team from maintaining your own repo
    Secured: you can use cloud firewall to protect your services
    Automated vulnerability scans: some registries will automatically scan your images and alert you on your 
    Geo-replicated: got a team distributed around the world? A geo-replicated container registry may speed up things for teammebers as it'll be sitting beside them.
    Integrated security: it's common to have some sorte of authentication, role-based access control and virtual network integration
    Integrated with your cloud: most managed container registries will provide some integration with your cloud meaning that'll be easier to share and deploy those images to your environments.

Docker Hub

Docker Hub is the world's most popular Docker container registry. With it you can create, manage, and deliver your teams' container applications. Docker Hub is also where you'll get official images for the most popular images such as CentOS, Python, Golang, Ubuntu, MariaDb, nginx, Node, Alpine, MongoDB and more!
Google Container Registry

Google Container Registry (GCR) - GCR is Google's container history. Thery you can store your images, perform vulnerability analysis, and manage access control. It also supports CI/CD integrations so you can fully automate your pipelines.
Amazon Elastic Container Registry

Amazon Elastic Container Registry (ECR) - ECR is a fully-managed container registry that makes it easy for developers to store, manage, and deploy your images. ECR is integrated with Amazon Elastic Container Service (ECS), simplifying your development to production workflow. Amazon ECR eliminates the need to operate your own container repositories or worry about scaling the underlying infrastructure.
Azure Container Registry

Azure Container Registry (ACR) is another a fully-managed Docker container registry allowing you to build, store, secure, scan, replicate, and manage container images and artifacts with a fully managed, geo-replicated instance of OCI distribution. ACR allows you to connect across environments, including Azure Kubernetes Service and Azure Red Hat OpenShift, and across Azure services.
Quay

Quay allows you to Store your containers on private and public repos. Quay also allows you to automate your container builds, and integrates with GitHub and others. Quay also provides automated scan containers for vulnerabilities and other tools.
Red Hat Container Registry

For RHEL users, Red Hat has also their own containter repository called Red Hat Container Registry. You will find everything here, not only RH-related stuff.




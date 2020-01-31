# Monolithic application deployment challenges

Monolithic applications are applications where all of the database and business logic is tied together and packaged as a single system. Since, in general, monolithic applications are deployed as a single package, deployments are somewhat simple but painful due to the following reasons:

## Deployment and release as a single concept
There is no differentiation between deploying build artifacts and actually making features available to the end user. More often, releases have coupling to the environments. This increases the risk of deploying new features.

## All or nothing deployment
All or nothing deployment increases the risk of application downtime and failures. In the case of rollbacks, teams fail to deliver the expected new features. Besides, hotfixes, or service packs become the norm to deliver the right kind of functionality.

## Central databases as a single point of failure
In monolithic applications, a big, centralized database is a single point of failure. This database is quite often large and difficult to break down. This results in an increase in mean time to recover (MMTR) and mean time between failures (MTBF).

## Deployment and releases are big events
Due to small changes in the application, the entire application could get deployed. This comes with developers and ops teams' huge time and energy investment. Needless to say, collaboration between the various teams involved is the key for a successful release. This becomes even harder when many geo-distributed teams are working on the development and release. These kinds of deployments/releases use a lot of handholding and manual steps. This impacts the end customers who have to face application downtime. If you are familiar with these kinds of deployments, then you'll also be familiar with marathon sessions in the so-called war rooms and endless sessions of defect triage on conference bridges. 

## Time to market
Carrying out any change to the system in such cases becomes harder. In such environments, executing any business change takes time. This makes responding to market forces difficult--the business can also lose its market share. With the microservice architecture, we are addressing some of these challenges. This architecture provides greater flexibility and isolation for service deployment. It has proven to deliver much faster turnaround time and much needed business agility.

# Prerequisites for successful microservice deployments

Any architectural style comes with a set of associated patterns and practices to follow. The microservice architectural style is no different. Microservice implementation has more chances of being successful with the adoption of the following practices:

    * Self-sufficient teams: Amazon, who is a pioneer of SOA and microservice architectures, follows the Two Pizza Teams paradigm. This means usually a microservice team will have no more than 7-10 team members. These team members will have all the necessary skills and roles, for example, development, operations, and business analyst. Such a service team handles the development, operations, and management of a microservice. 
    * CI and CD: CI and CD are prerequisites for implementing microservices. Smaller self-sufficient teams, who can integrate their work frequently, are precursors to the success of microservices. This architecture is not as simple as monolith. However, automation and the ability to push code upgrades regularly enables teams to handle complexity. Tools, such as TFS, Team Foundation Online Services, Teamcity, and Jenkins, are quite popular toolchains in this space.
    * Infrastructure as code: The idea of representing hardware and infrastructure components, such as networks with code, is new. It helps you make deployment environments, such as integration, testing, and production, look exactly identical. This means developers and test engineers will be able to easily reproduce production defects in lower environments. With tools such as CFEngine, Chef, Puppet, Ansible, and Powershell DSC, you can write your entire infrastructure as code. With this paradigm shift, you can also put your infrastructure under a version control system and ship it as an artifact in deployment.
    * Utilization of cloud computing: Cloud computing is a big catalyst toward adopting microservices. It is not mandatory as such for microservice deployment, though. Cloud computing comes with near infinite scale, elasticity, and rapid provisioning capability. It is a no brainer that the cloud is a natural ally for microservices. So, knowledge and experience with the Azure cloud will help you adopt microservices.



# Reviewing microservices for .NET developers

# Benefits of a Microservice-styled architecture

Apart from some of the definite advantages of SOA, microservices provide certain additional differentiating factors that makes it a clear winner. At the core, microservices were defined to be completely independent of other services in the system and run in their process. The attribute of being independent required certain discipline and strategy in the application design. Some of the benefits it provides are:

    Clear code boundaries: This resulted in easier code changes. It has independent modules provided an isolated functionality that led to a change in one microservice has little impact on others.
    Easy deployment: It is possible to deploy one microservice at a time if required.
    Technology adaptation: The preceding attributes led to the gain of this much sought after benefit. This allows us to adopt different technologies in different modules.
    Affordable scalability: This allows us to scale only chosen components/modules instead of the whole application.
    Distributed system: This comes implied, but a word of caution is necessary here. Make sure that your asynchronous calls are used well and the synchronous ones don't block the whole flow of information. Use data partitioning well. We will come to this a little later, so don't worry for now.
    Quick market response: In a competitive world, this is a definite advantage as users tend to quickly lose interest if you are slow to respond to new feature requests or adopt a new technology within your system

# Messaging in microservices

This is another important area that needs its share of discussion. There are primarily two main types of messaging utilized in microservices:

    Synchronous
    Asynchronous

# Deployment

Monolith deployments for enterprise applications can be challenging for more than one reason. Having a central database, which is difficult to break down, only increases the overall challenge along with time to market.

For microservices, the scenario is much different. The benefits don't just come by virtue of the architecture being microservices. Instead, it is the planning from the initial stages itself. You can't expect an enterprise-scale microservice to be managed without continuous delivery (CD) and continuous integration (CI). So strong is the requirement for CI and CD right from the early stages that without it, the production stage may never see the light of the day.

Tools such as CFEngine, chef, puppet, ansible, and powershell DSC help you represent an infrastructure with code and let you easily make different environments exactly the same. Azure could be an ally here. The rapid and repeated provisioning required here could easily be met with it.


# Monitoring

The monolith world had a few advantages of its own. Easier monitoring and logging is one of those areas where things are easier compared to microservices. The sheer number of microservices across which an enterprise system might be spread can be mind-boggling.

Unlike a monolith architecture, monitoring is very much required from the very beginning in a microservice-based architecture. There is a wide range of reasons why monitoring can be categorized:

    Health: We need to preemptively know when a service failure is imminent. Key parameters, such as CPU and memory utilization, along with other metadata could be a precursor to either the impending failure or just a flaw in the service that needs to be fixed. Just imagine an insurance company's rate engine getting overloaded and going out of service or even performing slow when a few hundred field executives try to share the cost with the probable clients. Nobody likes to wait these days.
    Availability: There might a situation when the service may not perform extensive calculations, but the bare availability of the service itself might be crucial to the entire system. In such a scenario, I remember relying upon pings to listeners that would wait for a few minutes before shooting out e-mails to the system administrators. It worked for monoliths with one or two services to be monitored. However, with microservices, much more metadata comes into the picture.
    Performance: For platforms receiving high footfall, such as banking and e-commerce, availability alone does not deliver the service required. Considering the number of people converging at their platforms in very short spans, ranging from a few minutes to even tens of seconds, performance is not a luxury anymore. You need to know how the system is responding by means of data, such as concurrent users being served, and compare that with the health parameters in the background. This might provide an e-commerce platform with the ability to decide whether upgrades are required before the upcoming holiday season. For more sales, you need to serve a higher number of people.
    Security: In any system, you can plan resilience only up to a specific level. No matter how well designed a system is, there would be thresholds beyond which the systems will falter, which can result in a domino effect. However, having a thoughtfully designed security system in place could easily avert DoS and SQL Injection attacks. This would really matter from system to system when dealing with microservices. So think ahead and think carefully when setting up trust levels between your microservices. The default strategy that I have seen people utilizing is securing the endpoints with microservices. However, covering this aspect increases the depth of your system's security and is worthwhile spending some time with.
    Auditing: Domains such as healthcare, financing, and banking are a few domains that have the most strict compliance standards around all associated services. And it is pretty much the same world over. Depending upon the kind of compliance you are dealing with, you might have a requirement to keep the data for a specific period of time as a record, keep the data in a specific format to be shared with regulatory authorities, or even sync with systems provided by the authority. Taxation systems could be another example here. With a distributed architecture, you don't want to risk losing the data record set related to even a single transaction since that would amount to compliance failure.
    Troubleshooting system failures: This, I bet, would be a favorite for a long time to come to anybody who is getting started with microservices. I remember the initial days when I use to try troubleshooting a scenario involving two Windows services. I never thought of recommending a similar design again. But time has changed and so has the technology today.

# Reactive microservices

We have progressed well while transitioning our monolithic application to the microservice-styled architecture. We have also briefly touched upon the possibility of introducing reactive traits to our services. We now know what are the key attributes of reactive microservices are:

    Responsiveness
    Resilience
    Autonomous
    Being message-driven

We also saw the benefits of reactive microservices amounting to less work on our part when it comes to managing communication across/between the microservices. This benefit translates not just into reduced work but the capability to focus on the core job of executing the business logic instead of trying to grapple with the complexities of inter-service communication.


# Service Discovery

Some of the service discovery mechanisms are available within the open source community. They are as follows:

    Zookeeper: Zookeeper (http://zookeeper.apache.org/) is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services. It's written in Java, is strongly consistent (CP), and uses the Zab (http://www.stanford.edu/class/cs347/reading/zab.pdf) protocol to coordinate changes across the ensemble (cluster).
    Consul: Consul makes it simple for services to register themselves and discover other services via a DNS or HTTP interface. It registers external services, such as SaaS providers, as well. It also acts as a centralized configuration store in the form of key values. It also has failure detection properties. It is based on the peer-to-peer gossip protocol.
    Etcd: Etcd is a highly available key-value store for shared configuration and service discovery. It was inspired by Zookeeper and Doozer. It's written in go,and uses Raft (https://ramcloud.stanford.edu/wiki/download/attachments/11370504/raft.pdf) for consensus, and has an HTTP- plus JSON-based API.


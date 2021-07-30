# Lesson-4: Open Source PaaS

This wiki is built in Notion. Here are all the notes you need to continue.

---

Instructor: Katie Gamanji, Ecosystem Advocate  @CNCF

### Outcomes

- PaaS Mechanisms
- Cloud Foundry
- Function as a Service

# **PaaS Mechanisms**

### Context

Kubernetes offers a wide range of functionalities that form the principles of any modern infrastructure. These capabilities revolve around automation, portability, extensibility, flexibility, self-healing, and many more. However, managing Kubernetes at scale is challenging, especially when the clusters are self-hosted in datacenters or private clouds. In this case, a team has to keep up-to-date with the latest Kubernetes releases, to ensure the platform is updated, upgraded, managed, and configured to meet the production-grade standards.

Within any organization, a release process consists of releasing the application to multiple environments, such as sandbox, staging, and production. In this case, each environment is represented by a separate Kubernetes cluster, with a total of 3 clusters. However, the number of clusters grows exponentially if the platform is replicated across different regions. This is usually required to keep market proximity and deliver a service to consumers with minimal latency. As a result, if the infrastructure is distributed across the US, Europe, and the Asia Pacific, a team ends up operating 9 clusters. Configuring, managing, upgrading, updating, and deploying to all of these clusters is a herculean task and often requires a dedicated team.

In these circumstances, if an organization does not have sufficient engineering resources, delegating the platform management to a 3rd party provider is a more suitable solution. This is covered by a PaaS or Platform as a Service solution.

[]()

### Summary

The industry is abundant with cloud-computing offerings that offer variate level of attraction of services. Some of the widely used cloud-computing services are:
• **On-premise** - where an engineering team has full control over the platform, including the physical servers
• **IaaS** or **Infrastructure as a Service** - where a team consumes compute, network, and storage resources from a vendor
• **PaaS** or **Platform as a Service** - where the infrastructure is fully managed by a provider, and the team is focused on application deployment
Widely used cloud-computing offerings
Releasing a product to a production environment implies that a platform has been build to host this particular product. A platform consists of multiple services that need to be configured, wired, and maintained together. These services are:
• **Networking** - establish communication between internal and external systems, such as internet connection, firewalls, routers, and cables
• **Storage**- collect and store digital data, such as files, blocks, or objects
• **Servers** - physical machines that provide compute services for a platform
• **Virtualization** - abstracts physical servers, storage, and networking. For example, we have learned that hypervisors are used to virtualize servers.
• **O/S** - operating systems that connect the software to physical resources (e.g. Linux, Ubuntu, Windows, etc)
• **Middleware** - help the developers to build an application by making it easy to consume platform capabilities (e.g. messaging, API, data management)
• **Runtime** - execution context for an application. For example, using JVM (or Java Virtual Machine) as a Java runtime
• **Data** - tools for collection and storage of data that is required by an application during execution(e.g. MySQL, MongoDB, or CockroachDB)
• **Applications** - the business logic for a product
**On-premise**
On-premise represents a cloud-computing offering where the engineering team has full control of the platform services (from networking to applications). This solution is suitable for organizations that have sufficient engineering power and regulations that demand full control of their technology stack and operations within it.
**IaaS**
IaaS solutions provide the abstraction of networking, storage, server, and virtualization layers. As a result, these services are consumed on-demand by the engineering teams. Additionally, IaaS provides a suitable abstraction for the management of self-hosted Kubernetes clusters, which depend on compute, network, and storage components for a successful bootstrap process.
The most common IaaS solutions are delivered by public cloud providers such as AWS, GCP, Microsoft Azure, and many more.
**PaaS**
PaaS is a cloud-computing offering that enables an engineering team to fully focus on application development. It abstracts all services except the application and the data associated with it. As a result, the team is required to manage the code base and any database service that the product needs to be fully operational.
Popular PaaS solutions are App Engine from GCP, Heroku, Cloud Foundry, Beanstalk from AWS, and many more.
**Advantages**:
• Time-efficiency - engineering focus is shifter toward development rather than infrastructure management
• Scalability and high availability - on-demand resource consumption enables an application to easily scale and fail-over
• Rich application catalog - integration of external service (e.g. databases) with minimal effort
**Disadvantages**:
• Vendor lock-in - it is challenging to interchange PaaS providers without service disruption
• Data security - since data is managed by a 3rd party, an extra layer of complexity is added to ensure data confidentiality
• Operational limitations - the service catalog is limited to the services offered by the integrated cloud provider
Variate levels of service management from cloud-computing offerings
Throughout its evolution, an organization might use one or a subset of available cloud-computing services. It is essential to select an offering that closely meets business requirements. However, it is important to consider the following traits of cloud-computing services:
• The fewer components are delegated to external providers, the **more control** there is over available functionalities
• The more ownership there is across the stack, the **more complexity** is introduced in managing and delivering the product
• The fewer components are managed by an engineering team, the quicker is the **usability** of the stack. As such, with a PaaS offering the engineering team can deploy their application immediately. While if choosing an on-premise solution, the release of a product is possible only after the platform is built.
Complexity vs Usability for each cloud-computing offering
**New terms**
• **On-premise** - cloud-computing service, where a team owns the entire technology stack.
• **IaaS** - cloud-computing service that offers the abstraction of networking, storage, server, and virtualization layers.
• **PaaS** - cloud-computing service, where the infrastructure components are managed fully by a 3rd party provider, and a team manages only the application and the data associated with it.
**Further Reading**
Read more about on-premise, IaaS, and PaaS solutions:
• [On Premise Vs Cloud](https://www.ebcgroup.co.uk/news-insights/on-premises-vs-cloud)
• [Infrastructure as a Service](https://susedefines.suse.com/definition/infrastructure-as-a-service/)
• [Platform as a Service](https://susedefines.suse.com/definition/platform-as-a-service/)
• [Cloud Computing in 2021: What You Should Know about Public, Private, Hybrid, PaaS, SaaS and FaaS](https://www.suse.com/c/cloud-computing-in-2021-what-you-should-know-about-public-private-hybrid-paas-saas-and-faas/)**NEXT**

![https://video.udacity-data.com/topher/2020/December/5fe11cd8_screenshot-2020-12-21-at-22.08.13/screenshot-2020-12-21-at-22.08.13.png](https://video.udacity-data.com/topher/2020/December/5fe11cd8_screenshot-2020-12-21-at-22.08.13/screenshot-2020-12-21-at-22.08.13.png)

![https://video.udacity-data.com/topher/2020/December/5fe11def_screenshot-2020-12-21-at-22.12.32/screenshot-2020-12-21-at-22.12.32.png](https://video.udacity-data.com/topher/2020/December/5fe11def_screenshot-2020-12-21-at-22.12.32/screenshot-2020-12-21-at-22.12.32.png)

![https://video.udacity-data.com/topher/2020/December/5fe25ca6_screen-shot-2020-12-22-at-12.52.42-pm/screen-shot-2020-12-22-at-12.52.42-pm.png](https://video.udacity-data.com/topher/2020/December/5fe25ca6_screen-shot-2020-12-22-at-12.52.42-pm/screen-shot-2020-12-22-at-12.52.42-pm.png)

---

- **Cloud Foundry**: Cloud Foundry is an open-source PaaS, a stand-alone software package that can be installed on any available infrastructure; private, public, or hybrid cloud. Due to its open-source nature, there is no vendor lock-in and the community can contribute to its future releases and definition of the roadmap. As such, Cloud Foundry offers a production-grade release process through a solid developer experience, that enables any application to be deployed with ease.

*Note:* some offerings of Cloud Foundry, can be deployed on top of Kubernetes, meaning that its main components are running as pods within a cluster.

Cloud Foundry consists of multiple components that provide these core capabilities:

- **Routing** - handle the incoming external traffic and route it to applications
- **Authentication** - identity management to user accounts
- **Application lifecycle** - controls the application deployments, monitors their status, and reconciles any new changes to reach the desired state of the application.
- **Application storage and execution** - handle the availability of artifacts to applications
- **Service** - use service brokers to provisions on-demand the dependency services for an application, such as a database or third-party APIs
- **Messaging** - ensure the communication between Cloud Foundry components
- **Metrics and logging** - aggregates the system and application metrics and logs

In the following sections, we will explore Cloud Foundry using [SUSE Cloud Application Platform Developer Sandbox](https://developer.suse.com/capsandbox/). However, you can explore Cloud Foundry's functionalities using the following offerings as well:

- [IBM Cloud Foundry](https://www.ibm.com/cloud/cloud-foundry)
- [SAP Cloud Platform](https://www.sap.com/products/cloud-platform/get-started.html)
- [Anynines](https://paas.anynines.com/)

[]()

### Excercise

---

# FAAS(Function as a Service)

An organization will always explore the most efficient offering to deploy a product to consumers. PaaS solutions are lightweight on initial setup, as a team can release the code in production within days.

However, there are use cases where customers interact with a service only once a day or for a couple of hours within a day. For example, a service to update a timetable with the new bus schedule once a day. In this case, using a PaaS offering has one major downside: it is not cost-efficient. For example, with Cloud Foundry there will always be an instance of the application up and running, even if the service is used once a day. However, the team is billed for a full day.

For this scenario, a **FaaS** or **Function as a Service** is a more suitable offering. FaaS is an event-driven cloud-computing service that allows the execution of code without any management of the infrastructure and configuration files. As a result, the timetable update service is invoked only once a day, and for the rest of the time, there are no replicas of this service. A team will be billed only for the time the service is executed.

Popular FaaS solutions are AWS Lambda, Azure Functions, Cloud Functions from GCP, and many more.

Throughout the release process, a FaaS solution only requires the application code that is built and executed immediately. In comparison with a PaaS offering, this FaaS has a quicker usability rate, as no data management or configuration files are necessary.

![https://video.udacity-data.com/topher/2020/December/5fe1ddba_screenshot-2020-12-22-at-11.51.10/screenshot-2020-12-22-at-11.51.10.png](https://video.udacity-data.com/topher/2020/December/5fe1ddba_screenshot-2020-12-22-at-11.51.10/screenshot-2020-12-22-at-11.51.10.png)

FaaS dependencies to deploy an application

## **New Terms**

- **FaaS** or **Function as a Service** - event-driven cloud-computing service that requires only the application code to execute successfully.

## **Further Reading**

Explore FaaS in more detail:

- [What is Function-as-a-Service (FaaS)?](https://www.redhat.com/en/topics/cloud-native-apps/what-is-faas?source=searchresultlisting)

[]()

Throughout its evolution, an organization needs to periodically evaluate available cloud-computing services, to ensure that the business requirements are always fulfilled. The industry has an abundance of cloud-computing offerings, such as on-premise, IaaS, PaaS, and FaaS, with a rich collection of open-source and vendor managed tools. In this lesson, we analyzed Cloud Foundry, an open-source PaaS, that can be hosted on any available compute and provide a unified and powerful end-user experience. We have also explored, FaaS an event-driven offering, that increases the cost-efficiency of a platform.
Overall, throughout this lesson we explored:
• PaaS Mechanisms
• Cloud Foundry
• Function as a Service
**Glossary**
• **On-premise** - cloud-computing service, where a team owns the entire technology stack.
• **IaaS** or **Infrastructure as a Service** - cloud-computing service that offers the abstraction of networking, storage, server, and virtualization layers.
• **PaaS** or **Platform as a Service** - cloud-computing service, where the infrastructure components are managed fully by a 3rd party provider, and a team manages only the application and the data associated with it.
• **Cloud Foundry** - an open-source PaaS offering, that can be hosted on any available infrastructure
• **FaaS** or **Function as a Service** - event-driven cloud-computing service that requires only the application code to execute successfully.**NEXT**

---
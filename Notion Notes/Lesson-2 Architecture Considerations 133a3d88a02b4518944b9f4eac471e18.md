# Lesson-2: Architecture Considerations

This wiki is built in Notion. Here are all the notes you need to continue.

---

Instructor: Katie Gamanji, Ecosystem Advocate  @CNCF

### Outcomes-

- Monoliths and Microservices
- Trade-offs for Monoliths and Microservices
- Practices for Application Development

## Design Considerations for Cloud-Native Applications:

When building an application it is important to allocate time for context discovery. This includes listing all necessary functionalities of the application and enumerating any resources that can enable its buildout. This phase sets the fundamentals of the project. If properly implemented, it can enable the creation of services that are scalable, resilient, and extensible.

![Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/IMG_20210703_225101.jpg](Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/IMG_20210703_225101.jpg)

The first step in the context discovery process is to list the **functional requirements**, or what application capabilities should deliver to the end-users. For example, a good starting point is to expand on the following:

- Stakeholders
- Functionalities
- End users
- Input and output process
- Engineering teams

Having a good understanding of functional requirements and available resources can lead to a simpler choice between monolithic and microservice-based architectures.

The second step is to enumerate the **available resources** that facilitates the implementation of the project. For example, a good starting point is to list available:

- Engineering resources
- Financial resources
- Timeframes
- Internal knowledge

## Monolith & Microservices

- Monolith: It's  a application design where all application tiers are managed as a single unit

In a monolithic architecture, application tiers can be described as:

- part of the same unit
- managed in a single repository
- sharing existing resources (e.g. CPU and memory)
- developed in one programming language
- released using a single binary

![Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/Untitled.png](Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/Untitled.png)

- Microservices: It's an application design where application tiers are managed as independent, smaller units.

In a microservice architecture, application tiers are managed independently, as different units. Each unit has the following characteristics:

- managed in a separate repository
- own allocated resources (e.g. CPU and memory)
- well-defined API (Application Programming Interface) for connection to other units
- implemented using the programming language of choice
- released using its own binary

![Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/Untitled%201.png](Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/Untitled%201.png)

Also, each architecture encapsulates the 3 main tires of an application:

- UI (User Interface) - handles HTTP requests from the users and returns a response
- Business logic - contained the code that provides a service to the users
- Data layer - implements access and storage of data objects

---

## Trade-offs for Monolith & Microservices

The trade-offs cover development complexity, scalability, time to deploy, flexibility, operational cost, and reliability. Let's examine each trade-off in more detail!

![Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/IMG_20210703_224941.jpg](Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/IMG_20210703_224941.jpg)

### **Development Complexity**

Development complexity represents the effort required to deploy and manage an application.

- **Monoliths** - one programming language; one repository; enables sequential development
- **Microservice** - multiple programming languages; multiple repositories; enables concurrent development

### **Scalability**

Scalability captures how an application is able to scales up and down, based on the incoming traffic.

- **Monoliths** - replication of the entire stack; hence it's heavy on resource consumption
- **Microservice** - replication of a single unit, providing on-demand consumption of resources

### **Time to Deploy**

Time to deploy encapsulates the build of a delivery pipeline that is used to ship features.

- **Monoliths** - one delivery pipeline that deploys the entire stack; more risk with each deployment leading to a lower velocity rate
- **Microservice** - multiple delivery pipelines that deploy separate units; less risk with each deployment leading to a higher feature development rate

### **Flexibility**

Flexibility implies the ability to adapt to new technologies and introduce new functionalities.

- **Monoliths** - low rate, since the entire application stack might need restructuring to incorporate new functionalities
- **Microservice** - high rate, since changing an independent unit is straightforward

### **Operational Cost**

Operational cost represents the cost of necessary resources to release a product.

- **Monoliths** - low initial cost, since one code base and one pipeline should be managed. However, the cost increases exponentially when the application needs to operate at scale.
- **Microservice** - high initial cost, since multiple repositories and pipelines require management. However, at scale, the cost remains proportional to the consumed resources at that point in time.

### **Reliability**

Reliability captures practices for an application to recover from failure and tools to monitor an application.

- **Monoliths** - in a failure scenario, the entire stack needs to be recovered. Also, the visibility into each functionality is low, since all the logs and metrics are aggregated together.
- **Microservice** - in a failure scenario, only the failed unit needs to be recovered. Also, there is high visibility into the logs and metrics for each unit.

Each application architecture has a set of trade-offs that need to be considered at the genesis of a project. But more importantly, it is paramount to understand how the application will be maintained in the future e.g. at scale, under load, supporting multiple releases a day, etc.

> There is no "golden path" to design a product, but a good understanding of the trade-offs will provide a clear project roadmap. Regardless if a monolith or microservice architecture is chosen, as long as the project is coupled with an efficient delivery pipeline, the ability to adopt new technologies, and easily add features, the path to could-native deployment is certain.

---

## Best Practices For Application Deployment

Refer to the previous handwritten note that covers - Best Practices like health checks, metrics, logs, tracing, and resource consumption.

### **Health Checks**

Health checks are implemented to showcase the status of an application. These checks report if an application is running and meets the expected behavior to serve incoming traffic. Usually, health checks are represented by an HTTP endpoint such as `/healthz` or `/status`. These endpoints return an HTTP response showcasing if the application is healthy or in an error state.

![https://video.udacity-data.com/topher/2020/December/5fdddf65_screenshot-2020-12-19-at-11.09.17/screenshot-2020-12-19-at-11.09.17.png](https://video.udacity-data.com/topher/2020/December/5fdddf65_screenshot-2020-12-19-at-11.09.17/screenshot-2020-12-19-at-11.09.17.png)

`/status` health check that showcases that the application is healthy

### **Metrics**

Metrics are necessary to quantify the performance of the application. To fully understand how a service handles requests, it is mandatory to collect statistics on how the service operates. For example, the number of active users, handled requests, or the number of logins. Additionally, it is paramount to gather statistics on resources that the application requires to be fully operational. For example, the amount of CPU, memory, and network throughput. Usually, the collection of metrics are returned via an HTTP endpoint such as `/metrics`, which contains the internal metrics such as the number of active users, consumed CPU, network throughput, etc.

![https://video.udacity-data.com/topher/2020/December/5fdde746_screenshot-2020-12-19-at-11.42.51/screenshot-2020-12-19-at-11.42.51.png](https://video.udacity-data.com/topher/2020/December/5fdde746_screenshot-2020-12-19-at-11.42.51/screenshot-2020-12-19-at-11.42.51.png)

`/metrics` endpoint that list of metrics counting the amount requests by the HTTP code returned

### **Logs**

Log aggregation provides valuable insights into what operations a service is performing at a point in time. It is the nucleus of any troubleshooting and debugging process. For example, it is essential to record if a user logged in successfully into a service, or encountered an error while performing a payment.

Usually, the logs are collected from STDOUT (standard out) and STDERR (standard error) through a passive logging mechanism. This means that any output or errors from the application are sent to the shell. Subsequently, these are collected by a logging tool, such as Fluentd or Splunk, and stored in backend storage. However, the application can send the logs directly to the backend storage. In this case, an active logging technique is used, as the log transmission is handled directly by the application, without a logging tool required.

There are multiple logging levels that can be attributed to an operation. Some of the most widely used are:

- **DEBUG** - record fine-grained events of application processes
- **INFO** - provide coarse-grained information about an operation
- **WARN** - records a potential issue with the service
- **ERROR** - notifies an error has been encountered, however, the application is still running
- **FATAL** - represents a critical situation, when the application is not operational

As well, it is common practice to associate each log line with a **timestamp**, that will exactly record when an operation was invoked.

![https://video.udacity-data.com/topher/2020/December/5fddef83_screenshot-2020-12-19-at-12.18.00/screenshot-2020-12-19-at-12.18.00.png](https://video.udacity-data.com/topher/2020/December/5fddef83_screenshot-2020-12-19-at-12.18.00/screenshot-2020-12-19-at-12.18.00.png)

Multiple INFO log lines recorded when a Prometheus service started

### **Tracing**

Tracing is capable of creating a full picture of how different services are invoked to fulfill a single request. Usually, tracing is integrated through a library at the application layer, where the developer can record when a particular service is invoked. These records for individual services are defined as spans. A collection of spans define a trace that recreates the entire lifecycle of a request.

### **Resource Consumption**

Resource consumption encapsulates the resources an application requires to be fully operational. This usually refers to the amount of CPU and memory that is consumed by an application during its execution. Additionally, it is beneficial to benchmark the network throughput, or how many requests can an application handle concurrently. Having awareness of resource boundaries is essential to ensure that the application is up and running 24/7.

![https://video.udacity-data.com/topher/2020/December/5fddf5b1_screenshot-2020-12-19-at-12.44.19/screenshot-2020-12-19-at-12.44.19.png](https://video.udacity-data.com/topher/2020/December/5fddf5b1_screenshot-2020-12-19-at-12.44.19/screenshot-2020-12-19-at-12.44.19.png)

A graph showcasing the CPU consumption of the **coredns** container

---

### Edge Case: Amorphous Applications

Throughout the maintenance stage, the application structure and functionalities can change, and this is expected! The architecture of an application is not static, it is amorphous and in constant movement. This represents the organic growth of a product that is responsive to customer feedback and new emerging technologies.

![Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/IMG_20210703_224857.jpg](Lesson-2%20Architecture%20Considerations%20133a3d88a02b4518944b9f4eac471e18/IMG_20210703_224857.jpg)

Both monolith and microservice-based applications transition in the maintenance phase after the production release. When considering adding new functionalities or incorporating new tools, it is always beneficial to focus on extensibility rather than flexibility. Generally speaking, it is more efficient to manage multiple services with a well-defined and simple functionality (as in the case of microservices), rather than add more abstraction layers to support new services (as we’ve seen with the monoliths). However, to have a well-structured maintenance phase, it is essential to understand the reasons an architecture is chosen for an application and involved trade-offs.

- A **split** operation - is applied if a service covers too many functionalities and it's complex to manage. Having smaller, manageable units is preferred in this context.
- A **merge** operation- is applied if units are too granular or perform closely interlinked operations, and it provides a development advantage to merge these together. For example, merging 2 separate services for log output and log format in a single service.
- A **replace** operation - is adopted when a more efficient implementation is identified for a service. For example, rewriting a Java service in Go, to optimize the overall execution time.
- A **stale** operation - is performed for services that are no longer providing any business value, and should be archived or deprecated. For example, services that were used to perform a one-off migration process.

---
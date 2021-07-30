# Lesson-1: Introduction to Micro-services

This wiki is built in Notion. Here are all the notes you need to continue.

# The Basics & Pre-requisites

---

Instructor: Katie Gamanji, Ecosystem Advocate  @CNCF

### Syllabus

1. Intro to Microservices.
2. Architecture Considerations.
3. Container Orchestration.
4. Open Source Paas.
5. Cloud Native CI/CD.

### Tools & Pre-requisites

- **web application development with Python (Python Installation⤵)**

[https://www.youtube.com/watch?v=uDbDIhR76H4](https://www.youtube.com/watch?v=uDbDIhR76H4)

- using the **CLI** or command-line interface

[https://www.youtube.com/watch?v=mUXVBMhr7Xg](https://www.youtube.com/watch?v=mUXVBMhr7Xg)

- using **git commands**

[https://www.youtube.com/watch?v=8JJ101D3knE](https://www.youtube.com/watch?v=8JJ101D3knE)

- creating a **DockerHub account**

[https://www.youtube.com/watch?v=3c-iBn73dDE](https://www.youtube.com/watch?v=3c-iBn73dDE)

- Vagrant

[https://www.youtube.com/watch?v=vBreXjkizgo](https://www.youtube.com/watch?v=vBreXjkizgo)

- Virtual Box

[https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

---

## Lesson-1: Introduction to Micro-services

- Introduction to Cloud Native
- CNCF and Cloud Native tooling
- Stakeholders
- Tools, Environment & Dependencies

- **Cloud Native**: It refers to the set of practices that empowers an organization to build & manage applications at scale while using private, public and hybrid cloud providers.

In addition to scale, an organization needs to be agile in integrating customer feedback and adapting to the surrounding technology ecosystem.

- **Containers**: Containers are small & manageable units that have an application running inside. It can be deployed fast & resiliently, fits well with a microservice-based architecture.

Microservices are used to manage and configure a collection of small, independent services that can be easily packaged and executed within a container.

![Lesson-1%20Introduction%20to%20Micro-services%2059febe7e1f5d43c4a52062b4674f93b5/IMG_20210625_230133.jpg](Lesson-1%20Introduction%20to%20Micro-services%2059febe7e1f5d43c4a52062b4674f93b5/IMG_20210625_230133.jpg)

---

**Kubernetes** had its first initial release in 2014 and it derives from Borg, a Google open-source container orchestrator. Currently, Kubernetes is part of **CNCF** or **Cloud Native Computing Foundation**. CNCF was founded in 2015, and it provides a **vendor-neutral home to open-source projects** such as Kubernetes, Prometheus, ETCD, Envoy, and many more.

Overall, Kubernetes is a container orchestrator that is capable to solutionize the integration of the following functionalities:

- Runtime
- Networking
- Storage
- Service Mesh
- Logs and metrics
- Tracing

![Lesson-1%20Introduction%20to%20Micro-services%2059febe7e1f5d43c4a52062b4674f93b5/IMG_20210625_230059.jpg](Lesson-1%20Introduction%20to%20Micro-services%2059febe7e1f5d43c4a52062b4674f93b5/IMG_20210625_230059.jpg)

---

An engineering team can use cloud-native tooling to enable quick delivery of **value to customers** and **easily extend** to new features and technologies. These are the main reasons why an organization needs to adopt cloud-native technologies. However, when trialing cloud-native tooling, there are two main perspectives to address: business and technical stakeholders.

![Lesson-1%20Introduction%20to%20Micro-services%2059febe7e1f5d43c4a52062b4674f93b5/IMG_20210625_225946.jpg](Lesson-1%20Introduction%20to%20Micro-services%2059febe7e1f5d43c4a52062b4674f93b5/IMG_20210625_225946.jpg)

From a **business perspective**, the adoption of cloud-native tooling represents:

- Agility - perform strategic transformations
- Growth - quickly iterate on customer feedback
- Service availability - ensures the product is available to customers 24/7

From a **technical perspective**, the adoption of cloud-native tooling represents:

- Automation - release a service without human intervention
- Orchestration - introduce a container orchestrator to manage thousands of services with minimal effort
- Observability - ability to independently troubleshoot and debug each component

---
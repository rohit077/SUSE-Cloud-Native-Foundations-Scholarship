# Lesson-5: CI/CD with Cloud Native Tooling

This wiki is built in Notion. Here are all the notes you need to continue.

---

Instructor: Katie Gamanji, Ecosystem Advocate  @CNCF

### Outcomes

- Continuous Application Deployment
- The CI Fundamentals
- The CD Fundamentals
- Configuration Manager

# **CI/CD with Cloud Native Tooling**

In this lesson, we will explore how to use cloud-native tooling to construct a CI/CD pipeline. As well, we will explore template configuration managers, such as Helm to enable the easy deployment of an application to multiple clusters.

![https://video.udacity-data.com/topher/2020/December/5fe38442_screen-shot-2020-12-23-at-9.54.00-am/screen-shot-2020-12-23-at-9.54.00-am.png](https://video.udacity-data.com/topher/2020/December/5fe38442_screen-shot-2020-12-23-at-9.54.00-am/screen-shot-2020-12-23-at-9.54.00-am.png)

### Big Picture: Application Releases

In the case of a PaaS offering, the release of a new feature is managed by the provider. For example, Cloud Foundry monitors the repository with the source code, and when a new commit is identified, the user can easily deploy the latest changes with a click of a button. On the other side, releasing an application to a Kubernetes cluster consists of a series of manually typed docker and kubectl commands. At this stage, this approach has no automation integrated.

[]()

A delivery pipeline includes stages that can test, validate, package, and push new features to a production environment. It is common practice for the main branch commits to proceed through all stages of the pipeline to reach the end-users. Overall, a delivery pipeline is triggered when a new commit is available. The new changes should traverse the following stages:

- **Build** - compile the application source code and its dependencies. If this stage fails the developer should address it immediately as there might be missing dependencies or errors in the code.
- **Test** - run a suite of tests, such as unit testing, integration, UI, smoke, or security tests. These tests aim to validate the behavior of the code. If this stage fails, then developers must correct it to prevent dysfunctional code from reaching the end-users.
- **Package** - create an executable that contains the latest code and its dependencies. This is a runnable instance of the application that can be deployed to end-users.
- **Deploy** - push the packaged application to one or more environments, such as sandbox, staging, and production. Usually, the sandbox and staging deployments are automatic, and the production deployment requires engineering validation and triggering.

It is common practice to push an application through multiple environments before it reaches the end-users. Usually, these are categorized as follows:

- Sandbox - development environment, where new changes can be tested with minimal risk.
- Staging - an environment identical to production, and where a release can be simulated without affecting the end-user experience.
- Production - customer-facing environment and any changes in this environment will affect the customer experience.

![https://video.udacity-data.com/topher/2020/December/5fe21b39_screenshot-2020-12-22-at-16.07.41/screenshot-2020-12-22-at-16.07.41.png](https://video.udacity-data.com/topher/2020/December/5fe21b39_screenshot-2020-12-22-at-16.07.41/screenshot-2020-12-22-at-16.07.41.png)

Continuous Deployment pipeline

Overall, a delivery pipeline consists of two phases:

- **Continuous Integration** (or CI) includes the **build**, **test**, and **package** stages.
- **Continuous Delivery** (or CD) handles the **deploy** stage.

### **Advantages of a CI/CD pipeline**

- Frequent releases - automation enables engineers to ship new code as soon as it's available and improves responsiveness to customer feedback.
- Less risk - automation of releases eliminates the need for manual intervention and configuration.
- Developer productivity - a structured release process allows every product to be released independently of other components

## **New Terms**

- **Continuous Integration** - a mechanism that produces the package of an application that can be deployed.
- **Continuous Delivery** - a mechanism to push the packaged application through multiple environments, including production.
- **Continuous Deployment** - a procedure that contains the Continuous Integration and Continuous Delivery of a product.

---

## The CI Fundamentals

- **C**ontinuous Integration: Continuous Integration (CI) is a mechanism that produces the package of an application that can be deployed to consumers. As such, every commit to the main branch is built, tested, and packaged, if it meets the expected behavior. Within the cloud-native, the result of Continuous Integration represents a Docker image or an artifact that is OCI compliant.

[]()

Let's explore the commands and tools associated with each Continuous Integration stage!

![https://video.udacity-data.com/topher/2020/December/5fe22208_screenshot-2020-12-22-at-16.42.36/screenshot-2020-12-22-at-16.42.36.png](https://video.udacity-data.com/topher/2020/December/5fe22208_screenshot-2020-12-22-at-16.42.36/screenshot-2020-12-22-at-16.42.36.png)

Continuous Integration stages

### **Build**

The build stage compiles the application source code and associated dependencies. We have explored this stage as part of the Dockerfile. For example, the Dockerfile for the Python hello-world application instructed the installation of dependencies from *requirements.txt* file and execution of the *app.py* at the container start.

```python
# use pip to install any application dependencies
RUN pip install -r requirements.txt

# execute command  on the container start
CMD [ "python", "app.py" ]
```

### **Test**

The technology landscape provides an abundance of frameworks, libraries, and tools to test and validate the behavior of an application. For Python application, some of the well-known frameworks are:

- [pytest](https://docs.pytest.org/en/stable/) - for functional testing of the application
- [pylint](https://pypi.org/project/pylint/) - for static code analysts of the application codebase

### **Package**

The result of the package stage is an executable that contains the latest features and their dependencies. With Docker, this stage is implemented using `docker build` and `docker push` commands. These create a Docker image using a Dockerfile, and stores the image to a registry, such as DockerHub. For example, to create and store the image for Python hello-world application, the following commands are used:

```docker
# build an image using a Dockerfile
docker build -t python-helloworld .

# store and distribute an image using DockerHub
docker push pixelpotato/python-helloworld:v1.0.0
```

*Note:* Build backs does not require a Docker file to create an OCI compliant image or artifact.

## GitHub Actions

GitHub Actions are event-driven workflows that can be executed when a new commit is available, on external or scheduled events. These can be easily integrated within any repository and provide immediate feedback if a new commit passes the quality check. Additionally, GitHub Actions are supported for multiple programming languages and can offer tailored notifications (e.g. in Slack) and status badges for a project.

A GitHub Action consists of one or more jobs. A job contains a sequence of steps that execute standalone commands, known as actions. When an event occurs, the GitHub Action is triggered and executes the sequence of commands to perform an operation, such as code build or test.

![https://video.udacity-data.com/topher/2020/December/5fe22eb7_screenshot-2020-12-22-at-17.33.17/screenshot-2020-12-22-at-17.33.17.png](https://video.udacity-data.com/topher/2020/December/5fe22eb7_screenshot-2020-12-22-at-17.33.17/screenshot-2020-12-22-at-17.33.17.png)

GitHub Actions structure

Let's configure a GitHub Action that prints the Python version!

GitHub Actions are configured using YAML syntax, hence uses the `.yaml` or `.yml` file extensions. These files are stored in `.github/workflows` directory within the code repository. Within this folder, the `python-version.yml` contains the following sections:

```python
## file location and name: # .github/workflows/python-version.yml

##  Named of the workflow.
name: Python version
## Set the trigger policy.
## In this case, the workflow is execute on a `push` event,
## or when a new commit is pushed to the repository
on: [push]
## List the steps to be executed by the workflow
jobs:
  ## Set the name of the job
  check-python-version:
    ## Configure the operating system the workflow should run on.
    ## In this case, the job on Ubuntu
    runs-on: ubuntu-latest
    ## Define a sequence of steps to be executed
    steps:
      ## Use the public `checkout` action in version v2
      ## to checkout the existing code in the repository
      - uses: actions/checkout@v2
      ## Use the public `setup-python` action  in version v2
      ## to install python on the  Ubuntu based environment
      - uses: actions/setup-python@v2
      ## Executes the `python --version` command
      - run: python --version

```

On the successful execution of the workflow, the Python version is printed. For example, `Python 3.9.0`.

```python
## Define a test that will always pass successfully
deftest_always_passes():
	assertTrue
```

Once we have completed writing the test, we can construct a GitHub Action that will execute the tests using `pytest` tool. For this purpose, a new workflow is defined in the [pytest.yml file](https://github.com/udacity/nd064_course_1/blob/main/.github/workflows/pytest.yml) within the `.github/workflows` directory.

To trigger the workflow a new commit is necessary (e.g. editing the README.md fie). On the successful execution of the GitHub Action, we should see an output mentioning that all the test passed successfully (as expected):

```
Run pytest
============================= test session starts ==============================
platform linux -- Python 3.8.6, pytest-6.2.1, py-1.10.0, pluggy-0.13.1
rootdir: /home/runner/work/nd064_course_1/nd064_course_1
collected 1 item

solutions/python-helloworld/test_with_pytest.py .                        [100%]

============================== 1 passedin 0.02s ===============================

```

Also, this demo showcases the "Python version" GitHub Action that was used as an example in the slides.

## **Further reading**

Explore GitHub Actions in more detail:

- [Introduction to GitHub Actions](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions)
- [Create an example workflow](https://docs.github.com/en/free-pro-team@latest/actions/learn-github-actions/introduction-to-github-actions#create-an-example-workflow)
- Inspect public workflows on [GitHub Marketplace](https://github.com/marketplace?type=actions)

GitHub has a rich library of upstream actions that can be easily integrated with any repository. The suite includes a [Build and push Docker images](https://github.com/marketplace/actions/build-and-push-docker-images) action which uses 3 main components:

- **login** - to logging into DockerHub
- **setup-buildx** - to use an extended Docker CLI build capabilities
- **setup-qemu** - to enable the execution of different multi-architecture containers

The following snippet showcases the Docker build and push of the application, under `.github/workflows/docker-build.yaml` file:

```docker
# This is a basic workflow to help you get started with Actions

name: Docker Build and Push

# Controls when the action will run. Triggers the workflow on push or pull request
# events but only for the master branch
on:
  push:
    branches: [ master ]
  pull_request:
    branches: [ master ]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      -
        name: Checkout
        uses: actions/checkout@v2
      -
        name: Set up QEMU
        uses: docker/setup-qemu-action@v1
      -
        name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v1
      -
        name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKERHUB_USERNAME }}
          password: ${{ secrets.DOCKERHUB_TOKEN }}
      -
        name: Build and push
        uses: docker/build-push-action@v2
        with:
          context: .
          file: ./Dockerfile
          platforms: linux/amd64
          push: true
          tags: {{ YOUR_DOCKERHUB_REPOSITORY }}/python-helloworld:latest

```

The Docker build and push workflow can be found in [course repository](https://github.com/udacity/nd064_course_1/blob/main/solutions/github-actions/docker-build.yml). Make sure to move this file to `.github/workflows/docker-build.yaml` location to execute it.

---

- **Continuous Deployment**: Once an engineering team has automated the packaging of an application, the next phase is to release it to customers. Before the application is pushed to a production environment, it is important to check the functionality of an application when deployed and integrated with platform components. Only when the application meets the excepted behavior, it is deployed to a customer-facing environment. The process of propagating an application through multiple environments, until it reached the end-users, is known as the Continuous Delivery (or CD) stage.

It is common practice to push the code through at least 3 environments: sandbox, staging, and production. A reminder of each environment's purpose :

- Sandbox - development environment, where new changes can be tested with minimal risk.
- Staging - an environment identical to production, and where a release can be simulated without affecting the end-user experience.
- Production - customer-facing environment and any changes in this environment will affect the customer experience.

The sandbox and staging environments are fully automated. As such, if the deployment to sandbox is successful and meets the expected behavior, then the code will be propagated to the staging automatically. However, the push to production requires engineering validation and triggering, as this is the environment that the end-users will interact with. The production deployment can be fully automated, however, doing so implies a high confidence rate that the code will not introduce customer-facing disruptions.

![https://video.udacity-data.com/topher/2020/December/5fe27900_screenshot-2020-12-22-at-22.43.57/screenshot-2020-12-22-at-22.43.57.png](https://video.udacity-data.com/topher/2020/December/5fe27900_screenshot-2020-12-22-at-22.43.57/screenshot-2020-12-22-at-22.43.57.png)

Continuous Delivery stages

Within the deployment pipeline, Continuous Delivery covers the **deploy** stage. Throughout this course, we have practiced the deployment of an application while using `kubectl` commands.

Kubernetes CLI (kubectl), support the management of imperative and declarative configuration. With the imperative approach, a Python hello-world application is deployed by creating a Deployment resource and referencing the desired Docker image:

```docker
# deploy `python-helloworld` using the imperative approach
kubectl create deploy python-helloworld --image=pixelpotato/python-helloworld:v1.0.0

```

On the other side, with the declarative approach, the Python hello-world Deployment is store in a YAML manifest and is created using the `kubectl apply` command:

```docker
# deploy `python-helloworld` using the declarative approach
kubectl apply -f deployment.yaml

```

*Note:* The *declarative* approach is the recommended way to deploy resources within a production environment. Hence, this configuration management technique will be referenced in the next sections.

[]()

## Argo CD

ArgoCD is a declarative Continuous Delivery tool for Kubernetes, which follows the GitOps patterns. As such, ArgoCD operates on configuration stored in manifests (*declarative*) and uses Git repositories as the source of truth for the desired state of an application (*GitOps pattern*). As such, ArgoCD monitors the new commits to Git repositories and applies the latest changes reconciled automatically or on a manual trigger. Additionally, ArgoCD offers deployment to target environments and multiple clusters, and support for multiple config management tools (such as plain YAML, Helm, Kustomize).

For application deployment through multiple environments, ArgoCD provides CRDs (Custom Resource Definitions) to configure and manage the application release.

### **Project resource**

The **Project** resource is a CRD that provides a logical grouping of applications, including access to source and destination repositories, and permissions to resources within the cluster. This resource is handy to segregate and control the deployment to multiple clusters.

### **Application resource**

The **Application** resource that stores the configuration of how an application should be deployed and managed.Let's explore how a Python hello-world application is deployed using an ArgoCD Application resource: *Note:* It is assumed that the declarative manifests for Python hello-world are available ((e.g. deploy.yaml, service.yaml, etc).

```
## API endpoint used to create the Application resource
apiVersion: argoproj.io/v1alpha1
kind: Application
## Set the name of the resource and namespace where it should be deployed.
## In this case the Application resource name is set to  `python-helloworld `
## and it is deployed in the `argocd` namespace
metadata:
  name: python-helloworld
  namespace: argocd
spec:
  ## Set the target cluster and namespace to deploy the Python hello-world application.
  ## For example, the Python hello-world application is deployed in the `default` namespace
  ## within the local cluster or `https://kubernetes.default.svc`
  destination:
    namespace: default
    server: https://kubernetes.default.svc
  ## Set the project the application belongs to.
  ## In this case the `default` project is used.
  project: default
  ## Define the source of the Python hello-world application manifests.
  ## In this example, the manifests are stored in the `argocd-demo` repository
  ## under the `python-manifests` folder. Additionally, the latest commit within
  ## the repository is targeted or `HEAD`.
  source:
    path: python-manifests
    repoURL:
    https://github.com/kgamanji/argocd-demo
    targetRevision: HEAD
  # # Set the sync policy.
  ## If left empty, the sync policy will default to manual.
  syncPolicy: {}

```

### **App of Apps**

![https://video.udacity-data.com/topher/2020/December/5fe31a70_screenshot-2020-12-23-at-10.22.28/screenshot-2020-12-23-at-10.22.28.png](https://video.udacity-data.com/topher/2020/December/5fe31a70_screenshot-2020-12-23-at-10.22.28/screenshot-2020-12-23-at-10.22.28.png)

App of Apps: manage the Application CRDs for multiple microservices with a single Application CRD

ArgoCD provides an “app-of-apps” technique that enables a group of applications to be deployed and configured together. This technique is useful if a product is developed using a microservice-based architecture, and a single point of orchestration is necessary to deploy all microservices. For example, a Web-store Application CRD can be used to manage the Application CRDs for the UI, Login, and Payment units. In this case, one point of control is provisioned to release and manage multiple microservices.

## **New Terms**

- **GitOps** - an operating model that refers to the Git repositories as the source of truth for declarative infrastructure and applications.

## **Further Reading**

Explore the GitOps pattern and ArgoCD resources:

- [A Guide To GitOps](https://www.weave.works/technologies/gitops/)
- [ArgoCD Project CRD](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#projects)
- [ArgoCD Application CRD](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/#applications)

---

## Configuration Managers

A CI/CD pipeline is essential to automate and standardize the application release process. New changes are propagated through multiple environments, including the production cluster, and ensure that the consumers can use the latest features. In an ideal situation, all clusters are similarly configured, such that the engineering team can inspect a realistic simulation of a production deployment. This implies that a set of nearly similar manifests are required for each cluster, sandbox, staging, and production. To reduce the management overhead of overseeing a similar suite configuration for each cluster, templating is necessary.

For example, to deploy the `nginx-alpine` application 4 resource manifests were necessary: Namespace, Deployment, Service, and Configmap. However, this suite of manifests represents the deployment to a single environment, e.g sandbox. The application should be propagated through staging and production environment, which reference a separate set of manifests. It is essential to ensure that the application configuration is tailored for each environment, for example, allocate more CPU and memory to the application in production since it handles more traffic, or have a different number of replicas for each cluster. In this case, an engineering team ends up managing 3 sets of manifests, 1 for each cluster.

![https://video.udacity-data.com/topher/2020/December/5fe33ef0_screenshot-2020-12-23-at-12.58.15/screenshot-2020-12-23-at-12.58.15.png](https://video.udacity-data.com/topher/2020/December/5fe33ef0_screenshot-2020-12-23-at-12.58.15/screenshot-2020-12-23-at-12.58.15.png)

A team has to manage multiple manifests, one for each enviroment, to deploy an application successfully to ptoduction

However, the number of manifests grows exponentially when the application is distributed across multiple regions. As such, if the application is released in AP(Asia Pacific) and the US, a team ends up managing 9 different sets of manifests.

![https://video.udacity-data.com/topher/2020/December/5fe3418e_screenshot-2020-12-23-at-13.08.00/screenshot-2020-12-23-at-13.08.00.png](https://video.udacity-data.com/topher/2020/December/5fe3418e_screenshot-2020-12-23-at-13.08.00/screenshot-2020-12-23-at-13.08.00.png)

Application distribution across multiple regions implies a larger number of manifests that a team should manage

It is clear that it is necessary to introduce a mechanism to store and manage manifests in a reliable, scalable, and flexible way. This capability is offered by configuration management tools, such as:

- **Helm** - package manager that templates exiting manifests, and uses input files to tailor configuration for each environment
- **Kustomize** - a template-free mechanism that uses a base and multiple overlays, to manage the configuration for each environment
- **Jsonnet** - a programming language, that enables the templating of manifests as JSON files, that can be easily consumed by Kubernetes

In the following sections, we will deep-dive into Helm, as the template manager of choice for this course.

## **Helm**

[]()

## **Summary**

**Helm** is a package manager, that manages Kubernetes manifests through charts. A Helm chart is a collection of YAML files that describe the state of multiple Kubernetes resources. These files can be parametrized using Go template.

A Helm chart is composed of the following files:

- **Chart.yaml** - expose chart details, such as description, version, and dependencies
- **templates/** folder - contains templates YAML manifests for Kubernetes resources
- **values.yaml** - default input configuration file for the chart. If no other values file is supplied, the parameters in this file will be used.

![https://video.udacity-data.com/topher/2020/December/5fe3460e_screenshot-2020-12-23-at-13.28.39/screenshot-2020-12-23-at-13.28.39.png](https://video.udacity-data.com/topher/2020/December/5fe3460e_screenshot-2020-12-23-at-13.28.39/screenshot-2020-12-23-at-13.28.39.png)

Helm Chart structure

### **Chart.yaml**

A `Chart.yaml` file encompasses the details of the chart, such as version, description, and maintainer list. For example, a Python hello-world Helm chart contains the following Chart.yaml configuration:

```
## The chart API version
apiVersion: v1
## The name of the chart.
## In this case,  the chart name is`python-helloworld `.
name: python-helloworld
## A single-sentence description of this project
description: Install Python HelloWorld
## A list of keywords about this project to quickly identify the chart's capabilities.
keywords:
- python
- helloworld
## The chart version, here set to `3.7.0`
version: 3.7.0
## List of maintainers, their names, and method of contact
maintainers:
- name: kgamanji
  email: kgamanji@xyz.com

```

### **Templates folder**

The `templates/` folder is a directory containing templated YAML manifests, that require an input file to generate valid Kubernetes resources. For example, the `templates/` folder contains the manifests for Deployment and Namespace resources, used to deploy the Python hello-world application:

```
templates/
├── deployment.yaml
└── namespace.yaml

```

These manifests can be templated using Go template. For example, instead of hardcoding the name of the Namespace, it can be parameterized as following:

```
apiVersion: v1
kind: Namespace
metadata:
  name: {{ .Values.namespace.name }}

```

*Note:* The `.Values` object is used to access the parameters passed from the input `values.yaml` file.

### **values.yaml**

The `values.yaml` file contains default input parameters for a Helm chart. The parameters are consumed by the templated YAML manifests through the `.Values` object. The end result is a suite of valid Kubernetes resources that can be successfully deployed. For example, to provide the configuration for the Deployment and Namespace resources, the `values.yaml` has the following structure:

```
## provide the name of the namespace
namespace:
  name: test

## define the image to execute with the Deployment
image:
  repository:
     pixelpotato/python-helloworld
  tag: v1.0.0

## set the number of replicas for an application
replicaCount: 3

```

It is noteworthy, that any fields within the Kubernetes resources can be parameterized. Additionally, the `values.yaml` file can be overwritten fully or partially, depending if changes are required to all parameters or just to a subset of them. If no override input file is provided, the Helm chart will fallback to using the default `values.yaml` file (included with the chart).

For example, to override the name of the Namespace to `prod` the following `values-prod.yaml` file can be constructed:

```
## override the name of the namespace
namespace:
  name: prod

```

The end result will be a valid Kubernetes Namespace object:

```
apiVersion: v1
kind: Namespace
metadata:
  name: prod
```

---

## Argo CD and Helm

So far, we have explored how an engineering team can use Helm charts to template the manifests for multiple clusters. The next stage consists of integrating the Helm charts in the deploy phase of the CI/CD pipeline.

ArgoCD supports the deployment of manifests that are managed by a Helm chart. To implement this approach, the Application CRD requires to change the source of manifests to a Helm chart. An example of the manifest can be found below:

```
[...]
  source:
    ## change the source of manifests to a Helm chart
    helm:
      ## define the input values file
      valueFiles:
      - values.yaml
    ## set the path to the folder where the Helm chart is stored
    path: solutions/helm/python-helloworld
    ## set the base repository that contains the Helm chart
    repoURL: https://github.com/udacity/nd064_course_1
    targetRevision: HEAD

```

The full YAML representation of the Application CRD that deploys a Helm chart can be found in the [course repository](https://github.com/udacity/nd064_course_1/blob/main/solutions/argocd/argocd-helm-python.yaml).

## **New Terms**

- **Helm** - package manager tool used to template a suite of Kubernetes manifests.
- **Helm chart** - a collection of configuration, input, and templated YAML files used to deploy Kubernetes resources.

## **Further reading**

Explore Helm configuration in more detail:

- [The Chart Template Developer's Guide](https://helm.sh/docs/chart_template_guide/#the-chart-template-developers-guide)
- [The Chart.yaml File](https://helm.sh/docs/topics/charts/#the-chartyaml-file)
- [Templates and Values](https://helm.sh/docs/topics/charts/#templates-and-values)

---

## Edge Case: Push and Pull methodologies for CI/CD

[]()

Within the CI/CD ecosystem, tools have a variate level of capabilities that offer the deployment of an application to production environments. At a high level, these tools are using a push-based or pull-based model to release new features.

Let's explore each CI/CD model in more detail!

### **Push-based CI/CD**

In a **push-based model**, as shown in the image below, the developer commits new code to the Git repository (1), which triggers the Continuous Integration stages (2). The code is packaged and distributed using an image registry, such as DockerHub (3). The Continuous Delivery stage is triggered once the YAML manifests are updated to reference the new image tag (4). A Continuous Delivery tool then pushed the updated manifests to multiple clusters (5).

![https://video.udacity-data.com/topher/2020/December/5fe368ef_screenshot-2020-12-23-at-15.55.14/screenshot-2020-12-23-at-15.55.14.png](https://video.udacity-data.com/topher/2020/December/5fe368ef_screenshot-2020-12-23-at-15.55.14/screenshot-2020-12-23-at-15.55.14.png)

Push-based CI/CD workflow

This model is fully operational and many tools within the ecosystem offer this deployment approach, e.g. Jenkins and Circle CI. However, there is one downside to this model: changes should be actively propagated to all environments. If this is not fulfilled, a scenario might be reached where multiple changes need to be deployed to production, which increases the failure rate and complicated the recovery procedures. Hence, wide awareness is required of features pushed to a production environment.
**Pull-based CI/CD**
In a **pull-based** CI/CD approach, the release process is still be triggered by a developer that pushed new features to the source code (1). The package (2) of the application is similar, resulting in a new image stored in DockerHub (3). However, once the YAML manifests are updated with the new image tag, a pull-based Continuous Delivery tool identifies new changes (4) and applies them to a Kubernetes cluster (5). As a result, this simplifies the process of application release, as new features can be applied automatically as soon as they are available. Tools that offer this CI/CD model are ArgoCD and Flux.
Pull-based CI/CD workflow:

![https://video.udacity-data.com/topher/2020/December/5fe36bfa_screenshot-2020-12-23-at-16.10.03/screenshot-2020-12-23-at-16.10.03.png](https://video.udacity-data.com/topher/2020/December/5fe36bfa_screenshot-2020-12-23-at-16.10.03/screenshot-2020-12-23-at-16.10.03.png)

It is paramount to build a deployment pipeline that fits the business requirements closely and automates the release process. There is no "golden path" that would cover all engineering requirements. However, the pull and push-based CI/CD tools contribute to the ultimate goal to ships code securely, automatically, and reliably.

---

Once a team has developed a product, it is paramount to automate the release process. The building, testing, packaging, and deploying an application to multiple clusters should be highly automated. Building a structured and well-defined CI/CD process is the key to a successful product release. In this lesson, we have explored GitHub Actions to implement the Continuous Integration stages and ArgoCD to perform the Continuous Delivery stages. Additionally, we have covered Helm, as a template configuration manager, that allows the parameterization of YAML manifests and their easy deployment across multiple Kubernetes clusters.

Overall, in this lesson we have explored:

- Continuous Application Deployment
- The CI Fundamentals
- The CD Fundamentals
- Configuration Managers

## **New Terms**

- **Continuous Integration** - a mechanism that produces the package of an application that can be deployed.
- **Continuous Delivery** - a mechanism to push the packaged application through multiple environments, including production.
- **Continuous Deployment** - a procedure that contains the Continuous Integration and Continuous Delivery of a product.
- **GitOps** - an operating model that refers to the Git repositories as the source of truth for declarative infrastructure and applications.
- **Helm** - package manager tool used to template a suite of Kubernetes manifests.
- **Helm chart** - a collection of configuration, input, and templated YAML files used to deploy Kubernetes resources.
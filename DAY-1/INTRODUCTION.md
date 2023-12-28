**What is Kubernetes (k8s)?**

Kubernetes is an open-source Container Management tool that automates container deployment, container scaling, descaling, and container load balancing (also called a container orchestration tool). It is written in Golang and has a vast community. It was first developed by Google and later donated to CNCF (Cloud Native Computing Foundation).
Kubernetes can group ‘n’ number of containers into one logical unit for managing and deploying them easily.
2. It schedules, runs, manages isolated containers which are running on physical/virtual/cloud machines.
3. All top clod providers support K8S.

**What is monolithic architecture?**

We can say that all different business services are packaged together as a single unit which is tightly coupled so typically when you look at the monolithic architecture it primarily consists of three tiers.

**High-Level Monolithic Architecture:**
1. _Presentation layer:_
 At a high level the presentation tier is the front-end layer of the three-tier architecture. It is primarily a user interface of a web application. This tier is often built with web technologies such as HTML, Javascript, CSS, or through other popular web development frameworks and this tier communicates with the other two tiers at the bottom using API calls.
2. _Application tier:_
 This is where you have all the business logic. It is often written in Java, Dotnet, C, Python, C++, or some more related languages.
3. _Data-tier:_
 This is where all the data is stored and this data is accessed by the application layer when API calls. This tier consists of database technologies such as MySQL, Oracle DB, Microsoft SQL Server, MongoDB etc.

So that’s a high-level overview of the monolithic architecture and its tiers. So all these layers are packaged and deployed together as one single unit and that is one of the main reasons why it is called monolithic architecture.

**History:**
1. Google developed an internal system called borg later name as OMEGA to deploy & manage thousands google applications & services on their cluster.
2. Google introduce K8S in 2014.

**Online Platform for K8S**

1. Kubernetes playground
2. Play with K8S etc.

**Cloud sopport K8s:**
1. AWS(EKS)
2. GCP(GKE)
3. Azure(AKS)

K8S Installation tool:
1. Minicube
2. Kubeadm

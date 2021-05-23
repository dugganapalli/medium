Kubernetes or K8s is a famous open source “container orchestration” platform. It is designed to automate Deploying, Scaling, and Operating Application Containers. First we need to understand one fundamental thing, why do we need container orchestrators?

We know singularity and docker, have taken off over the past few years, with more and more people at the enterprise level moving to containerization for different applications. The questions that arise are how do we manage all these containers running n a single host and across the entire enterprise infrastructure. Container orchestrators to the rescue! They solve the problem of deploying multiple containers wither by themselves or as a part of an application across many hosts.
Some key features we need from Container orchestrators are:
ability to provision hosts
start containers in a host
restart failing containers
ability to link containers together so that they can communicate with their peers
expose required containers as services to the world outside the cluster
scaling the cluster up or down
Now let’s jump in and look at the K8 architecture from a high level. Everything will be explained in context to the figure shown below

MASTER NODE:
This consists of 3 main parts:
Kube API server
Scheduler
Controller Manager
We will look at what each does here:
Kube API server: This is the frontend that allows you to interact with Kubernetes API, as the name suggests. It takes care of communications.
Scheduler: Watches created Pods, who do not have a Node design yet, and designs the Pod to run on a specific Node.
Controller Manager: Runs controllers! These are background threads that run tasks in a cluster. The controller actually has a bunch of different roles, but that’s all compiled into a single binary. The roles include:
i. Node Controller: responsible for the worker states
ii. Replication Controller: responsible for maintaining the correct number of Pods for the replicated controllers
iii. End-Point Controller: joins services and Pods together.
iv. Service account and token controllers: handle access management.
etcd :
Kubernetes uses etcd as its database and stores all cluster data here. It si a simple distributed key value (KV) store. Some of the information that might be stored, is job scheduling info, Pod details, stage information, etc.
Link: https://github.com/etcd-io/etcd/blob/master/Documentation/docs.md
kubectl application:
It is a command line interface for running commands against Kubernetes clusters. Responsible for Interaction with Master Node.
This has a config file called kubeconfig. This file has server information, as well as authentication information to access the API Server.
Here is a link for further review: https://kubernetes.io/docs/reference/kubectl/overview/
WORKER NODE:
These are the heart and soul of the K8 platform. These are the nodes where your platform operates. The Worker Nodes communicate back with the Master Node. Communication to a Worker Node is handled by the Kubelet Process.
Kubelet Process:
It’s an agent that communicates with the API Server to see if Pods have been designed to the Nodes. It executes Pod containers via the container engine. It mounts and runs Pod volume and secrets & is aware of Pod of Node states and responds back to the Master.

If the kubelet process is not working correctly on the worker node
Container native platform:
Ex. Docker running on worker nodes. Docker works together with Kubelet to run containers on the Node.
Kube-proxy:
Network proxy & Load balancer for the service, on a single Worker Node. It handles the network routing for TCP and UDP Packets, and performs connection forwarding.
Pods:
Having the Docker Daemon allows you to run containers. Containers of an application are tightly coupled together in a Pod.
A Pod is the smallest unit that can be scheduled as a deployment in Kubernetes.
This group of containers share storage, Linux name space, IP addresses, amongst other things. They’re also co- located and share resources that are always scheduled together.
Now that the pods have been deployed and are running, the kubelet process communicates with the Pods to check on state and health, and the Kube-proxy routes any packets to the Pods from other resources that might be wanting to communicate with them.
Worker Nodes can be exposed to the internet via load balancer. And, traffic coming into the Nodes is also handled by the Kube-proxy, which is how an End-user ends up talking to a Kubernetes application.

# Kubernetes
Kubernetes Repo For learnings and Hands on using minikube

# I will be discussing the below topics
a) What is kubernetes
b) Highlevel architecure of K8S
c) Deploying applications in k8S
d) Monitoring k8S cluster

## What is Kubernetes

Kubernetes is a container orchestration platform that users can automate the deployment of the applications, scale, manage, upgrade in a secured way.
In general terms according to me we can think Kubernetes is like a "Railway station" which contains multiple platforms called as "nodes" in Kubernetes and the scheduling of applications on the nodes can be treated as schedling different trains to different platforms (PF1, PF2 etc) based on certain criteria.

## Kubernetes Architectire

What is a cluster : 

A cluster is a group of nodes(machines) interconnected that can host multiple applications to serve a specific purpose.

A Kubernetes cluster contains multiple machines (VM's or baremetals) called nodes. Each node can be installed with multiple applications called conatiners.
All these applications can talk each other to serve a specific purpose that provides a business solution.

What is a container :

A container is a 

### Kubernetes components :

Kuberntes cluster consists of nodes which are master and minion. Can have multiple masters and minions or nodes will be multiple and the configuration of having how many masters and how many minions depends on the use case and the exact requirement.

### Master node:

The master node will maage, plan, Monitor and schedule the nodes i.e Minions.

How a user know which node is master or minion ..
A Master node will have the components kube-apiserver, Controll-Manager, ETCD-Cluster, Kube-Scheduler.
Using these components different tasks are handled to service specific purpose. The detailed discussion on what each component does will be done going forward.

### worker node:

A minion will host applications as containers.
A minion node consists of the components Kubelet, kube-proxy, container runtime (Docker etc).

So based on the components that are installed on the machines a user can decide if the node is a master or minion.


<img width="259" alt="image" src="https://user-images.githubusercontent.com/31388628/169792764-99cf29b0-7594-4abe-a614-b76540bd2ec9.png">


<img width="390" alt="image" src="https://user-images.githubusercontent.com/31388628/169792518-5ce584b3-670b-48b4-9eac-ac76b09ba890.png">
  

<img width="259" alt="image" src="https://user-images.githubusercontent.com/31388628/169790506-d07ee01d-c48b-4801-95a5-1bece1e911e3.png">


<img width="478" alt="image" src="https://user-images.githubusercontent.com/31388628/169794381-a473655a-e69e-44b5-8e5f-7bb32c21869f.png">


                   
  ### Comparison Of a Kubernetes Cluster to a Railway Station :
  
 #### Kubernetes Master node ---> Governing body of Railway station which consists of multiple people that handles different tasks.
  
  ETCD-Cluster ---> This is the system that has all information about the state of the Railway station, how many platforms exists, state of each platform, condition, Trains that pass the station daily etc.
  
  Controller-Manager ---> The system or the person that will check on the status of the platforms if free or occupied, health check of the platform, other criteria to meet if a train can be scheduled or not.
  
  Scheduler ---> The system or the person to decide which train to be scheduled on which platform based on certain criteria like platform availability, platform length vs train length, condition etc.
  
  Kube-API server ---> This is the display board which is installed infront of the station where passengers will interact with to know which train is scheduled on which platform and at what time. The display board will act as an API that will be interacted with the passengers and the display board will be interacted with other components of the governing body where the display will fetch the data from other components and communicate to the passengers.
  
#### Kubernetes Worker nodes ---> These are the different platforms where the trains are scheduled and serves the passengers. Each platform has a name or number called PF1, PF2 which are similar to node names.
 
 Kubelet ---> This is the signalling system or the signals that is available for each platform which interacts with the kube-api server and schedule the train on the platform. 
 
 Kube-Proxy ---> 
 
 Container Run Time Engine ---> The train is the container and the driver is the run time enginer which will ensure the containers are running as expected.


## Deep dive on Kubernetes components

### ETCD :

What is ETCD ?

ETCD is a simple distributed key-value store that faster and secure.

Why we need ETCD ?

As said Kubernetes cluster consists of multiple nodes (masters and minions), multiple applications running as containers and other different components. There should be some way to monitor the state of the cluster and store somewhere such that the same state can be retrieved if an issue occurs.

What ETCD stores ?

etcdctl is a command line tool that will help to store the needed information in the etcd cluster using key-value schema.
ETCD cluster is stored with the kubernetes cluster information i.e configuration data, its state and its metadata.

### KUBE-API Server :

What is KUBE-API Server ?

It acts as a API server to the user. What does this mean ? When a user tries to create a Kubernetes object the API server is first notified and then it validates the request, perform the needed task and send back the needed information to the user.

Why we need Kube-API server ?

There should be a central mechanism that should validate the user request and ensure that the request is coming from a valid user, as well it is a valid request and then asks the respective cmponents to perform the needed task. It is also important that the changed state has to be updated with the ETCD cluster which KUBE-API server will take care.

What KUBE-API server will do ?

Example: A user tries to create a pod using a spec of a command i.e kubectl run the following sequence happens.

1) User issues the request
2) Kube-API server autenticate's the user
3) Validate Requests
4) Retrieve the data
5) Update ETCD
6) Scheduler will decide on which node the pod to be created
7) Kubelet on the specific node will create the pod

### Kube-Controller-Manager :

What is Kube-Controller-Manager ?

Controllers are kind of daemons that will watch the state of the cluster and then make the changes to the cluster if something is not as expected.
This is an automatic way of handling the issues to be fixed or also known as self healing mechanism.

Controllers talk to kube-API server and gets the information of the node through kubelet.

Why we need Controller ?

With a cluster having 10s or 100s of nodes and 100s or 1000s of applications running there should be some automatic mechanism to see that all nodes are healthy always for the applications to have HA. 

What it actually do ?

There are different types of controllers.
 1) Node Controller
 2) Replication Controller

Node Controller :

The Node-controller will do this task to see that all nodes are healthy and if any node is not healthy it will mark it as "Not Ready" and the pods(smallest object of Kubernetes where the application is run) on that nodes will be scheduled to a different node if they part part of a replicaset.

Replication Controller :

It is responsible for monitoring the status of replicasets and ensuring that desired number of pods are available all the times within the set. If a pod dies then a new pod is launched to makre sure the number of replicas defined are available.

Note : There are couple of concepts like pod, replicaset etc were discussed which will be expalined in detailed going forward. For now think of them as kubernetes objects.


### Kube-Scheduler :

What is a kube-scheduler ?

It is the control plane component that will schedule the pod on the kubernetes nodes based on certain criteria like if a label is assigned, available CPU/memory etc

Why we need a scheduler ?

When a user creates a kubernetes object that inturn creates a pod. A pod is a low level component that has the application code and should run on a worker node for providing the needed functionality to the user. 

What the scheduler will do ?

Example : A user tries to create a nginx pod which acts as webpage that to be disaplyed to millions of users when opened in a browser. Once the command is executed a pod is generated but where should that pod be scheduled and run with multiple nodes available ? This is where the scheulder comes into picture and decide based on different criteria like labels, available cpu/memory etc and schedule the pod on a specific node and serves the needed functionality.






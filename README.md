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



                   
  
 







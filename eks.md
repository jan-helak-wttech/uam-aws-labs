# AWS Elastic Kubernetes Service

# Table of contents

- [AWS Elastic Kubernetes Service](#aws-elastic-kubernetes-service)
- [Table of contents](#table-of-contents)
  - [TLDR](#tldr)
  - [Introduction](#introduction)
  - [Use Cases for kubernetes:](#use-cases-for-kubernetes)
  - [Cluster Architecture](#cluster-architecture)
  - [Kubernetes objects](#kubernetes-objects)
  - [kubectl - Kubernetes CLI](#kubectl---kubernetes-cli)
  - [Working with kubernetes](#working-with-kubernetes)
  - [local development](#local-development)
  - [AWS Elastic Kubernetes Service](#aws-elastic-kubernetes-service-1)
  - [Bonus lectures](#bonus-lectures)
  - [Labs](#labs)
  - [Tasks](#tasks)


## TLDR
![Kubernetes logo](https://upload.wikimedia.org/wikipedia/commons/thumb/3/39/Kubernetes_logo_without_workmark.svg/200px-Kubernetes_logo_without_workmark.svg.png)

> *Kubernetes* is a *container orchestration platform*. It is working as autonomous system for running microservices in a cluster of machines.
> *EKS* is a managed Kubernetes service provided by AWS. It allows to run Kubernetes cluster in AWS cloud.

## Introduction
- *Kubernetes* means *helmsman* or *pilot* in Greek;
- Official website of Kubernetes: https://kubernetes.io/;
- Platform introduced by Google in 2014, available as open source on Apache License 2.0;
- Supported by leading cloud providers: AWS, Azure, Google Cloud, IBM Cloud, Oracle Cloud, DigitalOcean, Linode, Alibaba Cloud, etc;

## Use Cases for kubernetes:
- *Microservices*
- *High availability*
- *Scalability*
- *Fault tolerance*
- *Cloud agnostic*
- *Hybrid cloud*
- *Open source*
- *Case studies: https://kubernetes.io/case-studies/;*

## Cluster Architecture

![Kubernetes Architecture](https://kubernetes.io/images/docs/components-of-kubernetes.svg)

- Kubernetes cluster consists of:
  - *Master node(s)* - responsible for managing the cluster;
  - *Worker node(s)* - responsible for running containers;
- Master node consists of:
  - *API server* - provides API for managing the cluster;
  - *Cloud controller manager* - responsible for managing cloud resources;
  - *Scheduler* - responsible for scheduling containers on worker nodes;
  - *Controller manager* - responsible for managing the cluster;
  - *etcd* - key-value store for storing cluster state;
 
- Worker node consists of:
  - *Kubelet* - agent responsible for running containers on the node;
  - *Kube-proxy* - network proxy for forwarding traffic to containers;
  - *Container runtime* - software for running containers (Docker, containerd, CRI-O, etc);

## Kubernetes objects
- *namespace* - logical set of Kubernetes objects;
- *pod* - logical set of containers with required resources;
- *volume* - logical storage for containers;
- *secret* - sensitive data for containers;
- *config map* - configuration data for containers;
- *replica set* - logical set of pods with defined number of replicas;
- *deployment* - logical set of pods with common access point and defined number of replicas. Deployments have following strategies:
  - *Recreate* - old pods are killed and new pods are created;
  - *Rolling update* - old pods are killed and new pods are created one by one;
- *job* - logical set of pods with defined number of replicas. Jobs are used for running batch jobs;
- *cron job* - logical set of pods with defined number of replicas. Cron jobs are used for running batch jobs periodically;
- *deamon set* - logical set of pods with defined static number of replicas;
- *service* - logical set of pods with common access point. Types of services:
  - *ClusterIP* - service is available only inside the cluster;
  - *NodePort* - service is available on a port on each node;
  - *LoadBalancer* - service is available on a port on each node and load balancer is created in cloud provider
  - *ExternalName* - service is available on a domain name;
- *stateful set* - logical set of pods with defined number of replicas. Stateful sets are used for running stateful applications;
- *labels* - key-value pairs for identifying Kubernetes objects;

## kubectl - Kubernetes CLI
- kubectl is a command line tool for managing Kubernetes clusters;
- cheatsheet: https://kubernetes.io/docs/reference/kubectl/cheatsheet/;

## Working with kubernetes
- cluster configuration can be stored in a collection of YAML files;
- updates of cluster configuration can be applied by kubectl and proper YAML file;

## local development
- *minikube* - local Kubernetes cluster for development and testing;
- landing page: https://minikube.sigs.k8s.io/;

## AWS Elastic Kubernetes Service
- AWS EKS is a managed Kubernetes service provided by AWS;
- landing page: https://aws.amazon.com/eks/;
- it's a regional service
- master nodes are managed by AWS;
- worker nodes are managed by AWS or by user; Types of worker nodes:
  - *EC2* - worker nodes are EC2 instances;
  - *Fargate* - worker nodes are serverless containers;
- starting with EKS: https://docs.aws.amazon.com/eks/latest/userguide/getting-started-console.html#eks-configure-kubectl
- *eksctl* - command line tool for creating and managing EKS clusters;
  
## Bonus lectures
- SRE - Site Reliability Engineering - [read online](https://sre.google/sre-book/table-of-contents/)
- eksctl - command line tool for creating and managing EKS clusters in AWS - [read online](https://eksctl.io/)
- Removing docker shim - history of Kubernetes and Docker - [read online](https://kubernetes.io/blog/2022/01/07/kubernetes-is-moving-on-from-dockershim/)


## Labs
1. Connect to created EKS cluster using `aws eks` command.

2. Check if kubectl is working:
```
kubectl get svc
```

3. Inspact kubectl command options:
```
kubectl
kubectl api-resources
kubectl explain
```

4. Check all namespaces:
```
kubectl get namespaces
```
5. Show all the objects from system namespace:
```
kubectl get all -n kube-system
kubectl get all -n kube-node-lease
kubectl get all -n kube-public
```

6. Create new namespace:
```
kubectl create namespace uam-lab
```

7. Set new namespace as default:
```
kubectl config set-context --current --namespace=uam-lab
```

8. Check existing nodes:
```
kubectl get nodes
```

9. Create node group using AWS console.
10. Check existing nodes:
```
kubectl get nodes
```
11. Deploy simple pod using manual approach:
```
kubectl run nginx --image=nginx
```

12. Check existing pods:
```
kubectl get pods
```

13. Log into existing pod to see it if it's working:
```
kubectl exec -it nginx -- /bin/bash
```

14. Expose pod using a load balancer:
```
kubectl expose pod nginx --type=LoadBalancer --port=80
```

15. Check created objects:
```
kubectl get all
```

16. Remove created LoadBalancer service:
```
kubectl delete svc nginx
```

17. Remove created pod:
```
kubectl delete pod nginx
```

18.  Deploy nginx service using yaml file:
```
curl -LO https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/application/nginx-app.yaml
kubectl apply -f nginx-app.yaml
kubectl get pod -o wide
```

19. Check the results
20. Modify yaml file and increase nginx version by 1:
21. Apply changes:
```
kubectl apply -f nginx-app.yaml
```
22. Check logs:
```
kubectl logs -f <deployment-name>
kubectl describe pod <pod-name>
```
23. Check rollout history:
```
kubectl rollout history deployment <deployment-name>
```
24. Rollback to previous version:
```
kubectl rollout history deployment my-nginx 
kubectl rollout history deployment my-nginx --revision=1
kubectl rollout undo deployment my-nginx --to-revision=1
```


## Tasks
1. Check AWS SLA agreement for EKS service:
   - https://aws.amazon.com/eks/sla/
2. Check AWS EKS pricing:
   - https://aws.amazon.com/eks/pricing/
3. Check AWS Fargate pricing:
    - https://aws.amazon.com/fargate/pricing/
4. Use aws cli to create EKS cluster in AWS:
   - cluster should be created in us-east-1 region;
   - cluster should have 3 worker nodes (t3.small)
   - worker nodes should be managed by AWS (managed group);
   - worker nodes should not be accessible from the Internet;
5. Create definition of service in yaml file:
   - with type of LoadBalancer;
   - service should be available publicly;
  
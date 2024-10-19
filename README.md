Introduction

Install the minikube to set up kubernetes cluster; follow the link https://kubernetes.io/docs/setup/minikube/#installation

Hello-deployment.yml

This YAML file defines a Kubernetes Deployment resource, which is used to manage a set of identical Pods. 
The hello-deployment ensures that the desired number of replicas of a Pod are running and provides declarative updates to Pods and ReplicaSets.
Pod Template: The template section defines the structure of the Pod, and because it's part of the Deployment, 
Kubernetes will ensure that the desired number of Pods (1 in this case) are running and automatically recreate Pods if they are deleted or fail.


Container Port: Port 80 is specified, which means the container will listen for traffic on port 80, typically used for HTTP traffic.
This YAML defines a Kubernetes Deployment named hello-test-deployment that runs a single replica of a Pod containing a container
with the hello-world:nanoserver-ltsc2022 image. The container listens on port 80. Kubernetes will ensure that 1 replica of this Pod is always running, 
and the Pods will have the label app: hello-app-test, which is used to select and identify the Pods


 Ingress Rule:-

This YAML file configures an Ingress resource in Kubernetes that uses AWS’s Application Load Balancer (ALB). 
It specifies health check configurations, defines which AWS ALB will be used, and sets an Ingress class. Traffic that doesn't match other rules will be forwarded to the app3-nginx-nodeport-service.
The annotations control the AWS ALB’s behavior, such as how it checks the health of services and whether it’s internet-facing.

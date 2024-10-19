Kubernetes Deployment, ClusterIP Service, and Ingress Configuration
This document provides a detailed explanation of a Kubernetes Deployment, ClusterIP Service, and Ingress configuration. These resources work together to deploy a simple application and expose it both internally and externally, depending on the configuration.

Requirements
Before you deploy these resources, ensure the following prerequisites are met:

Kubernetes Cluster: A functional Kubernetes cluster (e.g., Minikube, GKE, AKS, EKS).
kubectl: The Kubernetes command-line tool should be installed and configured to communicate with your cluster.
AWS Load Balancer Controller: If using the Ingress resource with an AWS Load Balancer, ensure the AWS Load Balancer Controller is installed.

Explanation
1. Deployment:
A Deployment manages the creation and scaling of Pods in Kubernetes. It ensures the desired number of Pods are running and can update or roll back changes automatically.

apiVersion:

apps/v1 for Deployment resources.
kind: Deployment to manage application Pods.
metadata: Defines the name and labels for identifying the deployment.
spec:
replicas:

Defines how many instances of the Pod should run.

selector: 

Ensures the deployment manages Pods matching the label app: hello-app-test.
template: Describes the Pod that will be created.
containers:
name: hello-app-test, the container name.

image: hello-world:nanoserver-ltsc2022, a simple "Hello World" image from Docker Hub.

ports: Exposes port 80 to receive traffic.

2. ClusterIP Service:
A Service in Kubernetes provides network access to Pods, grouping them under a stable endpoint. ClusterIP is the default service type, which is only accessible from within the cluster.

apiVersion: v1 for core Kubernetes resources.
kind: Service to expose Pods over the network.
metadata: Defines the name and labels to associate with the deployment.
spec:
type: ClusterIP to expose the service internally.
selector: Selects Pods labeled app: hello-app-test.
ports:
port: The port exposed by the service.
targetPort: The port on the Pod that traffic is forwarded to (also 80 in this case).
3. Ingress:
An Ingress resource manages external access to services in the cluster, typically HTTP and HTTPS. In this configuration, it is set up to work with an AWS Application Load Balancer (ALB).

apiVersion: networking.k8s.io/v1 for Ingress resources.
kind: Ingress to manage HTTP/HTTPS traffic.
metadata: Defines the name, labels, and annotations for configuring the AWS ALB.
annotations:
alb.ingress.kubernetes.io/load-balancer-name: Specifies the load balancer name.
alb.ingress.kubernetes.io/scheme: Configures the ALB to be internet-facing.
alb.ingress.kubernetes.io/healthcheck-*: Health check settings for the load balancer.
spec:
ingressClassName: Specifies the AWS ingress class.
defaultBackend: Defines the default backend service (app3-nginx-nodeport-service) to route traffic to.
Workflow
Deployment:

The Deployment ensures that one replica of the hello-world container is running.
The Pod is labeled with app: hello-app-test, allowing it to be targeted by a Service.
ClusterIP Service:

The ClusterIP Service exposes the Pod internally on port 80.
It routes traffic to the Pod, specifically to its port 80.
Ingress:

Ingress resource, integrated with AWS Load Balancer, allows external traffic to be routed to the Service.
Load balancer performs health checks, and routes HTTP requests from the internet to the app3-nginx-nodeport-service, which forwards the traffic to the correct Pod.



Here’s a visual representation of the workflow:

          +----------------------------+
          |   External Client (Internet)|  
          +-------------+--------------+
                        |
                        | HTTP/HTTPS Traffic
                        |
              +---------v---------+
              |     AWS ALB       |   <- Ingress resource configures this ALB
              +---------+---------+
                        |
                        | Routes traffic to service
                        |
              +---------v---------+
              | ClusterIP Service |   <- Internal service that forwards traffic to Pod
              +---------+---------+
                        |
                        | Selects Pod via label
                        |
              +---------v---------+
              |    Pod (Hello App)|
              +-------------------+
                        |
                        | Application Running
                        |
              +-------------------+
              |     Container     |   <- Runs hello-world:nanoserver-ltsc2022 image
              +-------------------+


Deployment Steps
Apply the Deployment:
kubectl apply -f deployment.yaml
Creates the Pod running the hello-world container.

Apply the Service:
kubectl apply -f service.yaml
Exposes the Pod internally using the ClusterIP service.

Apply the Ingress:
kubectl apply -f ingress.yaml
This configures the AWS ALB to route external traffic to your Service.

Verify:

Check if the Pod is running:

kubectl get pods

Verify the service:

kubectl get service hello-test-cluster-ip-service

Check if the Ingress is configured correctly:

kubectl get ingress ingress-basics

Summary
Deployment ensures the hello-world application runs continuously and handles updates or scaling.
ClusterIP Service provides stable internal access to the application.

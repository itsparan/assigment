Requirements:-
Before you deploy these resources, ensure the following prerequisites are met:
1.	Kubernetes Cluster: A functional Kubernetes cluster (e.g., Minikube, GKE, AKS, EKS).
2.	kubectl: The Kubernetes command-line tool should be installed and configured to communicate with your cluster.
3.	AWS Load Balancer Controller: If using the Ingress resource with an AWS Load Balancer, ensure the AWS Load Balancer Controller is installed.
4.	Docker Hub Access: Ensure your cluster can access Docker Hub to pull the hello-world:nanoserver-ltsc2022 image.

YAML Manifest


1. Deployment:

A Deployment manages the creation and scaling of Pods in Kubernetes. It ensures the desired number of Pods are running and can update or roll back changes automatically.

apiVersion: apps/v1 for Deployment resources.
kind: Deployment to manage application Pods.
metadata: Defines the name and labels for identifying the deployment.
spec:
replicas: Defines how many instances of the Pod should run.
selector: Ensures the deployment manages Pods matching the label app: hello-app-test.
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


Workflow
Deployment:

The Deployment ensures that one replica of the hello-world container is running.
The Pod is labeled with app: hello-app-test, allowing it to be targeted by a Service.
ClusterIP Service:

The ClusterIP Service exposes the Pod internally on port 80.
It routes traffic to the Pod, specifically to its port 80.
Ingress:

The Ingress resource, integrated with AWS Load Balancer, allows external traffic to be routed to the Service.
The load balancer performs health checks, and routes HTTP requests from the internet to the app3-nginx-nodeport-service, which forwards the traffic to the correct Pod.



Deployment Steps
Apply the Deployment:

kubectl apply -f deployment.yaml
This creates the Pod running the hello-world container.

Apply the Service:

kubectl apply -f service.yaml

This exposes the Pod internally using the ClusterIP service.

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



Workflow Diagram :-

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


Work Flow and Details:

Kubernetes Deployment, ClusterIP Service, and Ingress Configuration

This document provides a detailed explanation of a Kubernetes Deployment, ClusterIP Service, and Ingress configuration. These resources work together to deploy a simple application and expose it both internally and externally, depending on the configuration.

Requirements



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

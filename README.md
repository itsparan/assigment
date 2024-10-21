# Kubernetes Hello-World Deployment

This repository contains a simple example of deploying a Hello-World application in Kubernetes using `minikube` and `kubectl`. The deployment includes a service and ingress configuration for external access to the application.

## Prerequisites

Ensure that you have the following tools installed:

- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) (for interacting with Kubernetes)
- A Kubernetes cluster running in AWS (Amazon Web Services). You can create this using [EKS](https://docs.aws.amazon.com/eks/latest/userguide/getting-started.html) or
- [Kops](https://kops.sigs.k8s.io/getting_started/aws/).
- [Git](https://git-scm.com/)

## Setup

1. Clone the repository:

    ```bash
    git clone https://github.com/itsparan/assignment.git
    cd assignment
    ```

2. Navigate to the `hello-test-assignment` directory:

    ```bash
    cd assigment/hello-test-assigment/
    ```

3. Apply the Kubernetes configuration:

    ```bash
    kubectl apply -f hello-deployment.yml
    kubectl apply -f hello-ingress.yml
    ```

4. Verify that the deployment and ingress have been created:

    ```bash
    kubectl get ingress
    kubectl get service
    ```

## Access the Application

1. Get the Minikube IP:

    ```bash
    minikube ip
    ```

2. Access the application using `curl`:

    ```bash
    curl http://<minikube-ip>:<node-port>
    ```

    For example:

    ```bash
    curl http://192.168.49.2:30619
    ```

3. Alternatively, use the ingress URL (configured as `hello-world.example`):

    ```bash
    curl --resolve "hello-world.example:80:<minikube-ip>" -i http://hello-world.example
    ```

    Example:

    ```bash
    curl --resolve "hello-world.example:80:192.168.49.2" -i http://hello-world.example
    ```

4. You should see the following response:

    ```plaintext
    Hello, world!
    Version: 1.0.0
    Hostname: web-<pod-identifier>
    ```

## Cleanup

To delete the resources created during the setup, use the following command:

```bash
kubectl delete -f hello-deployment.yml
kubectl delete -f hello-ingress.yml

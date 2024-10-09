# Kubernetes Advance Assignment

This is the kubernetes advance assignment in which we have implemented helm chart and created different namespaces for different uses like development, staging etc. This repository contains a Helm chart for deploying a Spring Boot REST API to a Kubernetes cluster. The application is packaged as a Docker image and deployed using Kubernetes and Helm.
### github url : https://github.com/anandkwr/kubernetesAdvance_assignment.git
## •	Create Helm charts for a sample application (e.g., a web server) with configurable values.
### Step 1: Create a helm chart
1. Create an application folder and navigate to the application folder and run below command to create a helm chart named kubernetes-helm-chart:
   ```bash
   helm create kubernetes-helm-chart
   ```
Expected output:
  ```bash
  anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ helm create kubernetes-helm-chart
  Creating kubernetes-helm-chart
  anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ ls
  kubernetes-helm-chart
  ```
### Step 2: Edit the values.yaml file.
Go to kubernetes-helm-chart and edit the values.yaml file to include configurable values for your Spring Boot application. Set the Docker image reference, container port and replica set.
### Step 3: Edit the deployment.yaml file inside templates folder and update it to use the Spring Boot Docker image and save the file after making changes.
### Step 4: Helm chart installation
Run the below command to install the helm chart and create the deployment and pods
   ```bash
   helm install kubernetes-helm-release ./kubernetes-helm-chart
   ```
Expected output:
   ```bash
   anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ helm install kubernetes-helm-release ./kubernetes-helm-chart
NAME: kubernetes-helm-release
LAST DEPLOYED: Wed Oct  9 14:54:19 2024
NAMESPACE: default
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services kubernetes-helm-release-kubernetes-helm-chart)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
   ```
### Step 5: Verify the deployment
run the below query to verify the pods and services are up and running.
   ```bash
   kubectl get pods
   ```
Expected output:
   ```bash
   anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ kubectl get pods
  NAME                                                             READY   STATUS    RESTARTS      AGE
  kubernetes-helm-release-kubernetes-helm-chart-74897995b7-h5jmm   1/1     Running   0             54s
   ```
## •	Upgrade the application by changing the Helm chart values and performing a Helm upgrade.
### Step 1: Edit the values.yaml and change the replica set to 2 and save the file and upgrade the helm application
run the below command to upgarde the application.
   ```bash
   helm upgrade kubernetes-helm-release ./kubernetes-helm-chart
   ```
expected output:
   ```bash
anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ helm upgrade kubernetes-helm-release ./kubernetes-helm-chart
Release "kubernetes-helm-release" has been upgraded. Happy Helming!
NAME: kubernetes-helm-release
LAST DEPLOYED: Wed Oct  9 16:12:23 2024
NAMESPACE: default
STATUS: deployed
REVISION: 3
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace default -o jsonpath="{.spec.ports[0].nodePort}" services kubernetes-helm-release-kubernetes-helm-chart)
  export NODE_IP=$(kubectl get nodes --namespace default -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
   ```
### step 2: verify the upgarde
run the below command to get the pods.
   ```bash
   kubectl get pods
   ```
expected output:
   ```bash
  anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ kubectl get pods
  NAME                                                             READY   STATUS    RESTARTS      AGE
  kubernetes-helm-release-kubernetes-helm-chart-74897995b7-h5jmm   1/1     Running   0             20m
  kubernetes-helm-release-kubernetes-helm-chart-74897995b7-vp8fw   1/1     Running   0             42s
   ```
## •	Implement a namespace strategy for your Kubernetes cluster, organizing applications and resources into separate namespaces based on their environments (e.g., development, staging, production).
### step 1: create different types of namespace
Run below commands to create different namespaces like development, staging, production.
   ```bash
  kubectl create namespace development
  kubectl create namespace staging
  kubectl create namespace production
   ```
### step 2: verify the different types of namespace
   ```bash
   kubectl get namespaces
   ```
Expected output:
   ```bash
  anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ kubectl get namespaces
  NAME              STATUS   AGE
  default           Active   23d
  development       Active   34s
  kube-node-lease   Active   23d
  kube-public       Active   23d
  kube-system       Active   23d
  production        Active   17s
  staging           Active   24s
   ```
### step 3: Helm installation for the different types of namespace
run the below command to install helm for different namespaces
   ```bash
  helm install kubernetes-release-dev ./kubernetes-helm-chart --namespace development
  helm install kubernetes-release-staging ./kubernetes-helm-chart --namespace staging
  helm install kubernetes-release-prod ./kubernetes-helm-chart --namespace production
   ```
expected output for development namespace helm installation:
   ```bash
anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ helm install kubernetes-release-dev ./kubernetes-helm-chart --namespace development
NAME: kubernetes-release-dev
LAST DEPLOYED: Wed Oct  9 16:48:24 2024
NAMESPACE: development
STATUS: deployed
REVISION: 1
NOTES:
1. Get the application URL by running these commands:
  export NODE_PORT=$(kubectl get --namespace development -o jsonpath="{.spec.ports[0].nodePort}" services kubernetes-release-dev-kubernetes-helm-chart)
  export NODE_IP=$(kubectl get nodes --namespace development -o jsonpath="{.items[0].status.addresses[0].address}")
  echo http://$NODE_IP:$NODE_PORT
   ```
### Verify Deployments in Different Namespaces:
run the below query to verify the deployments
   ```bash
  kubectl get pods -n development
  kubectl get pods -n staging
  kubectl get pods -n production
   ```
## •	Rollback the application to a previous version using Helm rollback.
### step 1: View the Helm Release History:
run below command to get helm release history
   ```bash
   helm history kubernetes-helm-release
   ```
expected output:
 ```bash
  anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ helm history kubernetes-helm-release
  REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION
  1               Wed Oct  9 15:48:40 2024        superseded      kubernetes-helm-chart-0.1.0     1.16.0          Install complete
  2               Wed Oct  9 16:08:12 2024        superseded      kubernetes-helm-chart-0.1.0     1.16.0          Upgrade complete
  3               Wed Oct  9 16:12:23 2024        deployed        kubernetes-helm-chart-0.1.0     1.16.0          Upgrade complete
   ```
### step 2: Rollback to a Previous Version:
run the below command to rollback to previous version
   ```bash
   helm rollback kubernetes-helm-release 2
   ```
### step 3: run the application using ip 
   ```bash
   minikube service kubernetes-helm-release-kubernetes-helm-chart --url
   ```
expected output:

   ```bash
  anandkumar@IN-HZ7K1N3:/mnt/c/Users/anandkumar04/assignment/kubernetes_advance$ minikube service kubernetes-helm-release-kubernetes-helm-chart --url
  http://127.0.0.1:41525
❗  Because you are using a Docker driver on linux, the terminal needs to be open to run it.
   ```
Hit the above url on chrome to test the application




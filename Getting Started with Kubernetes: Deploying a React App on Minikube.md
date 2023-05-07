# Getting Started with Kubernetes: Deploying a React App on Minikube

Deploying a React app on Kubernetes can be a complex process, requiring the configuration of multiple components such as pods, services, deployments, and more. However, using Minikube can simplify this process by providing a local environment where developers can test and debug their application before deploying it to a production Kubernetes cluster.

In this article, we’ll go through the steps to deploy a React app on Minikube.



## Prerequisites
Before you can start deploying your React application on Minikube, you will need to have the following tools installed on your system:

Docker: A containerization platform that allows you to package your application and its dependencies into a Docker image.
Minikube: A tool that allows you to run Kubernetes clusters locally.
kubectl: A command-line tool that allows you to interact with Kubernetes clusters.
If you do not have these tools installed, you can download them from their official websites and install them on your system.

## Step 1: Create a React App and Dockerize It
To deploy a React app on Minikube, first, you need to create a React app and then Dockerize it. If you already have a Dockerized React app, you can skip Step 1 and proceed to Step 2.

To create a React app and Dockerize it, you can follow these steps:

Creating a React app, If you already have a React app then you can move to the next step
`npx create-react-app my-app`
Create a Dockerfile in your app directory by running the following command:

```
##### Base image
FROM node:16-alpine

##### Set the working directory
WORKDIR /app

##### Copy package.json and package-lock.json
COPY package.json ./

##### Install dependencies
RUN npm install --silent

##### Copy the app
COPY . ./

##### Build the app
RUN npm run build

##### Serve the app
CMD ["npm", "start"]
```


Build the Docker image by running the following command in your app directory:

`docker build -t <your-image-name> .`

To check if your Docker image was successfully built, use the command `docker images`

## Step 2: Start Minikube
Before deploying your app, you need to start Minikube. To start Minikube, run the following command in your terminal:

`minikube start`
This command will start a local Kubernetes cluster using Minikube.

## Step 3: Create a Deployment
The next step is to create a Kubernetes deployment for your React app. To do this, create a new file named “deployment.yaml” in your app directory and add the following code:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <your-deployment-name>
spec:
  replicas: 3
  selector:
    matchLabels:
      app: <your-app-name>
  template:
    metadata:
      labels:
        app: <your-app-name>
    spec:
      containers:
        - name: <your-container-name>
          image: <your-image-name>
          ports:
            - containerPort: 3000
```
Note- Make sure to replace the variable with the appropriate names

## Step 4: Create a Service
After creating a deployment, you need to create a Kubernetes service to expose your app to the outside world. To do this, create a new file named “service.yaml” in your app directory and add the following code:
```
apiVersion: v1
kind: Service
metadata:
  name: <your-service-name>
spec:
  selector:
    app: <your-app-name>
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 3000
  type: NodePort
```
Note- Make sure to replace the variable with the appropriate names

## Step 5: Apply Kubernetes Configurations

Now that you have created the deployment and service configurations for your React app, you need to apply these configurations to the Minikube cluster. To do this, run the following commands in your app directory
```
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
These commands will apply the deployment and service configurations to the Minikube cluster.

To check the pods are created and running, run the following command
`kubectl get pods`
To check the service port number, run the following command and note the service port in front of your running service
`kubectl get services`
To access the app, you have to use the IP address and the service’s port number. To find the IP address, run the following command:

`minikube ip`
To access the app, open a web browser and enter the following URL

`http://<minikube-ip-address>:<service-port>`
or can use this command to directly get the url link

`minikube service <myservice> --url`
## Conclusion
Deploying a React app on Minikube can be a simple and efficient process. By following the steps outlined in this article, you can deploy your React app locally and make necessary changes before deploying it to a production environment. With Minikube, you can test and develop your application in a Kubernetes environment without the need for multiple servers and configurations.

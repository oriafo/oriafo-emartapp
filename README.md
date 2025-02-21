# eMart Application

This project provides an eMart application, designed to manage and display products for an e-commerce platform. The application is composed of multiple components, including a frontend, backend, and a robust deployment environment using Kubernetes.

## Features

- **Frontend**: Built with React.js for dynamic, fast UI rendering.
- **Backend APIs**: Implemented using Node.js and Java.
- **Stateful Storage**: Persistent data storage integration using Kubernetes StatefulSet for handling stateful services like databases.
- **Scalability**: Supports automatic scaling through Kubernetes.
- **CI/CD Pipeline**: Automated deployments using Jenkins.

## Architecture

The eMart application architecture consists of the following components:

- **Frontend (Client)**: A React.js-based user interface.
- **Node.js API**: Handles core business logic for product and user management.
- **Java API**: Provides additional backend functionality.
- **Nginx**: Acts as a reverse proxy for routing traffic to services.
- **Database (StatefulSet)**: Stateful service (e.g., MySQL, PostgreSQL) for storing product data.

## Prerequisites

To set up and run this application, ensure that the following tools are installed:

- **Kubernetes**: For managing containerized applications across clusters.
- **kubectl**: Command-line tool for interacting with the Kubernetes cluster.
- **Jenkins**: For continuous integration and deployment automation.
- **Helm (optional)**: For managing Kubernetes applications via Helm charts.
- **Node.js and NPM**: For managing JavaScript dependencies.
- **Java**: For backend services.

## Setup and Installation

1. Clone the repository:

    ```bash
    git clone https://github.com/devopshydclub/emartapp.git
    cd emartapp
    ```
2. **Build Docker images**: If you haven't already built the Docker images for the services (e.g., frontend, backend), follow these instructions:

    ```bash
    docker build -t emart-frontend:latest ./frontend
    docker build -t emart-backend-node:latest ./backend-node
    docker build -t emart-backend-java:latest ./backend-java
    ```

3. **Deploy Kubernetes Resources**

   Apply the Kubernetes resource files:

   - **Frontend Deployment**:

        ```bash
        kubectl apply -f kubernetes/frontend-deployment.yaml
        ```
   - **Backend Deployment:**

        ```bash
        kubectl apply -f kubernetes/backend-deployment.yaml
        ```
   - **Database StatefulSet:**
        ```bash
        kubectl apply -f kubernetes/database-statefulset.yaml
        ```
4. **Check the status of your Kubernetes resources**

    After deploying, check the status of your pods and services:
    
    ```bash
    kubectl get pods
    kubectl get services
    kubectl get statefulsets
    ```
5. **Access the Application**:If you're using a LoadBalancer service type for the frontend, it should expose the application to the outside world. For local development or testing, consider using `kubectl port-forward`.

    ```bash
    kubectl port-forward service/emart-frontend 8080:80
    ```

    Now, you should be able to access the application at [http://localhost:8080](http://localhost:8080).

## CI/CD with Jenkins

- Set up a Jenkins pipeline to automate testing, building, and deployment of your application.
- The Jenkins pipeline can interact with the Kubernetes cluster to deploy and update the application as part of the CI/CD process.



<!-- eMart Application

This project provides an eMart application, designed to manage and display products for an e-commerce platform. The application is composed of multiple components, including a frontend, backend, and a robust deployment environment using Kubernetes.
Features

    Frontend: Built with React.js for dynamic, fast UI rendering.
    Backend APIs: Implemented using Node.js and Java.
    Stateful Storage: Persistent data storage integration using Kubernetes StatefulSet for handling stateful services like databases.
    Scalability: Supports automatic scaling through Kubernetes.
    CI/CD Pipeline: Automated deployments using Jenkins.

Architecture

The eMart application architecture consists of the following components:

    Frontend (Client): A React.js-based user interface.
    Node.js API: Handles core business logic for product and user management.
    Java API: Provides additional backend functionality.
    Nginx: Acts as a reverse proxy for routing traffic to services.
    Database (StatefulSet): Stateful service (e.g., MySQL, PostgreSQL) for storing product data.

Prerequisites

To set up and run this application, ensure that the following tools are installed:

    Kubernetes: For managing containerized applications across clusters.
    kubectl: Command-line tool for interacting with the Kubernetes cluster.
    Jenkins: For continuous integration and deployment automation.
    Helm (optional): For managing Kubernetes applications via Helm charts.
    Node.js and NPM: For managing JavaScript dependencies.
    Java: For backend services.

Setup and Installation

    Clone the repository:

git clone https://github.com/devopshydclub/emartapp.git
cd emartapp

    Build Docker images: If you haven't already built the Docker images for the services (e.g., frontend, backend), follow these instructions:

    docker build -t emart-frontend:latest ./frontend
    docker build -t emart-backend-node:latest ./backend-node
    docker build -t emart-backend-java:latest ./backend-java

    Kubernetes Deployment:

    Use the Kubernetes resources provided below to deploy the application. These resources include Deployment and StatefulSet YAML files for managing pods, services, and persistent storage.

Kubernetes Resource Files
1. Frontend Deployment (kubernetes/frontend-deployment.yaml)

apiVersion: apps/v1
kind: Deployment
metadata:
  name: emart-frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: emart-frontend
  template:
    metadata:
      labels:
        app: emart-frontend
    spec:
      containers:
      - name: frontend
        image: emart-frontend:latest
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: emart-frontend
spec:
  selector:
    app: emart-frontend
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer

2. Backend Deployment (kubernetes/backend-deployment.yaml)

apiVersion: apps/v1
kind: Deployment
metadata:
  name: emart-backend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: emart-backend
  template:
    metadata:
      labels:
        app: emart-backend
    spec:
      containers:
      - name: backend-node
        image: emart-backend-node:latest
        ports:
        - containerPort: 3000
      - name: backend-java
        image: emart-backend-java:latest
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: emart-backend
spec:
  selector:
    app: emart-backend
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 8080
  type: ClusterIP

3. Database StatefulSet (kubernetes/database-statefulset.yaml)

apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: emart-database
spec:
  serviceName: "emart-database"
  replicas: 3
  selector:
    matchLabels:
      app: emart-database
  template:
    metadata:
      labels:
        app: emart-database
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "password"
        ports:
        - containerPort: 3306
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-data
    spec:
      accessModes: ["ReadWriteOnce"]
      resources:
        requests:
          storage: 1Gi

    Deploy Kubernetes Resources:

    Apply the Kubernetes resource files:

        Frontend Deployment:

kubectl apply -f kubernetes/frontend-deployment.yaml

Backend Deployment:

kubectl apply -f kubernetes/backend-deployment.yaml

Database StatefulSet:

    kubectl apply -f kubernetes/database-statefulset.yaml

Check the status of your Kubernetes resources:

After deploying, check the status of your pods and services:

kubectl get pods
kubectl get services
kubectl get statefulsets

Access the Application: If you're using a LoadBalancer service type for the frontend, it should expose the application to the outside world. For local development or testing, consider using kubectl port-forward.

    kubectl port-forward service/emart-frontend 8080:80

    Now, you should be able to access the application at http://localhost:8080.

CI/CD with Jenkins

    Set up a Jenkins pipeline to automate testing, building, and deployment of your application.
    The Jenkins pipeline can interact with the Kubernetes cluster to deploy and update the application as part of the CI/CD process. -->
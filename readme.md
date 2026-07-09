#  Hostel Management System on Kubernetes

![PHP](https://img.shields.io/badge/PHP-8.2-777BB4?logo=php)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?logo=mysql)
![Apache](https://img.shields.io/badge/Apache-2.4-D22128?logo=apache)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Orchestrated-326CE5?logo=kubernetes&logoColor=white)
![Minikube](https://img.shields.io/badge/Minikube-Local%20Cluster-FF6F00)
![License](https://img.shields.io/badge/License-MIT-green)

A **Hostel Management System** built using **PHP** and **MySQL**, containerized with **Docker**, and deployed on a **Kubernetes (Minikube)** cluster. This project demonstrates how a traditional web application can be transformed into a cloud-native application using containerization, orchestration, service discovery, and persistent storage.

---

##  Project Preview
<img width="1359" height="575" alt="warden_dashboard png" src="https://github.com/user-attachments/assets/286cce5c-76a0-493e-b045-e5c2db52768b" />

<p align="center">
    <img src="screenshots/warden_dashboard.png"  width="900">
</p>

<p align="center">
<b>Figure 1:</b> Warden Dashboard of the Hostel Management System running on Kubernetes.
</p>
---
##  Repository Highlights

- Containerized PHP application using Docker
- Deployed on Kubernetes (Minikube)
- Persistent MySQL storage using PV and PVC
- Secure credential management using Kubernetes Secrets
- Internal and external networking using ClusterIP and NodePort Services
- Step-by-step deployment guide included
- Kubernetes manifests organized under the `k8s/` directory

#  Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Technology Stack](#technology-stack)
- [Kubernetes Concepts Used](#kubernetes-concepts-used)
- [System Architecture](#system-architecture)
- [Application Request Flow](#application-request-flow)
- [Project Structure](#-project-structure)
- [Prerequisites](#prerequisites)
- [Deployment Guide](#deployment-guide)
- [Docker Commands](#docker-commands)
- [Kubernetes Commands](#kubernetes-commands)
- [Kubernetes Resource Files](#kubernetes-resource-files)
- [Database Setup](#database-setup)
- [Troubleshooting](#troubleshooting)
- [Challenges Faced](#challenges-faced)
- [Key Learnings](#key-learnings)
- [Future Enhancements](#future-enhancements)
- [License](#license)
- [Author](#author)

---

# Project Overview

The **Hostel Management System** is a web-based application designed to streamline hostel administration by providing dedicated portals for both wardens and students.

The application allows wardens to manage students, allocate hostel rooms, generate login credentials, publish announcements, and monitor complaints. Students can securely log in to view room information, check roommate details, submit complaints, and stay updated with hostel announcements.

To modernize the deployment process, the application has been **containerized using Docker** and deployed on a **local Kubernetes cluster created with Minikube**. Kubernetes automates application deployment, container management, networking, service discovery, and persistent database storage, making the application more scalable, reliable, and maintainable.

This project serves as a practical demonstration of modern **DevOps** practices by integrating application development with **Docker**, **Kubernetes**, and infrastructure automation.

---

# Features

## Warden Module

- Secure Warden Authentication
- Add New Students
- Allocate Hostel Rooms
- Generate Student Login Credentials
- View and Manage Student Complaints
- Publish Hostel Announcements

##  Student Module

- Secure Student Login
- View Room Details
- View Roommates
- Submit Complaints
- Track Complaint Status
- View Hostel Announcements

---

# Technology Stack

| Category | Technology |
|----------|------------|
| Frontend | HTML5, CSS3 |
| Backend | PHP 8.2 |
| Database | MySQL 8 |
| Web Server | Apache |
| Containerization | Docker |
| Container Orchestration | Kubernetes |
| Local Kubernetes Cluster | Minikube |
| CLI | kubectl |

---

# Kubernetes Concepts Used

This project demonstrates the implementation of the following Kubernetes resources.

| Resource | Purpose |
|----------|---------|
| Deployment | Manages Pods and ensures application availability |
| Pod | Runs the PHP and MySQL containers |
| Service | Enables communication between Pods |
| NodePort Service | Exposes the PHP application outside the cluster |
| ClusterIP Service | Enables internal communication between PHP and MySQL |
| Secret | Stores MySQL credentials securely |
| Persistent Volume | Provides persistent storage for the database |
| Persistent Volume Claim | Connects the database Pod to persistent storage |

---

# System Architecture

```text
                 Browser
                    │
                    ▼
          NodePort Service
            (php-service)
                    │
                    ▼
              PHP Deployment
                    │
                    ▼
                 PHP Pod
            (Apache + PHP)
                    │
                    ▼
      ClusterIP Service
       (mysql-service)
                    │
                    ▼
            MySQL Deployment
                    │
                    ▼
               MySQL Pod
                    │
                    ▼
                    PVC
                    │
                    ▼
                     PV
```

---

# Project Structure

```text
Hostel-Management-System/
│
├── Dockerfile
├── README.md
├── hostel.sql
├── index.php
├── db.php
├── dashboard.php
├── student_dashboard.php
├── student_login.php
├── warden_login.php
│
├── k8s/
│   ├── mysql-secret.yaml
│   ├── mysql-pv.yaml
│   ├── mysql-pvc.yaml
│   ├── mysql-deployment.yaml
│   ├── mysql-service.yaml
│   ├── php-deployment.yaml
│   └── php-service.yaml
│
└── screenshots/
    └── project-preview.png
```

---

# Prerequisites

Ensure the following software is installed before deploying the project.

- Docker Desktop
- Minikube
- kubectl
- Git

Verify the installation.

```bash
docker --version
```

```bash
minikube version
```

```bash
kubectl version --client
```

---

# Deployment Guide

### Clone the Repository

```bash
git clone https://github.com/<your-username>/Hostel-Management-System.git

cd Hostel-Management-System
```

---

### Start Minikube

```bash
minikube start
```

---

### Build the Docker Image

```bash
docker build -t hostel-app:v5 .
```

---

### Load the Image into Minikube

```bash
minikube image load hostel-app:v5
```

---

### Deploy Kubernetes Resources

Create the Secret

```bash
kubectl apply -f k8s/mysql-secret.yaml
```

Create the Persistent Volume

```bash
kubectl apply -f k8s/mysql-pv.yaml
```

Create the Persistent Volume Claim

```bash
kubectl apply -f k8s/mysql-pvc.yaml
```

Deploy MySQL

```bash
kubectl apply -f k8s/mysql-deployment.yaml
kubectl apply -f k8s/mysql-service.yaml
```

Deploy PHP Application

```bash
kubectl apply -f k8s/php-deployment.yaml
kubectl apply -f k8s/php-service.yaml
```

Verify all resources.

```bash
kubectl get pods
```

```bash
kubectl get svc
```

---

# Access the Application

```bash
minikube service php-service
```

The application will automatically open in your default web browser.

---

# Database Setup

After deploying the Kubernetes resources, import the MySQL database into the running MySQL Pod.

### Verify the MySQL Pod

```bash
kubectl get pods
```

### Copy the SQL File into the MySQL Pod

```bash
kubectl cp hostel.sql <mysql-pod-name>:/hostel.sql
```

Example:

```bash
kubectl cp hostel.sql mysql-deployment-xxxxxxxx-xxxxx:/hostel.sql
```

### Open the MySQL Container

```bash
kubectl exec -it deployment/mysql-deployment -- mysql -u root -p
```

Enter the MySQL root password when prompted.

### Create the Database

```sql
CREATE DATABASE hostel;
```

### Select the Database

```sql
USE hostel;
```

### Import the Database

```sql
SOURCE /hostel.sql;
```

---

# Kubernetes Resource Files

The project uses multiple Kubernetes manifest files to deploy and manage the application.

| File | Description |
|------|-------------|
| mysql-secret.yaml | Stores MySQL credentials securely |
| mysql-pv.yaml | Creates the Persistent Volume |
| mysql-pvc.yaml | Creates the Persistent Volume Claim |
| mysql-deployment.yaml | Deploys the MySQL database |
| mysql-service.yaml | Creates the ClusterIP Service for MySQL |
| php-deployment.yaml | Deploys the PHP application |
| php-service.yaml | Creates the NodePort Service for external access |

---

# Communication Flow

The communication between different Kubernetes components is shown below.

```text
Client
   │
   ▼
NodePort Service
   │
   ▼
PHP Pod
   │
   ▼
ClusterIP Service (mysql-service)
   │
   ▼
MySQL Pod
   │
   ▼
Persistent Volume Claim
   │
   ▼
Persistent Volume
```

### Explanation

1. A user accesses the application through a web browser.
2. The request reaches the **NodePort Service**, which exposes the application outside the Kubernetes cluster.
3. The request is forwarded to the **PHP Pod**.
4. The PHP application processes the request.
5. Whenever data is required, PHP communicates with **mysql-service**.
6. The **ClusterIP Service** routes the request to the MySQL Pod.
7. The MySQL database reads and writes data stored in the **Persistent Volume** through the **Persistent Volume Claim**.
8. The processed response is returned to the PHP application.
9. Finally, the generated web page is sent back to the user's browser.

---

# Useful Docker Commands

### Build Docker Image

```bash
docker build -t hostel-app:v5 .
```

### List Docker Images

```bash
docker images
```

### Run Docker Container

```bash
docker run -d -p 8080:80 hostel-app:v5
```

### Stop Running Containers

```bash
docker stop <container-id>
```

### Remove Docker Image

```bash
docker rmi hostel-app:v5
```

---

# Useful Kubernetes Commands

### View Running Pods

```bash
kubectl get pods
```

### View Services

```bash
kubectl get svc
```

### View Deployments

```bash
kubectl get deployments
```

### Describe a Pod

```bash
kubectl describe pod <pod-name>
```

### View Pod Logs

```bash
kubectl logs <pod-name>
```

### Execute Commands Inside a Pod

```bash
kubectl exec -it <pod-name> -- bash
```

### Restart a Deployment

```bash
kubectl rollout restart deployment/php-deployment
```

### Check Rollout Status

```bash
kubectl rollout status deployment/php-deployment
```

### Delete All Resources

```bash
kubectl delete -f k8s/
```

---

# Troubleshooting

### Application is not accessible

Check whether the PHP Pod is running.

```bash
kubectl get pods
```

Ensure the NodePort Service has been created.

```bash
kubectl get svc
```

---

### MySQL Connection Failed

Verify that:

- MySQL Pod is running.
- The database credentials stored in the Secret are correct.
- `db.php` uses the MySQL Service name instead of a Pod IP.

Example:

```php
$servername = "mysql-service";
```

---

### Database Tables Missing

Ensure the SQL file has been imported successfully.

```sql
USE hostel;

SHOW TABLES;
```

---

### Changes Not Reflected

If a new Docker image has been built, load it into Minikube again.

```bash
minikube image load hostel-app:v5
```

Then restart the deployment.

```bash
kubectl rollout restart deployment/php-deployment
```

---

### Verify Everything

```bash
kubectl get pods
```

```bash
kubectl get svc
```

Both the PHP and MySQL Pods should be in the **Running** state before accessing the application.

---

# Challenges Faced

During the development and deployment of this project, several practical challenges were encountered while containerizing and orchestrating the application.

## Docker Image Updates

After modifying the PHP source code, the running container did not reflect the changes immediately. This required rebuilding the Docker image, loading it into Minikube, and restarting the deployment to apply the latest version.

## Database Connectivity

Initially, the PHP application could not communicate with the MySQL database because the application attempted to connect using an incorrect host. The issue was resolved by using the Kubernetes **ClusterIP Service (`mysql-service`)**, which provides a stable endpoint for database communication.

## Database Initialization

The MySQL container initially started with an empty database. The database schema and sample data were imported manually using the provided SQL script to initialize the application.

## Persistent Storage

Ensuring that database data survived Pod recreation required configuring a **Persistent Volume (PV)** and **Persistent Volume Claim (PVC)**.

## Debugging Kubernetes Resources

Several Kubernetes debugging commands such as `kubectl logs`, `kubectl describe`, and `kubectl exec` were used to diagnose deployment, networking, and database-related issues during development.

---

# Key Learnings

This project provided hands-on experience with modern containerization and orchestration technologies.

The major concepts learned include:

- Docker image creation and management
- Kubernetes Deployments
- Pods and container lifecycle
- Service discovery using ClusterIP Services
- External application exposure using NodePort Services
- Secret management for sensitive credentials
- Persistent storage using PV and PVC
- Managing applications using Minikube
- Debugging Kubernetes workloads using kubectl
- Communication between multiple containers within a Kubernetes cluster

---

# Future Enhancements

The current project demonstrates the deployment of a PHP and MySQL application on a local Kubernetes cluster. The following improvements can further enhance the project.

- Deploy the application on Azure Kubernetes Service (AKS)
- Implement an Ingress Controller for HTTP routing
- Enable HTTPS using TLS certificates
- Configure Horizontal Pod Autoscaling (HPA)
- Introduce ConfigMaps for application configuration
- Automate deployment using GitHub Actions CI/CD
- Package Kubernetes resources using Helm Charts
- Store Docker images in Docker Hub or Azure Container Registry (ACR)
- Add monitoring using Prometheus and Grafana

---

# Skills Demonstrated

This project demonstrates practical knowledge in the following areas.

### Backend Development

- PHP
- MySQL
- Apache

### Containerization

- Docker
- Docker Images
- Docker Containers

### Kubernetes

- Deployments
- Pods
- Services
- Secrets
- Persistent Volumes
- Persistent Volume Claims
- Minikube
- kubectl

### DevOps

- Container Orchestration
- Service Discovery
- Persistent Storage
- Infrastructure as Code
- Kubernetes Debugging

---

# References

- Docker Documentation
- Kubernetes Documentation
- Minikube Documentation
- PHP Documentation
- MySQL Documentation

---

# License

This project is licensed under the MIT License.

Feel free to use, modify, and distribute this project for educational purposes.

---

# Author

**Balasoorya D A**


# CI/CD Pipeline for Spring Boot Java Application

This project implements a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Spring Boot-based Java web application using Jenkins, Docker, SonarQube, and Argo CD.

## Overview
The CI/CD pipeline automates the build, testing, static code analysis, and deployment processes. The heart of the project lies in its CI/CD pipeline, which ensures continuous feedback on code quality, consistent build environments, and smooth, automated deployments. This approach minimizes manual intervention and accelerates the development lifecycle.

## CI/CD Pipeline Steps

### 1. **Checkout**
- The pipeline starts by checking out the latest code from the GitHub repository.

### 2. **Build Application using Maven Docker Image**
- The application is built using a **Maven Docker image**, ensuring a consistent environment for every build. The Maven image compiles the Java application and packages it into a `.war` file.

### 3. **Static Code Analysis**
- **SonarQube** is utilized for static code analysis to detect code smells, bugs, and vulnerabilities. The credentials for SonarQube are securely fetched from **Jenkins credentials**.

### 4. **Build and Push Docker Image**
- The application `.war` file is incorporated into a Docker image, built using the Dockerfile from the repository.
- The Docker image is tagged with the Jenkins build number for versioning and then pushed to **Docker Hub**. The Docker Hub credentials are fetched from **Jenkins credentials** for secure access.

### 5. **Update Deployment File in GitHub**
- A script automatically updates the Kubernetes deployment YAML file in the GitHub repository with the new Docker image tag (based on the Jenkins build number). This ensures that the latest Docker image is used in deployments.
- The **GitHub credentials** stored in Jenkins are used to securely push these changes back to the GitHub repository.

### 6. **Deploy with Argo CD**
- **Argo CD** synchronizes the Kubernetes cluster with the updated deployment YAML from GitHub, ensuring the latest version of the application is deployed.

## Key Achievements
- **Automated Build and Deployment**: The pipeline automates the complete build-test-deploy cycle, enabling continuous integration and delivery.
- **Dockerized Build Environment**: Using a Maven Docker image for the build process ensures consistency across different environments.
- **Containerized Deployment**: The application is containerized using Docker, ensuring scalability and efficient resource management when deployed on Kubernetes.
- **Automatic Image Update**: The pipeline automates the update of Docker images on Docker Hub, and updates the deployment YAML in GitHub with the new image details, ensuring the Kubernetes cluster always deploys the latest image.

## Conclusion
The CI/CD pipeline is the central component of this project, enabling the automation of building, testing, and deploying the Spring Boot application. By integrating Docker, SonarQube, and Argo CD, the pipeline ensures a seamless, efficient, and secure process from code commit to production deployment.

## Additional Setup Notes
- Jenkins is configured to pull the pipeline script from the SCM (Source Code Management) system. The pipeline script is defined in the repository.
- Docker, Jenkins, and SonarQube are installed on the server to facilitate the build and analysis processes.
- Sonarqube,Docker hub and Github credentials are integrated with Jenkins for seamless access to repositories and registries.
- The SonarQube server URL is adjusted in the repository files to fit your environment.

## Deployment with Minikube and Argo CD
- **Minikube** is installed on a separate server to simulate a local Kubernetes cluster.
- **Argo CD** is installed via Helm to manage the deployment, ensuring that the application is synchronized with the latest Docker image.
- To access the Argo CD server, run the following command:

  kubectl port-forward service/argocd-server -n argocd --address 0.0.0.0 8080:443
  

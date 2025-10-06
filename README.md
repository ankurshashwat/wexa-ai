# DevOps Assessment: Containerized Next.js Application

This repository contains a simple Next.js application that has been containerized with Docker and is set up for automated deployment to a Kubernetes cluster (Minikube) using GitHub Actions.

## 📋 Table of Contents
1.  [Objective](#-objective)
2.  [Prerequisites](#-prerequisites)
3.  [Running Locally with Docker](#-running-locally-with-docker)
4.  [CI/CD with GitHub Actions](#-ci/cd-with-github-actions)
5.  [Deployment to Minikube](#-deployment-to-minikube)
6.  [Accessing the Application](#-accessing-the-application)

## 🎯 Objective
The goal of this project is to demonstrate proficiency in:
* **Containerization**: Using Docker with multi-stage builds for an optimized and secure image.
* **Automation (CI/CD)**: Building and pushing Docker images automatically to GHCR using GitHub Actions.
* **Orchestration**: Deploying the containerized application to Kubernetes using declarative manifest files.

## 🛠️ Prerequisites
Before you begin, ensure you have the following tools installed:
* [Docker](https://www.docker.com/get-started)
* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## 💻 Running Locally with Docker

You can build and run this application on your local machine using Docker.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/ankurshashwat/wexa-ai.git
    cd wexa-ai
    ```

2.  **Build the Docker image:**
    ```bash
    docker build -t nextjs-local-app .
    ```

3.  **Run the Docker container:**
    ```bash
    docker run -p 3000:3000 nextjs-local-app
    ```

4.  **Access the application:**
    Open your web browser and navigate to `http://localhost:3000`.

## 🚀 CI/CD with GitHub Actions

This project uses GitHub Actions to automate the building and publishing of the Docker image.

* **Workflow file**: `.github/workflows/main.yml`
* **Trigger**: The workflow runs automatically on every `push` to the `main` branch.
* **Actions**:
    1.  Checks out the code.
    2.  Logs into GitHub Container Registry (GHCR).
    3.  Builds the Docker image and pushes it to GHCR with two tags:
        * `latest`
        * The commit SHA (e.g., `a1b2c3d4`) for versioning.

The published image can be found at:
`ghcr.io/ankurshashwat/wexa-ai:latest`

## 📦 Deployment to Minikube

Follow these steps to deploy the application to your local Kubernetes cluster.

1.  **Start Minikube:**
    ```bash
    minikube start
    ```

2.  **Update the Deployment Manifest:**
    Before deploying, you **must** edit the `k8s/deployment.yaml` file. Replace the placeholder image URL with the URL of the image you pushed to GHCR.

    Find this line:
    ```yaml
    image: ghcr.io/ankurshashwat/wexa-ai:latest
    ```
    And change it to your actual image URL.

3.  **Apply the Kubernetes Manifests:**
    Use `kubectl` to apply the deployment and service configurations from the `k8s` folder.
    ```bash
    kubectl apply -f k8s/
    ```
    This command will create the Deployment and the Service in your Minikube cluster.

4.  **Verify the Deployment:**
    Check that the pods are running correctly.
    ```bash
    kubectl get pods
    ```
    You should see two pods with a status of `Running`.

## 🌐 Accessing the Application

Once deployed, you can access the application through the `NodePort` service.

1.  **Get the application URL:**
    Minikube provides a handy command to get the URL for a service.
    ```bash
    minikube service nextjs-service --url
    ```

2.  **View in browser:**
    The command will output a URL (e.g., `http://192.168.49.2:30001`). Open this URL in your web browser to see the running Next.js application.

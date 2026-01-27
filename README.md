# Flask App CI/CD with Jenkins, Ansible & Docker

This project demonstrates a fully automated CI/CD pipeline for a Python Flask web application. It uses Jenkins to orchestrate the workflow and Ansible to handle the container builds and deployment to Docker Hub.

## 🚀 System Architecture

1.  **Source Control**: Code is managed in GitHub.
2.  **CI/CD Pipeline**: Jenkins triggers a build on every commit.
3.  **Automation**: Jenkins calls an **Ansible Playbook** (`build-push.yml`).
4.  **Containerization**: Ansible builds the Docker image on a remote worker node.
5.  **Registry**: The final image is versioned and pushed to **Docker Hub**.

## 🛠 Tech Stack
*   **Backend:** Python / Flask
*   **CI/CD:** Jenkins
*   **Configuration Management:** Ansible
*   **Containerization:** Docker
*   **Registry:** Docker Hub

## 📂 Project Structure
*   `app.py` - Flask application logic.
*   `Jenkinsfile` - Declarative pipeline defining the build stages.
*   `build-push.yml` - Ansible playbook for remote Docker operations.
*   `Dockerfile` - Container build instructions.

## ⚙️ CI/CD Setup

### 1. Jenkins Credentials
The pipeline requires the following credentials configured in Jenkins:
*   `dockerhub-credentials`: Docker Hub username and password.
*   `worker-ssh-key`: SSH private key to access the Ansible worker nodes.

### 2. Ansible Inventory
Ensure your Jenkins agent has an inventory file at `/etc/ansible/hosts` containing your `docker_workers` group:
```ini
[docker_workers]
worker-node-ip ansible_user=ubuntu

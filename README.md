# 🌐 Automation of Website Deployment with Continuous Integration Using Jenkins

This project builds upon previous work introducing horizontal scalability for the Tooling Website by enabling the deployment of multiple web servers. While manually configuring a small number of servers is feasible, scaling to dozens or hundreds requires automation to ensure efficiency, accuracy, and repeatability.

Jenkins is widely used to implement Continuous Integration (CI), a practice where developers frequently commit small, incremental changes that are automatically built and tested to improve development speed and quality.

In this project, Jenkins is configured to continuously integrate and deploy updates to the NFS Server. Any change pushed to the GitHub repository triggers an automated pipeline that builds, tests, and deploys the updated application. This automation ensures a fast, consistent, and reliable deployment workflow—supporting scalable infrastructure and modern DevOps best practices.

<img width="925" height="494" alt="image" src="https://github.com/user-attachments/assets/5d940bb7-e00b-4adb-8367-6344e6ab8e34" />

---

# Step 1: Install Jenkins Server

> Launch an Ubuntu EC2 instance and name it Jenkins

> **Configure Security Group to allow:**

> Port 22 — SSH (Secure Shell): Used for remote server management and administrative access.

> Port 8080 — Jenkins Web UI: Default HTTP port used to access the Jenkins web interface for build and pipeline management.

<img width="1158" height="613" alt="image" src="https://github.com/user-attachments/assets/6ea65f87-a535-452a-95ae-8027eaa5e619" />


<img width="1164" height="478" alt="image" src="https://github.com/user-attachments/assets/d33d42fa-6420-4334-a3d3-e5af4e8d1405" />

## Connect to the server via SSH, then update the system packages and install the required dependencies.

```
sudo apt update

sudo apt install -y fontconfig openjdk-17-jre
```

## Add the Jenkins repository key and configure the Jenkins package repository on the server.

```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

- Install Jenkins:

```
sudo apt-get update

sudo apt-get install jenkins -y
```

- Start and enable Jenkins:

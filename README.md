# 🌐 Automation of Website Deployment with Continuous Integration Using Jenkins

This project builds upon previous work introducing horizontal scalability for the Tooling Website by enabling the deployment of multiple web servers. While manually configuring a small number of servers is feasible, scaling to dozens or hundreds requires automation to ensure efficiency, accuracy, and repeatability.

Jenkins is widely used to implement Continuous Integration (CI), a practice where developers frequently commit small, incremental changes that are automatically built and tested to improve development speed and quality.

In this project, Jenkins is configured to continuously integrate and deploy updates to the NFS Server. Any change pushed to the GitHub repository triggers an automated pipeline that builds, tests, and deploys the updated application. This automation ensures a fast, consistent, and reliable deployment workflow—supporting scalable infrastructure and modern DevOps best practices.

<img width="925" height="494" alt="image" src="https://github.com/user-attachments/assets/5d940bb7-e00b-4adb-8367-6344e6ab8e34" />

---

# Step 1: Install Jenkins Server

> Launch an Ubuntu EC2 instance and name it Jenkins

- **Configure Security Group to allow:**

> Port 22 — SSH (Secure Shell): Used for remote server management and administrative access.

> Port 8080 — Jenkins Web UI: Default HTTP port used to access the Jenkins web interface for build and pipeline management.

<img width="1158" height="613" alt="image" src="https://github.com/user-attachments/assets/6ea65f87-a535-452a-95ae-8027eaa5e619" />


<img width="1164" height="478" alt="image" src="https://github.com/user-attachments/assets/d33d42fa-6420-4334-a3d3-e5af4e8d1405" />

## Connect to the server via SSH, then update the system packages and install the required dependencies.

```
sudo apt update

sudo apt install -y fontconfig openjdk-17-jre
```

<img width="1320" height="737" alt="image" src="https://github.com/user-attachments/assets/3c5b6445-c1ef-460d-a487-52ea97dfd69f" />


## Add the Jenkins repository key and configure the Jenkins package repository on the server.

```
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | \
  sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
```

### - Install Jenkins:

```
sudo apt-get update

sudo apt-get install jenkins -y
```


<img width="1320" height="737" alt="image" src="https://github.com/user-attachments/assets/14cf66f6-f30f-44bb-ae89-dac0b1fcef68" />


- Start and enable Jenkins:
```
sudo systemctl start jenkins

sudo systemctl enable jenkins

sudo systemctl status jenkins
```


<img width="1320" height="532" alt="image" src="https://github.com/user-attachments/assets/98dbc760-c820-4a99-9de5-ff634de11dfb" />


### Jenkins Setup:

- Access Jenkins through your web browser at `http://<Jenkins-server-public-ip>:8080`

- You will be prompted for an admin password. Retrieve it from the server:

```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/9c0008de-b0e5-4413-8c3c-a85f63c65267" />


- Once you log in, you will be asked to choose plugins. Select the suggested plugins.


<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/1f9f2209-d4a1-4dc9-9b17-fc0e11f742bf" />


<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/8f4b02c4-352d-4475-8d08-6c489819d5e8" />


- Create an admin user (Username, Password, Full name, and Email):

<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/ad41224c-7b83-45a8-9bd7-ce3af0036ffe" />


<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/7974faaf-f13c-4c5c-93f0-040a546d1454" />


<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/5dee7be9-da4b-4c5f-8c46-3127334a5146" />



# Step 2 :  Configure Jenkins to retrieve source codes from GitHub using Webhooks

- Enable Webhooks in your GitHub repository settings by following this guide:

- Go to your "GitHub repository"

> Click "Settings"

>  Click "Webhooks"

>  Click "Add Webhook"

>  "Payload URL" (http://13.48.42.45:8080/github-webhook/)

>  "Content Type" (application/json)

>  Select “Just the push event”

>  Click on “Add Webhook”


<img width="1320" height="687" alt="Screenshot From 2025-11-30 21-10-37" src="https://github.com/user-attachments/assets/64c2a42e-0419-4131-8a99-06410df2e2f0" />


<img width="1320" height="687" alt="Screenshot From 2025-11-30 21-11-20" src="https://github.com/user-attachments/assets/26407839-d5a7-4b33-84d9-6dffff78fba5" />


- Create a Jenkins Freestyle Project:

> Go to Jenkins web console

> Click "New Item" and create a "Freestyle project"

> Item Name: tooling_github

> Select “OK”


<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/f20465ca-b9d2-46b8-b4b1-7db3e159b1b2" />


- To connect our GitHub repository, we need its URL, e.g:
```
https://github.com/bienary/tooling.git
```

- Go to your GitHub repository and copy your repository link.


<img width="1320" height="687" alt="Screenshot From 2025-11-30 21-29-38" src="https://github.com/user-attachments/assets/5ccc2847-a167-484a-a8ad-bca103b6ddef" />


- In the Jenkins project configuration page, click the Source Code Management section.

- Select Git as the repository source.

- In the Repository URL field, enter the URL of your Tooling GitHub repository. `https://github.com/bienary/tooling.git`

<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/6fa973c8-4a0c-4eb4-9a2e-cca7550e0665" />

- Click the "Build Now" button. If you have configured everything correctly, the build will be successful.
- Under “Branch Specifier,” select "Main" or "Master" depending on your GitHub repository branch
- Save the configuration.


<img width="1320" height="687" alt="image" src="https://github.com/user-attachments/assets/ded3e40a-a4cd-4c31-88f6-05f6656657c5" />


<img width="1320" height="394" alt="image" src="https://github.com/user-attachments/assets/292127fd-a777-4e0d-8bfc-4ee39996189c" />



- Configure for automated builds:


> Go to "Configure"

> Click Triggers

> Select “GitHub hook trigger for GITScm polling"

> Go to Post-build actions

> Select “Archive the artifacts”.

> Type ** under files to archive and then Save.


<img width="1315" height="428" alt="image" src="https://github.com/user-attachments/assets/af13b042-a01a-4a3f-b44e-b7219ef2f1a7" />


<img width="1315" height="615" alt="image" src="https://github.com/user-attachments/assets/52591ef3-c9ba-4c0a-bb47-a7fa3afdc608" />


- Now update the README.md file in your GitHub repository (for example, make a small edit) and push the change to the main branch. You’ll notice that a new build is triggered automatically via the webhook, and you can view its results—including the artifacts—on the Jenkins server.


<img width="1305" height="674" alt="image" src="https://github.com/user-attachments/assets/de907339-28d0-4d40-994e-38892bbccc19" />


> We’ve set up an automated Jenkins job that gets files from GitHub through webhook triggers. Since GitHub initiates the transfer whenever changes are pushed, this approach is considered a “push” method.


- By default, Jenkins stores artifacts locally on the server:

```
ls /var/lib/jenkins/jobs/tooling_github/builds/4/archive/
```

<img width="1323" height="124" alt="image" src="https://github.com/user-attachments/assets/02e0c89e-c9e5-47fa-8984-84fcc03e3058" />

---


## Step 3: Set up Jenkins to transfer files to the NFS server using SSH.

- Now that our artifacts are stored locally on the Jenkins server, let’s configure Jenkins to copy them to the NFS server.

- We’ll use the “Publish Over SSH” plugin for this purpose.

- Install the Publish Over SSH Plugin:

- From the Jenkins dashboard, go to `Manage Jenkins` → `Manage Plugins`.

- Click the `Available tab`.

- Search for `Publish over SSH` and install it without restarting.

<img width="1311" height="638" alt="Screenshot From 2025-12-02 01-39-18" src="https://github.com/user-attachments/assets/126fe3f9-37ae-4fef-a9a3-61037aeb130e" />


- ### Configure Jenkins to copy artifacts to the NFS server:
- On the main dashboard, go to "Manage Jenkins"
> Click on "System"

> Scroll down to the "Publish over SSH" section and configure the Plugin to connect to your NFS server.

> Key: Provide your private key used to connect to the NFS server via SSH

> SSH Server Name: NFS-Server

> Hostname: The private IP address of your NFS server

> Username: ec2-user

> Remote directory: /mnt/apps

Test the configuration and make sure the connection returns Success.


### Add a New Post-Build Action in Jenkins

> From the Jenkins Dashboard, navigate to Manage Jenkins → System.

> Scroll down to the Publish over SSH section.

> Click Add Post-build Action.

> Select Send build artifacts over SSH.

> In the Source files field, enter: `**`

> Update the Key field: replace the existing key with the new private key generated on the Jenkins server

> Click Save to apply the changes.

<img width="1299" height="612" alt="image" src="https://github.com/user-attachments/assets/a3846965-234d-43e5-8477-6da4ee6cb0df" />


<img width="1298" height="419" alt="image" src="https://github.com/user-attachments/assets/d412d012-4504-4969-a6c0-dc61cbafb8ad" />

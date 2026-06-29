Project: Nature Explorer Website with CI/CD:

Phase 1: Create the Application

Step 1: Create a project folder

mkdir nature-app

cd nature-app

Step 2: Create files

touch index.html style.css script.js Dockerfile Jenkinsfile

Step 3: Add code

Paste the Nature Explorer HTML code into index.html

Paste the CSS code into style.css

Paste the JavaScript code into script.js

Step 4: Test locally

Open index.html in your browser and verify:

Phase 2: Containerize the Application

Step 5: Create Dockerfile

Step 6: Install Docker

On our Linux machine:

sudo apt update

sudo apt install docker.io -y

Verify:

docker --version

Step 7: Build Docker Image

docker build -t nature-app .

Step 8: Run Container

docker run -d -p 80:80 --name nature-app nature-app

Check:

docker ps

Open:

http://localhost

You should see the Nature Explorer website.

Phase 3: Store Code in GitHub

Step 9: Create GitHub Repository

Use GitHub

Repository name: Nature-Application

nature-app

Step 10: Push Code
git init

git add .

git commit -m "Initial Commit"

git branch -M main

git remote add origin https://github.com/manjulakadari123/Nature-Application.git

git push -u origin main

Phase 4: Create AWS Server

Step 11: Launch EC2

Use AWS Console

Create Ubuntu EC2.

Allow:

SSH (22)

HTTP (80)

Jenkins (8080)

Step 12: Connect

ssh -i key.pem ubuntu@<public-ip>

Phase 5: Install Jenkins

Step 13: Install Java

sudo apt update

sudo apt install openjdk-21-jdk -y

Step 14: Install Jenkins

sudo wget -O /etc/apt/keyrings/jenkins-keyring.asc \
  https://pkg.jenkins.io/debian-stable/jenkins.io-2026.key
echo "deb [signed-by=/etc/apt/keyrings/jenkins-keyring.asc]" \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
  
sudo apt update

sudo apt install jenkins

Step 15: Start Jenkins

sudo systemctl start jenkins

sudo systemctl enable jenkins

Get password:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Open:

http://<public-ip>:8080

Phase 6: Install Docker on EC2

Step 16: Install Docker

sudo apt install docker.io -y

Step 17: Add Jenkins to Docker Group

sudo usermod -aG docker jenkins

sudo systemctl restart jenkins

Phase 7: Docker Hub

Step 18: Create Docker Hub Account

Go to:

Docker Hub

Create repository:

nature-app

Example image:

manjulakadari/nature-app

Phase 8: Configure Jenkins

Step 19: Install Plugins

Manage Jenkins → Plugins

Install:

Git

GitHub

Docker Pipeline

Restart Jenkins.

Step 20: Add Docker Hub Credentials

Manage Jenkins → Credentials

Add:

Username

Password / Access Token

ID: dockerhub- creds


Phase 9: Create Jenkinsfile

Step 21: Add Jenkinsfile to GitHub

This file will:

Pull code

Build image

Push image to Docker Hub

Deploy container

(Use the Docker Agent Jenkinsfile shared earlier.)

Commit and push:

git add Jenkinsfile

git commit -m "Added Jenkinsfile"

git push

Phase 10: Create Pipeline

Step 22: Create Pipeline Job

Jenkins → New Item

Name:

Nature-App-CICD

Select:

Pipeline

Step 23: Configure SCM

Repository:

https://github.com/<manjulakadari123>/Nature-Application.git

Save.

Phase 11: Run Pipeline

Step 24: Build Now

Click:

Build Now

Pipeline stages:

Checkout
↓
Build Docker Image
↓
Docker Login
↓
Push Image
↓
Deploy Container

Phase 12: Configure Auto Trigger

Step 25: Create Webhook

GitHub → Settings → Webhooks

Payload URL:

http://<EC2-IP>:8080/github-webhook/

Content Type:

application/json

Save.

Phase 13: Test CI/CD

Step 26: Change Website

Edit:

<h1>Nature Explorer V2</h1>

Push:

git add .

git commit -m "Updated Home Page"

git push

Result

GitHub Push
      ↓
Webhook
      ↓
Jenkins Pipeline
      ↓
Docker Build
      ↓
Docker Hub Push
      ↓
AWS Deployment

This is a complete beginner-friendly DevOps project that showcases Linux, Git, GitHub, Jenkins, Docker, Docker Hub, AWS EC2, CI/CD, and Docker Agents on your resume and LinkedIn.

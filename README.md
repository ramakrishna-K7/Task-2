                              TASK 2: Create a Simple Jenkins Pipeline for CI/CD


________________________________________
STEP 1: Launch EC2 Instance
•	AMI: Amazon Linux Kernel 5.10
•	Instance Type: t2.micro
•	EBS Volume: 8GB
________________________________________
STEP 2: Install Docker
yum install docker -y
systemctl start docker
chmod 777 /var/run/docker.sock
________________________________________
STEP 3: Install Jenkins
yum install jenkins -y
systemctl start jenkins
Install Jenkins Plugins

Go to:
•	Dashboard → Manage Jenkins → Plugins → Available Plugins
Install the following plugins:
1.	Pipeline Stage View
Configure Plugins (Global Tool Configuration)

•	JDK: Check "Install automatically" and select version 17.0.8.1+1
![image](https://github.com/user-attachments/assets/b6028f1c-798b-47ef-a2e8-4fd21be37fa7)

 
•	Git: Check "Install automatically"


Add Credentials in Jenkins
Go to:
•	Dashboard → Manage Jenkins → Credentials → System → Global Credentials → Add Credentials
Git Credentials:
•	Username: Ramakrishna-k7
•	Password: (GitHub password or token)
•	Credentials ID: git
DockerHub Credentials:
•	Username: ramakrishna737
•	Password: (DockerHub password)
•	Credentials ID: docker-passwd


Go to Jenkins:
•	Dashboard → new item → pipeline 
•	Under the triggers select the GitHub Hook  trigger for GITScm polling
•	Under pipeline select the pipeline script from SCM

________________________________________
STEP 4: Prepare GitHub Repository
Project structure:
Jenkins-pipeline.yml
Dockerfile
index.html   



Simple Dockerfile----

FROM httpd:2.4
COPY . /usr/local/apache2/htdocs/

Simple index.html----

GitHub  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Simple Web Page</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f4;
            text-align: center;
            padding: 50px;
        }
        h1 {
            color: #333;
        }
        p {
            color: #666;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <h1>Welcome to My Website</h1>
    <p>This is a simple HTML page for testing or demo purposes.</p>
</body>
</html> 
________________________________________

STEP 5: Configure Webhook
Enable GitHub Webhook to trigger Jenkins builds:
1.	Go to GitHub Repository → Settings → Webhooks → Add Webhook
2.	Payload URL: http://<your-jenkins-url>/github-webhook/
3.	Content Type: application/json
4.	Trigger: Only Push events
________________________________________

STEP 6: Jenkins Pipeline Script
Use this script in your Jenkins Pipeline job configuration:

pipeline {
    agent any

    stages {
        stage('code checkout from github') {
            steps {
                git branch: 'main', credentialsId: 'git', url: 'https://github.com/ramakrishna-K7/Task-2.git'
            }
        }
        stage('build'){
            steps{
               echo 'tools is not mentioned'
            }
        }
        stage('test'){
            steps{
                echo 'tools is not mentioned'
            }
        }
        stage('deploy'){
            steps{
                sh 'docker build -t apache . '
                sh ' docker run -d --name thunder -p 80:80 apache'
            }
        }
    }
}
________________________________________
 ![image](https://github.com/user-attachments/assets/3fea1d23-0d3b-421d-93aa-4d73f7b65142)


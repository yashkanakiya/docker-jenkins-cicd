# 🚀 CI/CD Project: Jenkins + Docker + Flask

This project demonstrates a **real-world CI/CD pipeline** using **Jenkins running inside Docker** to automatically build and deploy a **Flask application** as a Docker container.

It is designed to clearly explain **architecture, flow, and troubleshooting**, making it **interview-ready**.

---

## 📌 Project Objective

✔ Automate application build and deployment
✔ Use Jenkins Pipeline from GitHub (SCM)
✔ Build Docker image automatically
✔ Deploy Flask app using Docker container
✔ Demonstrate production-style CI/CD workflow

---

## 🧱 Architecture Overview

```
GitHub Repository
      ↓ (SCM)
Jenkins (Docker Container)
      ↓
Docker CLI → Docker Daemon (Host)
      ↓
Build Image → Run Flask App Container
```

### 🔑 Key Concept

Jenkins runs **inside a Docker container**, but builds and runs Docker images using the **host Docker daemon** via:

```
/var/run/docker.sock
```

This is a common **industry-standard setup**.

---

## 🛠️ Tech Stack

* **Jenkins** (CI/CD tool)
* **Docker** (Containerization)
* **GitHub** (Source Code Management)
* **Python Flask** (Sample Application)
* **Linux-based containers**

---

## 📂 Project Structure

```
docker-jenkins-cicd/
│── app.py
│── Dockerfile
│── Jenkinsfile
│── README.md
```

---

## ⚙️ Step 1: Run Jenkins with Docker Access

> ⚠️ **Windows users must use PowerShell (not CMD)**

```powershell
docker run -d `
  --name jenkins `
  -p 8080:8080 `
  -p 50000:50000 `
  -v jenkins_home:/var/jenkins_home `
  -v /var/run/docker.sock:/var/run/docker.sock `
  jenkins/jenkins:lts
```

### ✅ Why this is Important

* Allows Jenkins to communicate with host Docker
* Enables Docker image build & container run
* Without this → `docker: command not found` or permission errors

---

## 🔌 Step 2: Install Required Jenkins Plugins

Navigate to:

**Manage Jenkins → Manage Plugins**

Install:

* ✅ Docker Pipeline
* ✅ Git
* ✅ Pipeline
* ✅ GitHub Integration

🔁 Restart Jenkins after installation

---

## 🔐 Step 3: Fix Jenkins Docker Permissions (Critical)

Enter Jenkins container:

```bash
docker exec -it jenkins bash
```

Inside container:

```bash
apt-get update
apt-get install -y docker.io
usermod -aG docker jenkins
exit
```

Restart Jenkins:

```bash
docker restart jenkins
```

✔ Fixes Docker permission issues inside Jenkins

---

## 🧪 Step 4: Jenkinsfile (Pipeline Script)

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t flask-app:latest .'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f flask-container || true
                docker run -d -p 5000:5000 --name flask-container flask-app:latest
                '''
            }
        }
    }
}
```

### ✔ Best Practices Used

* SCM-based pipeline
* Clean container replacement
* Versioned Docker image
* Fully automated deployment

---

## 🧭 Step 5: Jenkins Pipeline Configuration

1. Open Jenkins UI → **New Item**
2. Select **Pipeline**
3. Under Pipeline section:

```
Definition → Pipeline script from SCM
SCM → Git
Repository URL → https://github.com/yashkanakiya/docker-jenkins-cicd
.git
Branch → */main
Script Path → Jenkinsfile
```

4. Save → **Build Now**

---

## 🌐 Step 6: Verify Application

Open browser:

```
http://localhost:5000
```

🎉 Flask application should be running successfully

---

## 🧠 Common Errors & Fixes

| Error                     | Cause                           | Solution                     |
| ------------------------- | ------------------------------- | ---------------------------- |
| docker: command not found | Docker not installed in Jenkins | Install `docker.io`          |
| permission denied         | Jenkins not in docker group     | `usermod -aG docker jenkins` |
| SCM checkout fails        | Git plugin missing              | Install Git plugin           |
| App not accessible        | Container not running           | `docker ps`                  |

---



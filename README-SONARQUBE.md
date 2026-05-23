# SonarQube Integration Guide

# Project Overview

This project demonstrates how to:

- Build a Spring Boot application
- Analyze source code quality
- Detect bugs and vulnerabilities
- Integrate SonarQube with Jenkins
- Automate quality checks inside CI/CD

---

# Architecture

```
Developer
   ↓
GitHub
   ↓
Jenkins Pipeline
   ↓
Maven Build
   ↓
SonarQube Analysis
```

---

#  Technologies Used

| Technology | Purpose |
|---|---|
| Spring Boot | Backend Framework |
| Maven | Build Tool |
| Jenkins | CI/CD Automation |
| SonarQube | Code Quality Analysis |
| Docker | Containerization |
| GitHub | Source Control |

---

#  Project Structure

```
simple-demo/
│
├── Jenkinsfile
├── pom.xml
├── README-SONARQUBE.md
│
└── src/
```

---

# What is SonarQube?

SonarQube is a static code analysis platform used to inspect source code quality.

It detects:

- Bugs
- Vulnerabilities
- Security hotspots
- Code smells
- Technical debt
- Duplicate code

---

#  Run SonarQube Using Docker

```
docker run -d \
--name sonarqube \
-p 9000:9000 \
sonarqube:lts-community
```

---

#  Access SonarQube

Open:
```
http://192.168.100.10:9000
```
---

# Default Credentials

Username:
```
admin
```
Password:
```
admin
```

---

# Configure SonarQube in Jenkins

---

# Install Plugins

Inside Jenkins install:

```
SonarQube Scanner
```

---

# Configure SonarQube Server

Navigate to:

```
Manage Jenkins
→ System
→ SonarQube Servers
```

Add:

| Field | Value |
|---|---|
| Name | sonarqube |
| Server URL | http://192.168.100.10:9000 |
| Token | Generated Sonar Token |

---

# Generate SonarQube Token

Inside SonarQube:

```text
My Account
→ Security
→ Generate Token
```

Copy the token into Jenkins.

---

# Maven Sonar Command

Run analysis manually:

```bash
mvn sonar:sonar
```

---

#  Jenkinsfile

```
pipeline {

    agent any

    tools {
        maven 'maven3'
    }

    stages {

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

    }
}
```

---

#  Run Jenkins Pipeline

The pipeline performs:

1. Build application
2. Run tests
3. Send project to SonarQube
4. Generate quality reports

---

# SonarQube Dashboard

The dashboard displays:

- Bugs
- Vulnerabilities
- Code coverage
- Duplication percentage
- Technical debt
- Quality gate status

---

# Screenshots

---

## Jenkins Sonar Stage

<img width="1345" height="823" alt="image" src="https://github.com/user-attachments/assets/5d23a163-f46e-4ab5-822f-98f4c787cd50" />

---

## Quality Gate

<img width="1438" height="815" alt="image" src="https://github.com/user-attachments/assets/86681d7e-fadd-4c02-879f-08e2ce98aa86" />

---

```text
[INFO] ANALYSIS SUCCESSFUL
[INFO] BUILD SUCCESS
```
<img width="1427" height="862" alt="image" src="https://github.com/user-attachments/assets/618a0dec-420e-473e-96d1-c97a32184d95" />

---


# 📄 License

This project is for educational and DevOps learning purposes.

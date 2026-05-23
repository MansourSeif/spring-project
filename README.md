# 🚀 Simple Spring Boot CI/CD Pipeline

A complete CI/CD pipeline project using:

- Spring Boot
- Jenkins
- Maven
- SonarQube
- Nexus Repository Manager
- Docker
- GitHub

This project demonstrates how to automate the software delivery lifecycle from source code commit to artifact deployment.

---

# 📌 Project Overview

This project showcases a complete DevOps workflow where:

1. Source code is pushed to GitHub
2. Jenkins pulls the project
3. Maven builds the application
4. Tests are executed
5. SonarQube analyzes code quality
6. Maven packages the application
7. Nexus stores the generated artifacts

---

# 🏗️ Architecture

```text
Developer
   ↓
GitHub Repository
   ↓
Jenkins Pipeline
   ├── Build
   ├── Test
   ├── SonarQube Analysis
   ├── Package
   └── Deploy to Nexus
            ↓
     Nexus Repository
```

---

# 🚀 Technologies Used

| Technology | Purpose |
|---|---|
| Java 17 | Programming Language |
| Spring Boot | Backend Framework |
| Maven | Build Automation |
| Jenkins | CI/CD Automation |
| SonarQube | Static Code Analysis |
| Nexus 3 | Artifact Repository |
| Docker | Containerization |
| GitHub | Version Control |


---

# ⚙️ CI/CD Pipeline Workflow

---

## 1️⃣ GitHub Source Code

The developer pushes code to GitHub:

```bash
git add .
git commit -m "Update project"
git push origin main
```

---

## 2️⃣ Jenkins Pulls the Repository

Jenkins automatically pulls the latest code from GitHub.

---

## 3️⃣ Build Stage

Jenkins executes:

```bash
mvn clean compile
```

This stage:

- Downloads dependencies
- Compiles source code
- Verifies project structure

---

## 4️⃣ Test Stage

Jenkins runs:

```bash
mvn test
```

This executes all JUnit tests.

---

## 5️⃣ SonarQube Analysis

Jenkins sends the project to SonarQube:

```bash
mvn sonar:sonar
```

SonarQube analyzes:

- Bugs
- Vulnerabilities
- Code smells
- Duplicated code
- Test coverage

---

## 6️⃣ Package Stage

Maven packages the application:

```bash
mvn package
```

Generated artifact:

```text
target/demo-0.0.1-SNAPSHOT.jar
```

---

## 7️⃣ Deploy to Nexus

Jenkins uploads the artifact to Nexus:

```bash
mvn deploy
```

Artifacts are stored inside:

- `maven-releases`
- `maven-snapshots`

---

# 🔍 SonarQube Integration

---

## What is SonarQube?

SonarQube is a static code analysis platform used to inspect code quality and security vulnerabilities.

It detects:

- Bugs
- Vulnerabilities
- Code smells
- Security issues
- Technical debt

---

## Run SonarQube with Docker

```bash
docker run -d \
--name sonarqube \
-p 9000:9000 \
sonarqube:lts-community
```

---

## Access SonarQube

```text
http://192.168.100.10:9000
```
---

## Default Credentials

Username:

```text
admin
```

Password:

```text
admin
```

---

## Configure SonarQube in Jenkins

Go to:

```text
Manage Jenkins → System → SonarQube Servers
```

Add:

| Field | Value |
|---|---|
| Name | sonarqube |
| Server URL | http://192.168.100.10:9000 |
| Authentication Token | Generated Token |

---

## Generate SonarQube Token

```text
My Account → Security → Generate Tokens
```
---

# 📦 Nexus Repository Integration

---

## What is Nexus?

Nexus Repository Manager is used to:

- Store Maven artifacts
- Manage dependencies
- Host private repositories
- Centralize binary storage

---

## Run Nexus with Docker

```bash
docker run -d \
--name nexus \
-p 8081:8081 \
sonatype/nexus3
```

---

## Access Nexus

```text
http://192.168.100.10:8081
```

---

## Retrieve Nexus Admin Password

```bash
docker exec -it nexus cat /nexus-data/admin.password
```

---

# 📦 Maven Configuration

---

## pom.xml

```xml
<distributionManagement>

    <repository>
        <id>nexus</id>
        <name>Nexus Releases</name>
        <url>
            http://192.168.100.10:8081/repository/maven-releases/
        </url>
    </repository>

    <snapshotRepository>
        <id>nexus</id>
        <name>Nexus Snapshots</name>
        <url>
            http://192.168.100.10:8081/repository/maven-snapshots/
        </url>
    </snapshotRepository>

</distributionManagement>
```

---

# 🔐 Maven Authentication

Credentials are stored inside:

```text
/var/jenkins_home/.m2/settings.xml
```

---

## settings.xml

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
          https://maven.apache.org/xsd/settings-1.0.0.xsd">

    <servers>

        <server>
            <id>nexus</id>
            <username>admin</username>
            <password>nexus123</password>
        </server>

    </servers>

</settings>
```
---

# 🌱 Spring Boot Application

---

## DemoApplication.java

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

---

# 📜 Jenkinsfile

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

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy Nexus') {
            steps {
                sh 'mvn deploy'
            }
        }

    }
}
```

---

# 🐳 Docker Containers Used

| Container | Port |
|---|---|
| Jenkins | 9090 |
| Nexus | 8081 |
| SonarQube | 9000 |


---

# 📸 Screenshots

---

## Jenkins Pipeline

<img width="1495" height="893" alt="image" src="https://github.com/user-attachments/assets/a092333a-4dfd-4355-8f94-6c9821ba87c7" />


---

## SonarQube Dashboard
<img width="1651" height="921" alt="image" src="https://github.com/user-attachments/assets/eb7d20e4-46e8-4f43-8c0c-5c395aed4984" />

---

## Nexus Repository

<img width="1782" height="912" alt="image" src="https://github.com/user-attachments/assets/b3c78b52-0683-4c20-9412-da370b69edee" />
<img width="1616" height="840" alt="image" src="https://github.com/user-attachments/assets/264ed7e3-a423-4d45-907b-90808ae35593" />


---

## Successful Build
<img width="1547" height="923" alt="image" src="https://github.com/user-attachments/assets/9ece33ab-1bcf-4993-ae7d-396e25f43c2b" />
<img width="1527" height="910" alt="image" src="https://github.com/user-attachments/assets/36be166a-e0fa-4f36-bcfe-3c82334688d8" />


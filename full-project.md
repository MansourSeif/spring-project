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
| Spring Boot 4 | Backend Framework |
| Maven | Build Automation |
| Jenkins | CI/CD Automation |
| SonarQube | Static Code Analysis |
| Nexus 3 | Artifact Repository |
| Docker | Containerization |
| GitHub | Version Control |

---

# 📁 Project Structure

```text
simple-demo/
│
├── Jenkinsfile
├── pom.xml
├── README.md
│
└── src
    ├── main
    │   ├── java
    │   │   └── com/example/demo
    │   │       ├── DemoApplication.java
    │   │       └── HelloController.java
    │   │
    │   └── resources
    │       └── application.properties
    │
    └── test
        └── java/com/example/demo
            └── DemoApplicationTests.java
```

---

# ⚙️ How the Pipeline Works

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

Jenkins connects to GitHub and downloads the latest source code.

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
target/simple-demo-0.0.1-SNAPSHOT.jar
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

# 🌱 Spring Boot Application

---

## DemoApplication.java

Main entry point:

```java
@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

---

## HelloController.java

Simple REST endpoint:

```java
@RestController
public class HelloController {

    @GetMapping("/")
    public String hello() {
        return "Hello Jenkins Nexus SonarQube";
    }

}
```

---

# 📦 Maven Configuration

The `pom.xml` contains:

- Dependencies
- Plugins
- Nexus deployment configuration

---

## Distribution Management

```xml
<distributionManagement>

    <repository>
        <id>nexus</id>
        <url>http://YOUR_SERVER_IP:8081/repository/maven-releases/</url>
    </repository>

    <snapshotRepository>
        <id>nexus</id>
        <url>http://YOUR_SERVER_IP:8081/repository/maven-snapshots/</url>
    </snapshotRepository>

</distributionManagement>
```

---

# 🔐 Maven Authentication

Maven credentials are stored inside:

```text
/var/jenkins_home/.m2/settings.xml
```

---

## settings.xml

```xml
<settings>

    <servers>

        <server>
            <id>nexus</id>
            <username>admin</username>
            <password>YOUR_PASSWORD</password>
        </server>

    </servers>

</settings>
```

⚠️ Important:

The `<id>` in `settings.xml` MUST match the `<id>` in `pom.xml`.

---

# 📜 Jenkinsfile

```groovy
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

# 📦 Nexus Repository Manager

---

## What is Nexus?

Nexus is an artifact repository manager used to:

- Store build artifacts
- Manage dependencies
- Host Maven repositories
- Centralize binary management

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
http://YOUR_SERVER_IP:8081
```

---

## Nexus Default Credentials

Username:

```text
admin
```

Retrieve password:

```bash
docker exec -it nexus cat /nexus-data/admin.password
```

---

# 🔍 SonarQube

---

## What is SonarQube?

SonarQube is a code quality inspection platform.

It detects:

- Bugs
- Vulnerabilities
- Code smells
- Security risks
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
http://YOUR_SERVER_IP:9000
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

# 🐳 Docker Containers Used

| Container | Port |
|---|---|
| Jenkins | 8080 |
| Nexus | 8081 |
| SonarQube | 9000 |

---

# ▶️ Running the Application Locally

---

## Clone Repository

```bash
git clone https://github.com/YOUR_USERNAME/simple-demo.git
```

---

## Navigate to Project

```bash
cd simple-demo
```

---

## Build Project

```bash
mvn clean install
```

---

## Run Application

```bash
mvn spring-boot:run
```

Application URL:

```text
http://localhost:8080
```

---

# 🌐 API Endpoint

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Returns welcome message |

Example response:

```text
Hello Jenkins Nexus SonarQube
```

---

# 📸 Screenshots

---

## Jenkins Pipeline

> Add screenshot here

```text
docs/images/jenkins-pipeline.png
```

---

## SonarQube Dashboard

> Add screenshot here

```text
docs/images/sonarqube-dashboard.png
```

---

## Nexus Repository

> Add screenshot here

```text
docs/images/nexus-dashboard.png
```

---

## Successful Build

> Add screenshot here

```text
docs/images/build-success.png
```

---

# 🛠️ Common Issues

---

## 401 Unauthorized

Cause:

- Wrong Nexus credentials
- Mismatched repository IDs

Fix:

Ensure:

```xml
<id>nexus</id>
```

matches in:

- `pom.xml`
- `settings.xml`

---

## SonarQube Connection Failed

Verify:

```bash
docker ps
```

Make sure SonarQube container is running.

---

## Nexus Not Reachable

Verify Nexus container:

```bash
docker ps
```

---

# 📚 Learning Objectives

This project demonstrates:

- CI/CD pipeline creation
- Jenkins automation
- Maven builds
- SonarQube integration
- Nexus deployment
- Spring Boot packaging
- Docker container usage

---

# 🔮 Future Improvements

Possible future enhancements:

- Dockerize Spring Boot app
- Kubernetes deployment
- GitHub Webhooks
- Slack notifications
- Prometheus monitoring
- Terraform infrastructure
- Automated production deployment

---

# ✅ Final Result

At the end of the pipeline:

✔ Source code validated  
✔ Tests executed  
✔ Code quality analyzed  
✔ JAR packaged  
✔ Artifact deployed to Nexus  

---

# 👨‍💻 Author

Your Name Here

GitHub:

```text
https://github.com/YOUR_USERNAME
```

---

# 📄 License

This project is for educational and DevOps learning purposes.

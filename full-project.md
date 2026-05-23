#  Nexus Repository Manager Integration

#  Project Overview

This project demonstrates how to:

- Build a Spring Boot application
- Package it using Maven
- Deploy artifacts automatically to Nexus
- Store SNAPSHOT and RELEASE builds
- Integrate Nexus with Jenkins CI/CD

---

#  Architecture

```
Developer
   ↓
GitHub
   ↓
Jenkins Pipeline
   ↓
Maven Build
   ↓
Nexus Repository Manager
```

---

#  Technologies Used

| Technology | Purpose |
|---|---|
| Spring Boot | Backend Framework |
| Maven | Build Tool |
| Jenkins | CI/CD Automation |
| Nexus 3 | Artifact Repository |
| Docker | Containerization |
| GitHub | Source Control |

---

#  Project Structure

```
simple-demo/
│
├── Jenkinsfile
├── pom.xml
├── README-NEXUS.md
│
└── src/
```

---

#  What is Nexus?

Nexus Repository Manager is a binary repository manager used to:

- Store Maven artifacts
- Manage dependencies
- Host internal repositories
- Centralize build outputs
- Share artifacts across teams

---

#  Run Nexus Using Docker

```
docker run -d \
--name nexus \
-p 8081:8081 \
sonatype/nexus3
```

---

#  Access Nexus

Open:

```text
http://192.168.100.10:8081
```

---

#  Nexus Default Credentials

Username:

```text
admin
```

Retrieve admin password:

```bash
docker exec -it nexus cat /nexus-data/admin.password
```

---

#  Create Maven Repositories

Inside Nexus:

## Create:

### Maven Releases

```text
maven-releases
```

### Maven Snapshots

```
maven-snapshots
```

---

# Maven Configuration

---

# pom.xml

```
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

#  Maven Authentication

Maven credentials are stored inside:

```
/var/jenkins_home/.m2/settings.xml
```

---

# settings.xml

```
<settings>

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

# 🚀 Deploy Artifact to Nexus

Run:

```
mvn clean deploy
```

---

#  Artifact Location

Artifacts will appear inside:

```text
com/example/demo/
```
---

#  Screenshots

---

## Nexus Dashboard

<img width="1402" height="840" alt="image" src="https://github.com/user-attachments/assets/5c252ab2-7f0c-4da4-990b-122b0f4cc390" />

---

## Uploaded Artifact

<img width="1376" height="798" alt="image" src="https://github.com/user-attachments/assets/1eed663e-717a-4ef2-9a55-50ba0a2e4ad1" />

---


# 🧩 Task 2: Jenkins Remoting Project (CI Pipeline)
📌 Project Overview

This project focuses on implementing Jenkins Remoting to enable distributed and secure build execution. The goal is to demonstrate how Jenkins can run jobs on remote agents (nodes), improving scalability, performance, and security in CI/CD pipelines.

The project integrates Azure DevOps Repositories with Jenkins, allowing Jenkins to pull code, execute pipelines remotely, and manage builds efficiently.

## 🔹 Architecture Flow

```
Azure DevOps Repo → Jenkins → Build/Test → (Optional) Deploy
```

Jenkins Responsibilities:

* Pull code from Azure DevOps repository
* Trigger builds automatically
* Execute pipelines using **Jenkinsfile**

---

## 🔹 Prerequisites

Ensure the following are available:

1. Jenkins installed (Java 17 compatible)
2. Azure DevOps project
3. Azure Repos (Git)
4. Admin access to Jenkins
5. Azure DevOps **Personal Access Token (PAT)**

---

## 🔹 Step 1: Install Required Jenkins Plugins

### 📌 Why?

Jenkins requires plugins to authenticate and communicate with Azure DevOps.

### ✅ Required Plugins

* Azure DevOps Plugin
* Git Plugin
* Pipeline Plugin
* Credentials Binding Plugin

### 🔧 Installation Steps

1. Jenkins Dashboard → Manage Jenkins
2. Plugins → Available Plugins
3. Search and install:

   * Azure DevOps
   * Git
   * Pipeline
4. Restart Jenkins

---

## 🔹 Step 2: Create Azure DevOps Personal Access Token (PAT)

### 📌 Why?

PAT is used by Jenkins to securely access Azure Repos.

### 🔧 Steps to Create PAT

1. Open Azure DevOps
2. User Settings → Personal Access Tokens
3. Click **New Token**
4. Configure:

   * Name: `Jenkins-Integration`
   * Organization: Your organization
   * Scope: Code → Read (or Read & Write)
5. Click **Create**
6. Copy the token (shown only once)

---
## 🔹 Step 3: Add Azure DevOps Credentials in Jenkins

### 📌 Why?

Secure authentication between Jenkins and Azure DevOps.

### 🔧 Steps

1. Manage Jenkins → Credentials
2. Select **Global** → Add Credentials
3. Configure:

   * Kind: Username with password
   * Username: Azure DevOps email
   * Password: PAT token
   * ID: `azure-devops-creds`
4. Save

---


## 🔹 Step 4: Get Azure Repo Git URL

1. Azure DevOps → Repos
2. Click **Clone**
3. Copy HTTPS URL

Example:

```
https://dev.azure.com/organization/project/_git/repository
```

---

## 🔹 Step 5: Create Jenkins Pipeline Job

### 🔧 Steps

1. Jenkins Dashboard → New Item
2. Job Name: `AzureDevOps-CI`
3. Select **Pipeline**
4. Click OK

---


## 🔹 Step 6: Configure Source Code Management

### 📌 Why?

To connect Jenkins with Azure Repos.

### 🔧 Configuration

* Pipeline Definition: **Pipeline script from SCM**
* SCM: Git
* Repository URL: Azure Repo HTTPS URL
* Credentials: `azure-devops-creds`
* Branch: `*/main`
* Script Path: `Jenkinsfile`

---


## 🔹 Step 7: Jenkinsfile (Pipeline Script)

### 📌 Purpose

Defines CI/CD pipeline stages.

```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'azure-devops-creds',
                    url: 'https://dev.azure.com/org/project/_git/repo'
            }
        }
        }
    }

```

Commit and push this file to Azure Repo.

---


## 🔹 Step 8: Enable Auto Trigger (Optional)

### 📌 Why?

Automatically trigger Jenkins build on every code push.

### 🔧 Jenkins Side

1. Job → Configure
2. Build Triggers → Enable **Poll SCM**
3. Schedule:

```
H/5 * * * *
```

### 🔧 Azure DevOps Side (Webhooks)

1. Project Settings → Service Hooks
2. Add Web Hook
3. Trigger: Code Push
4. URL:

```
http://<jenkins-url>/azure-webhook/
```

---

## 🔹 Step 9: Run & Verify

1. Click **Build Now** in Jenkins
2. Check **Console Output**
3. Verify:

   * Repository cloned successfully
   * All pipeline stages executed
   * No authentication errors

---

## ✅ Conclusion

This project demonstrates a complete **DevOps CI/CD workflow** using:

* Azure Blazor Web App
* Azure App Service
* Azure DevOps Repos
* Jenkins Pipeline Automation

---

Author: Sameen

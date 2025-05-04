Here's a basic example of a `azure-pipelines.yml` file for a Dockerized JavaScript (Node.js) application. This pipeline will:

1. Install Node.js dependencies.
2. Build the application.
3. Build and push a Docker image to Azure Container Registry (ACR).
4. Optionally deploy to Kubernetes or Azure App Service (mention if needed).

---

### ✅ Project Structure (example)

```
/my-app
├── Dockerfile
├── azure-pipelines.yml
├── package.json
└── index.js
```

---

### 📝 `azure-pipelines.yml`

```yaml
trigger:
  branches:
    include:
      - main

variables:
  imageName: 'nodejs-app'

stages:
- stage: Build
  displayName: 'Build Stage'
  jobs:
  - job: Build
    displayName: 'Build and Push Docker Image'
    pool:
      vmImage: 'ubuntu-latest'
    steps:
    - task: NodeTool@0
      inputs:
        versionSpec: '18.x'
      displayName: 'Install Node.js'

    - script: |
        npm install
        npm run build || echo "No build step defined"
      displayName: 'Install dependencies and build app'

    - task: Docker@2
      displayName: 'Build and Push Docker Image'
      inputs:
        containerRegistry: 'MyACRConnection'   # 🔑 Service connection to ACR
        repository: 'nodejs-app'
        command: 'buildAndPush'
        Dockerfile: '**/Dockerfile'
        tags: |
          $(Build.BuildId)
          latest
```

---

### 🐳 Sample `Dockerfile`

```dockerfile
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["node", "index.js"]
```

---

### 🔐 Prerequisites

1. **Azure Container Registry (ACR)** created.
2. **Service connection** `MyACRConnection` to ACR in Azure DevOps → Project Settings → Service Connections.
3. Repo hosted in Azure Repos or GitHub (connected to Azure DevOps).

---

Would you like me to add **deployment to AKS** or **App Service** in the next stage?

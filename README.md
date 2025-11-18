# 🚀 Deploying *chidiya_ud_backend* to Azure App Service

This guide explains how to host the backend on **Microsoft Azure App Service**.

---

## 📦 1. Requirements

- Azure account  
- Azure CLI (optional but recommended)  
- Node.js  
- Git  

---

## ⚙️ 2. Local Setup

Clone the project:

```bash
git clone https://github.com/NoxiousTab/chidiya_ud_backend.git
cd chidiya_ud_backend
```

Install dependencies:

```bash
npm install
```

Run locally:

```bash
npm start
```

---

# ☁️ Deploying to Azure App Service

Two methods:

---

# **Method 1: Deploy Automatically from GitHub (Recommended)**

### Step 1: Create Azure App Service

1. Go to Azure portal → App Services  
2. Click **Create**  
3. Choose:  
   - **Runtime stack:** Node.js  
   - **Publish:** Code  
   - **OS:** Linux  
4. Create

---

### Step 2: Connect GitHub Repo

1. App Service → Deployment Center  
2. Select GitHub  
3. Choose repo: `chidiya_ud_backend`  
4. Select branch: `main`  
5. Save

Azure will automatically deploy on each push.

---

# **Method 2: Deploy Using Azure CLI**

Login:

```bash
az login
```

Create resource group:

```bash
az group create --name chidiya-ud-rg --location "Central India"
```

Create App Service plan:

```bash
az appservice plan create \
  --name chidiya-ud-plan \
  --resource-group chidiya-ud-rg \
  --sku B1 \
  --is-linux
```

Create the web app:

```bash
az webapp create \
  --resource-group chidiya-ud-rg \
  --plan chidiya-ud-plan \
  --name chidiya-ud-backend \
  --runtime "NODE|18-lts"
```

Deploy:

```bash
az webapp up --name chidiya-ud-backend --resource-group chidiya-ud-rg
```

---

# 🔧 Environment Variables

Go to:

`Azure Portal → App Service → Configuration → Application Settings`

Add vars like:

```
PORT = 8080
MONGO_URI = your_connection_string
JWT_SECRET = your_secret
```

Restart the app.

---

# 🌍 Accessing API

Your API will be available at:

```
https://<your-app-name>.azurewebsites.net/
```

---

# 📄 Logs

```bash
az webapp log tail --name chidiya-ud-backend --resource-group chidiya-ud-rg
```

---

# ✅ Done

Backend successfully deployed to Azure App Service.

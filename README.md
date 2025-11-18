Hosting this repository on Azure is straightforward because it is a Node.js/TypeScript application, which is natively supported by **Azure App Service**.

Based on the project structure (`package.json`, `tsconfig.json`, and `src` folder), the best way to host this is using **Azure App Service (Linux)**.

Here are the two best methods to deploy it.

### **Method 1: Deploy via Azure Portal (Easiest)**

This method connects your GitHub repository directly to Azure, so any change you push to the `main` branch automatically updates the live site.

1.  **Create the App Service:**

      * Log in to the [Azure Portal](https://www.google.com/search?q=https://portal.azure.com).
      * Search for **"App Services"** and click **+ Create**.
      * **Resource Group:** Create a new one (e.g., `chidiya-backend-rg`).
      * **Name:** Give your app a unique name (e.g., `chidiya-api`).
      * **Publish:** Select **Code**.
      * **Runtime stack:** Select **Node 18 LTS** (or the version matching your local development).
      * **Operating System:** Select **Linux**.
      * Click **Review + create** -\> **Create**.

2.  **Connect GitHub Repository:**

      * Once the resource is created, go to the resource page.
      * In the left sidebar, look for **Deployment Center**.
      * Under **Source**, select **GitHub**.
      * Authorize Azure to access your GitHub account.
      * **Organization:** Select your username (`NoxiousTab` or your fork).
      * **Repository:** Select `chidiya_ud_backend`.
      * **Branch:** Select `main`.
      * Click **Save**.

3.  **Configure Build Command (Important):**

      * Since this is a TypeScript project, Azure needs to know how to build it.
      * Go to **Settings** -\> **Configuration** -\> **General Settings**.
      * In the **Startup Command** box, enter:
        ```bash
        npm run build && npm start
        ```
        *(Note: Check your `package.json`. If `npm start` already runs the build, you might just need `npm start`. Typically for TS, you need to compile first.)*

### **Method 2: Deploy via Azure CLI (Command Line)**

If you have the Azure CLI installed, you can do this faster from your terminal.

1.  **Login to Azure:**
    ```bash
    az login
    ```
2.  **Create the Resource Group & App Service:**
    ```bash
    # Create a resource group
    az group create --name chidiya-backend-rg --location "East US"

    # Create an App Service Plan (Free tier F1 for testing, or B1 for production)
    az appservice plan create --name chidiya-plan --resource-group chidiya-backend-rg --sku F1 --is-linux

    # Create the Web App
    az webapp create --resource-group chidiya-backend-rg --plan chidiya-plan --name <UNIQUE_APP_NAME> --runtime "NODE|18-lts"
    ```
3.  **Deploy the code:**
    Run this command from inside the project folder:
    ```bash
    az webapp up --name <UNIQUE_APP_NAME> --resource-group chidiya-backend-rg
    ```

### **Crucial Step: Environment Variables**

Your application likely fails to start without the database connection. The repository README mentions specific variables you must set.

1.  Go to your App Service in the Azure Portal.
2.  On the left menu, select **Settings** \> **Environment variables** (or "Configuration" in older views).
3.  Click **Add** (or "New application setting") and add the keys found in your `.env` file:
      * `PORT`: `8080` (Azure typically expects apps to listen on port 8080).
      * `MONGO_URI`: Your MongoDB connection string (e.g., from MongoDB Atlas).
      * `JWT_SECRET`: Your secure secret key.
4.  Click **Apply** and **Save**. The app will restart automatically.

### **How to Verify**

  * Once deployed, look for the **Default Domain** on the App Service Overview page (e.g., `https://chidiya-api.azurewebsites.net`).
  * Open that URL in your browser. If you see your API response or a "Welcome" message, it is working.
  * **Troubleshooting:** If you see an "Application Error," go to **Monitoring** -\> **Log Stream** on the left sidebar to see the live server logs (often shows database connection errors).

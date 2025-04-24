# Hello Cloud & DevOps 🚀

Welcome to the hands-on lab for Cloud and DevOps using GitHub and Azure! In this workshop, you will learn how to:
- Create your own GitHub and Azure Student accounts.
- Deploy a Python "Hello World" web app to Azure App Service.
- Set up GitHub Actions to automate deployment.
- Modify the web app to use Azure Storage for static files.

---

## 📝 Assignment Steps

### 1. Navigate to this Repository
You are already here! This is the starter repo containing a simple Python web app using Flask.

### 2. Create a GitHub Account
If you don’t already have one, create a free GitHub account:
👉 https://github.com/join

> ✅ If you are a student, you can get a free GitHub Pro account with additional features:
👉 https://education.github.com/pack

**‼️Note**: This will be your personal GitHub account. If you already have a GitHub account, you can use it for this lab.

### 3. Create an Azure Free Student Account
Register for a free Azure account with your student email:
👉 https://azure.microsoft.com/en-us/free/students

> ✅ With the Azure for Students offer, you'll get $100 in free credits and access to popular Azure services.

### 4. Fork this Repository
Click the **Fork** button at the top-right of this page to make your own copy of this repository under your GitHub account.

### 5. Create an Azure App Service
1. Log into the [Azure Portal](https://portal.azure.com).
2. Search for **App Services** and click **Create**.
3. Fill in the details:
   - **Resource Group**: Create a new one (e.g., `devops-lab`)
   - **Name**: Your unique app name (e.g., `hello-cloud-devops-<your-github-username>`)
   - **Publish**: Code
   - **Runtime stack**: Python 3.13 (or latest available)
   - **Region**: Select a region close to you (e.g., `West Europe`)
   - **Pricing plan**: Create a new plan
     - **Name**: Your unique plan name (e.g., `hello-cloud-devops-plan`)
     - **Size**: Basic (B1)
4. Explore the rest of the options and leave them as default.
5. Click **Review + Create** and then **Create**.

> ✅ Wait for the deployment to complete. This may take a few minutes.
> [Optionally] Learn more about [Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/overview).

### 6. Configure Deployment with GitHub Actions
1. Go to your forked repository on GitHub.
2. Click on the **Actions** tab.
3. Set up a new workflow by selecting **Set up a workflow yourself**.
4. Rename the file to `deploy-to-azure.yml` (or any name you prefer).
5. Add the following code to the file:

```yaml
name: Deploy to Azure App Service

on:
  push:
    branches:
      - main # Trigger deployment when commits are pushed to the main branch

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest # Use the latest Ubuntu runner to run the job

    steps:
    - name: Checkout code
      uses: actions/checkout@v3 # Check out the code from the repository

    - name: Set up Python
      uses: actions/setup-python@v4 # Set up Python environment on the runner
      with:
        python-version: '3.13'

    - name: Install dependencies
      run: pip install -r requirements.txt # Install the required Python packages

    - name: Deploy to Azure Web App
      uses: azure/webapps-deploy@v2 # Deploy the app to Azure Web App
      with:
        app-name: YOUR-APP-NAME-HERE # Replace with your Azure App Service name
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }} # Use the publish profile secret for authentication
        package: . # Specify the path to the package to deploy (current directory)
```

6. Go to your Azure App Service in the Azure Portal.
   - Click on **Deployment Center** in the left menu.
   - Select **GitHub** as the source and authorize Azure to access your GitHub account.
   - Select your forked repository and the `main` branch.
   - Azure will generate a **Publish Profile** for you. Go to the **Overview** tab and click on **Get publish profile**. This will download a `.publishsettings` file.
7. In your GitHub repository:
   - Go to **Settings** > **Secrets and variables** > **Actions**.
   - Click on **New repository secret**.
   - Name it `AZURE_WEBAPP_PUBLISH_PROFILE` and paste the content of the `.publishsettings` file you downloaded from Azure.

### Test your Deployment
After pushing changes to the `main` branch, GitHub Actions will automatically deploy your app to Azure!

Open your Azure App Service URL in a web browser to see your deployed app.
You can find the URL in the **Overview** tab of your App Service.
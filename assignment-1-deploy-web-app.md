# Assignment 1: Deploy a Python Web App to Azure ☁️🚀

In this assignment, you'll deploy a Python Flask application to Azure App Service and automate deployment using GitHub Actions.

---

## ✅ Step 1: Create an Azure App Service

An Azure App Service is a fully managed platform (PaaS) for building, deploying, and scaling web apps. It supports multiple programming languages, including Python!

1. In the Azure portal, search for **App Services** and click **Create** (Web App).
2. Fill in the details:
   - **Resource Group**: Create a new one (e.g., `rg-cloud-devops-lab`)
   - **Name**: Your unique app name (e.g., `app-hello-cloud-devops-<your-github-username>`)
   - **Publish**: Code
   - **Runtime stack**: Python 3.13 (or latest available)
   - **Region**: Select a region close to you (e.g., `West Europe`)
   - **Pricing plan**: Create a new plan
     - **Name**: Your unique plan name (e.g., `asp-hello-cloud-devops`)
     - **Size**: Free F1 (1 GB of storage and 60 minutes of compute time per day)
3. Explore the rest of the options and leave them as default.
4. Click **Review + Create** and then **Create**.

    > ⌛ Wait for the deployment to complete. This may take a few minutes.
    > [Optionally] Learn more about [Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/overview).

5. After the deployment is complete, go to the **Overview** tab of your App Service and click on the **Default domain** link to open your web app in a new tab. You should see a default Azure App Service page.
6. In the Azure portal, navigate to your App Service and click on **Environment variables** in the left menu.
7. Add the following environment variables:
   - `SCM_DO_BUILD_DURING_DEPLOYMENT` with value `true` (this will ensure that the app is built during deployment).
   - Click on **Apply** to save the changes.

## ✅ Step 2: Configure GitHub Actions for Deployment

GitHub Actions is a CI/CD tool that allows you to automate your software development workflows. In this case, we'll use it to deploy our web app to Azure.

1. Go to your forked repository on GitHub.
2. Click on the **Actions** tab and then click on **New workflow**.
3. Search for **Deploy a Python app to an Azure Web App** and select **Configure**.
   - This will create a new workflow file in the `.github/workflows` directory of your repository.
   - The workflow file is named `azure-webapps-python.yml` and contains the necessary steps to deploy your app to Azure.
4. Replace the `AZURE_WEBAPP_NAME` and `PYTHON_VERSION` variables in the workflow file with your app name and Python version (e.g., `3.13`).
5. Click on **Commit changes...** to save the workflow file.
6. Get your Azure **Publish Profile**:
   - In the Azure portal, go to your App Service.
   - Click on **Deployment Center** in the left menu.
   - Select **GitHub** as the source and **Authorize** Azure to access your GitHub account.
   - Select your forked repository and the `main` branch.
   - Select "Use available workflow: Use one of the workflow files available in the selected repository and branch" as the **Workflow Option**.
   - Click on **Save**.
   - Go to **Configuration** and enable **SCM Basic Auth Publishing Credentials**.
   - Azure will generate a **Publish Profile** for you. Go to the **Overview** tab and click on **Get publish profile**. This will download a `.publishsettings` file.
7. In your GitHub repository:
   - Go to **Settings** > **Secrets and variables** > **Actions**.
   - Click on **New repository secret**.
   - Name it `AZURE_WEBAPP_PUBLISH_PROFILE` and paste the content of the `.publishsettings` file you downloaded from Azure.

## ✅ Step 3: Push and Deploy

Push a change to your `main` branch (e.g., update the `README.md` file) and watch GitHub Actions deploy your app automatically (**Actions** tab).

## ✅ Step 4: Test Your Deployment

After pushing changes to the `main` branch, GitHub Actions will automatically deploy your app to Azure!

Open your Azure App Service URL in a web browser to see your deployed app.
You can find the URL in the **Overview** tab of your App Service.

You’re now ready to proceed to [Assignment 2: Use Azure Storage in Your Web App](assignment-2-use-azure-storage.md) 🎯
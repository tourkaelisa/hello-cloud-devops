# Assignment 1: Deploy a Python Web App to Azure ☁️🚀

In this assignment, you'll deploy a Python Flask application to Azure App Service and automate deployment using GitHub Actions.

---

## ✅ Step 1: Create an Azure App Service

An Azure App Service is a fully managed platform (PaaS) for building, deploying, and scaling web apps. It supports multiple programming languages, including Python!

1. In the Azure portal, search for **App Services** and click **Create**.
2. Fill in the details:
   - **Resource Group**: Create a new one (e.g., `rg-cloud-devops-lab`)
   - **Name**: Your unique app name (e.g., `app-hello-cloud-devops-<your-github-username>`)
   - **Publish**: Code
   - **Runtime stack**: Python 3.13 (or latest available)
   - **Region**: Select a region close to you (e.g., `West Europe`)
   - **Pricing plan**: Create a new plan
     - **Name**: Your unique plan name (e.g., `asp-hello-cloud-devops`)
     - **Size**: Basic (B1)
3. Explore the rest of the options and leave them as default.
4. Click **Review + Create** and then **Create**.

    > ⌛ Wait for the deployment to complete. This may take a few minutes.
    > [Optionally] Learn more about [Azure App Service](https://learn.microsoft.com/en-us/azure/app-service/overview).

## ✅ Step 2: Configure GitHub Actions for Deployment

GitHub Actions is a CI/CD tool that allows you to automate your software development workflows. In this case, we'll use it to deploy our web app to Azure.

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
6. Click on **Commit changes...** to save the workflow file.
7. Get your Azure **Publish Profile**:
   - In the Azure portal, go to your App Service.
   - Click on **Deployment Center** in the left menu.
   - Select **GitHub** as the source and authorize Azure to access your GitHub account.
   - Select your forked repository and the `main` branch.
   - Azure will generate a **Publish Profile** for you.
   - Go to the **Overview** tab and click on **Get publish profile**. This will download a `.publishsettings` file.
8. In your GitHub repository:
   - Go to **Settings** > **Secrets and variables** > **Actions**.
   - Click on **New repository secret**.
   - Name it `AZURE_WEBAPP_PUBLISH_PROFILE` and paste the content of the `.publishsettings` file you downloaded from Azure.

## ✅ Step 3: Push and Deploy

Push a change to your `main` branch (e.g., update the `README.md` file) and watch GitHub Actions deploy your app automatically.

## ✅ Step 4: Test Your Deployment

After pushing changes to the `main` branch, GitHub Actions will automatically deploy your app to Azure!

Open your Azure App Service URL in a web browser to see your deployed app.
You can find the URL in the **Overview** tab of your App Service.

You’re now ready to proceed to [Assignment 2: Use Azure Storage in Your Web App](assignment-2-use-azure-storage.md) 🎯
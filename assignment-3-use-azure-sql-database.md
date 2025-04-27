# Assignment 3: Use Azure SQL Database in Your Web App 🗄️🌐

In this assignment, you'll enhance your Python web app by using Azure SQL Database to store and retrieve data.

## ✅ Step 1: Create an Azure SQL Database

Azure SQL Database is a fully managed relational database service in the Microsoft cloud. It provides high availability, scalability, and security.

1. In the Azure portal, search for **SQL databases** and click **Create**.
2. Use the same resource group as before.
3. Choose a unique name for your database (e.g., `sqldb-hello-cloud-devops`).
4. On the **Server** section, click on **Create new**.
5. Name your server (e.g., `sql-hello-cloud-devops-<your-username>`).
6. Select the **Location** where your app service is located (e.g., `West Europe`).
7. Set **Authentication method** to `SQL authentication`.
8. Set **Admin login** to `sqladmin` and create a strong password (e.g., `StrongPassword123!`) and click **OK**.
9. On the **Backup storage redundancy** section, select `Locally redundant backup storage`.
10. [Optional] Explore the rest of the options and leave them as default.
11. Click **Review + Create** and then **Create**.

    > ⌛ Wait for the deployment to complete. This may take a few minutes.
    > [Optionally] Learn more about [Azure SQL Database](https://learn.microsoft.com/en-us/azure/azure-sql/database/sql-database-paas-overview?view=azuresql).

## ✅ Step 2: Configure Firewall Rules

In order the web app to access the SQL database, you need to configure the firewall rules.

1. In the Azure portal, navigate to your SQL server.
2. Click on **Networking** from the left menu.
3. Under **Firewall rules**, click on **Add your client IPv4 address** to allow access from your current IP address.
   - This is useful for development and testing purposes to allow access from your local machine.
   - For production, you may want to set up more secure access rules.
4. Under **Public network access**, select **Allow Azure services and resources to access this server**.
    - For demo purposes, this will allow your web app to access the SQL database (through the public internet).
    - In real-world scenarios, this connectivity is configured using private endpoints or virtual networks.
5. Click **Save** to apply the changes.

## ✅ Step 3: Create a Table in the SQL Database

Let's create a simple table to store some data. For this step, we will the query editor in the Azure portal to execute SQL commands. In a real-world scenario, you would typically use a database management tool like SQL Server Management Studio (SSMS) or Azure Data Studio.

1. In the Azure portal, navigate to your SQL database.
2. Click on **Query editor (preview)** from the left menu.
3. Connect to the database using the admin login and password you created earlier.
4. Run the following SQL command to create a simple table:

    ```sql
    CREATE TABLE Products (
        Id INT PRIMARY KEY IDENTITY(1,1),
        Name NVARCHAR(100) NOT NULL
    );
    ```
5. Insert some sample data into the table:

    ```sql
    INSERT INTO Products (Name) VALUES ('Milk'), ('Bread'), ('Eggs');
    ```

## ✅ Step 4: Modify Flask App to Use SQL Database

Now let's update your Flask web app to connect to the Azure SQL Database and display the products you inserted.

### 4.1 Install Required Python Package

First, you need a Python package to connect to SQL Server databases.  
We will use `pyodbc` along with the `pymssql` or `pytds` drivers. However, for simplicity and compatibility on Azure App Service, we'll use **`pyodbc`**.

In your `requirements.txt`, add the following line:

```plaintext
pyodbc==5.2.0
```

### 4.2 Update Your Application Code

Open your `app.py` and update it with the following:

```python
from flask import Flask, render_template
import pyodbc

app = Flask(__name__)

# Database connection settings (replace with your actual details)
server = '<your-server-name>.database.windows.net'
database = '<your-database-name>'
username = 'sqladmin'
password = '<your-password>'
driver = '{ODBC Driver 18 for SQL Server}'

def get_db_connection():
    connection_string = f'DRIVER={driver};SERVER={server};DATABASE={database};UID={username};PWD={password};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;'
    conn = pyodbc.connect(connection_string)
    return conn

@app.route('/')
def home():
    conn = get_db_connection()
    cursor = conn.cursor()
    cursor.execute('SELECT Name FROM Products')
    products = [row[0] for row in cursor.fetchall()]
    conn.close()
    
    return render_template('index.html', products=products)

@app.route('/about')
def about():
    return render_template('about.html')

@app.route('/devops')
def devops():
    return render_template('devops.html')

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)

```

‼️ **Important**: Committing sensitive information (like database credentials) to a public repository is a security risk. In a real-world application, you should use environment variables or Azure Key Vault to manage sensitive information securely. Likely for this assignment, your database is not publicly accessible, but it's a good practice to follow. 🔐

🏠 **Homework**: Try using environment variables to store your database credentials instead of hardcoding them in your code. You can use the `os` module in Python to access environment variables.

### 4.3 Update the HTML Template

Modify your `templates/index.html` to display the list of products by adding the following code within the `<main>` tag:

```html
<h2>Products List</h2>
<ul>
    {% for product in products %}
    <li>{{ product }}</li>
    {% else %}
    <li>No products found.</li>
    {% endfor %}
</ul>
```

This will create a simple unordered list of products fetched from the SQL database. Notice that we are using Jinja2 templating syntax to loop through the `products` list passed from the Flask app.

### 4.4 Deploy Your Changes

Commit your changes and push them to your GitHub repository. The GitHub Actions workflow will automatically deploy the updated app to Azure.

If you are editing the code from GitHub, you edit and commit the changes separately for each file (causing multiple deployments).

If you are editing the code in your local environment, you can commit and push all of your changes to your forked repository by running:

```bash
git add .
git commit -m "Add Azure SQL Database connection and products list"
git push
```

Once the deployment is complete, navigate to your web app and check if the list of products is displayed correctly.

[Optional] Try to add more products to the database using the SQL query editor in the Azure portal and refresh your web app to see the changes.

Congratulations! You have successfully integrated Azure SQL Database into your web app. Now you can store and retrieve data dynamically. 🏆🏆

🏠 **Homework**: Try adding a form to insert new products from the web app!

## ⚠️ Important: Clean Up Resources

After completing the assignments, remember to clean up your Azure resources to avoid unnecessary charges upon your free credits. You can delete the resource group you created for this assignment, which will remove all resources within it.

1. In the Azure portal, navigate to **Resource groups**.
2. Select the resource group you created for this assignment (e.g., `rg-hello-cloud-devops`).
3. Click on **Delete resource group**.
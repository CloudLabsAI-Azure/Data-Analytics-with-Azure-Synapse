# Lab 02: Query files using a serverless SQL pool

## Lab Scenario

SQL is probably the most used language for working with data in the world. Most data analysts are proficient in using SQL queries to retrieve, filter, and aggregate data - most commonly in relational databases. As organizations increasingly take advantage of scalable file storage to create data lakes, SQL is often still the preferred choice for querying the data. Azure Synapse Analytics provides serverless SQL pools that enable you to decouple the SQL query engine from the data storage and run queries against data files in common file formats such as delimited text and Parquet. In this lab, you will understand the technical tasks to design and implement data storage; develop data processing; and secure, monitor, and optimize data storage and data processing.

### Objectives
  
After completing this lab, you will be able to:

+ Task 1: Provision an Azure Synapse Analytics workspace
+ Task 2: Query the data in files.
+ Task 3: Access external data in a database.
+ Task 4: Visualize the query results.

## Task 1: Provision an Azure Synapse Analytics workspace

You'll need an Azure Synapse Analytics workspace with access to data lake storage. You can use the built-in serverless SQL pool to query files in the data lake.

In this task, you'll use a combination of a PowerShell script and an ARM template to provision an Azure Synapse Analytics workspace.

1. Use the **[\>_]** button to the right of the search bar at the top of the page to create a new Cloud Shell in the **Azure portal**.

    ![Azure portal with a cloud shell pane](./images/DA-image1.png)

1. The first time you open the Cloud Shell, you may be prompted to choose the type of shell you want to use (Bash or PowerShell). If so, select PowerShell.

    ![Azure portal with a cloud shell pane](./images/DA-image2.png)

    > **Note**: If you have previously created a cloud shell that uses a *Bash* environment, use the the drop-down menu at the top left of the cloud shell pane to change it to ***PowerShell***.

1. On Getting started window choose **Mount storage account(1)** then under Storage account subscription select your available **subscription (2)** from the dropdown and click on **Apply (3)**.

   ![Azure portal with a cloud shell pane](./images/DA-image3.png)

1. Within the Mount storage account pane, select **we will create a storage account for you (1)** and click **Next (2)**.

    ![Azure portal with a cloud shell pane](./images/DA-image4.png)

1. Note that you can resize the cloud shell by dragging the separator bar at the top of the pane, or by using the **&#8212;**, **&#9723;**, and **X** icons at the top right of the pane to minimize, maximize, and close the pane. For more information about using the Azure Cloud Shell, see the [Azure Cloud Shell documentation](https://docs.microsoft.com/azure/cloud-shell/overview).

1. In the PowerShell pane, manually enter the following commands to clone this repo:

    ```
    rm -r synapse -f
    git clone https://github.com/CloudLabsAI-Azure/Data-Analytics-with-Azure-Synapse synapse
    ```

1. After the repo has been cloned, enter the following commands to change to the folder for this lab and run the **setup.ps1** script it contains:

    ```
    cd synapse/Allfiles/labs/00
    ./setup.ps1
    ```

1. If prompted, provided resource group already exists. Are you sure want to update it. Enter **Y** and press enter.

1. If prompted, choose which subscription you want to use (this will only happen if you have access to multiple Azure subscriptions).

1. When prompted, enter a suitable password to be set for your Azure Synapse SQL pool.

    > **Note**: Be sure to remember this password!

1. Wait for the script to complete - this typically takes around 10 minutes, but in some cases may take longer. While you are waiting, review the [Serverless SQL pool in Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) article in the Azure Synapse Analytics documentation.

## Task 2: Query data in files

The script provisions an Azure Synapse Analytics workspace and an Azure Storage account to host the data lake, then uploads some data files to the data lake.

In this task, you will  be working with Synapse Studio where you will query the data for diffrent files.

### Task 2.1: View files in the data lake

1. After the script has completed, in the Azure portal, go to the **synapse** resource group that it created, and select your Synapse workspace.

   ![](./images/labimg1.png) 

   ![](./images/synapse-lab2-1.png) 

2. In the **Overview** page for your Synapse workspace, in the **Open Synapse Studio** card, select **Open** to Open Synapse Studio in a new browser tab, sign in if prompted.

   ![](./images/labimg2.png)

3. On the left side of Synapse Studio, use the **&rsaquo;&rsaquo;** icon to expand the menu - this reveals the different pages within Synapse Studio that you'll use to manage resources and perform data analytics tasks. 

   ![](./images/synapse-lab2-2.png)

4. On the **Data** **(1)** page, view the **Linked** **(2)** tab and verify that your workspace includes a link to your Azure Data Lake Storage Gen2 storage account, which should have a name similar to **synapse*xxxxxxx* (Primary - datalake*xxxxxxx*)**.

   ![](./images/labimg3.png)

5. Expand your storage account and verify that it contains a file system container named **files**.

6. Select the **files** container, and note that it contains a folder named **sales**. This folder contains the data files you are going to query.

   ![](./images/lamimg4.png)

7. Open the **sales** folder and the **csv** folder it contains, and observe that this folder contains .csv files for three years of sales data.

   ![](./images/labimg5.png)

8. Right-click any of the files and select **Preview** to see the data it contains. Note that the files do not contain a header row, so you can unselect the option to display column headers. Close the preview. 

    ![](./images/synapse-lab2-3.png)

9. Close the preview, and then use the **&#8593;** button to navigate back to the **sales** folder. 

    ![](./images/synapse-lab2-4.png)

10. In the **sales** folder, open the **json** folder and observe that it contains some sample sales orders in .json files. Preview any of these files to see the JSON format used for a sales order.

    ![](./images/labimg6.png)

11. Close the preview, and then use the **&#8593;** button to navigate back to the **sales** folder. 

12. In the **sales** folder, open the **parquet** folder and observe that it contains a subfolder for each year (2019-2021), in each of which a file named **orders.snappy.parquet** contains the order data for that year.  

    ![](./images/synapse-lab2-6.png)

13. Return to the **sales** folder so you can see the **csv**, **json**, and **parquet** folders.

### Task 2.2: Use SQL to query CSV files

1. Right-click the **csv** folder, and then in the **New SQL script (1)** list on the toolbar, select **Select TOP 100 rows (2)**. 

    ![](./images/synapse-lab2-7.png)

2. In the **File type** list, select **Text format (1)**, and then **Apply (2)** the settings to open a new SQL script that queries the data in the folder. 

    ![](./images/synapse-lab2-8.png)

3. In the **Properties** pane for **SQL Script 1** that is created, change the name to **Sales CSV query (1)**, and change the result settings to show **All rows (2)**. Then in the toolbar, select **Publish (3)** to save the script and use the **Properties (4)** button (which looks similar to **&#128463;**) on the right end of the toolbar to hide the **Properties** pane. 

    ![](./images/synapse-lab2-9.png)

4. Review the SQL code that has been generated, which should be similar to this:

    ```SQL
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/csv/',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0'
        ) AS [result]
    ```

    This code uses the OPENROWSET to read data from the CSV files in the sales folder and retrieves the first 100 rows of data.

5. Select the **...** next to publish and in the **Connect to** list, ensure **Built-in** is selected - this represents the built-in SQL Pool that was created with your workspace.
 
    ![](./images/synapse-lab2-10.png)
    
6. On the toolbar, use the **&#9655; Run** button to run the SQL code, and review the results, which should look similar to this:

    ![Azure portal with a cloud shell pane](./images/DP-203(1).png)

7. Note the results consist of columns named C1, C2, and so on. In this example, the CSV files do not include the column headers. While it's possible to work with the data using the generic column names that have been assigned, or by ordinal position, it will be easier to understand the data if you define a tabular schema. To accomplish this, add a WITH clause to the OPENROWSET function as shown here (replacing *datalakexxxxxxx* with the name of your data lake storage account), and then rerun the query:

    ```SQL
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/csv/',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0'
        )
        WITH (
            SalesOrderNumber VARCHAR(10) COLLATE Latin1_General_100_BIN2_UTF8,
            SalesOrderLineNumber INT,
            OrderDate DATE,
            CustomerName VARCHAR(25) COLLATE Latin1_General_100_BIN2_UTF8,
            EmailAddress VARCHAR(50) COLLATE Latin1_General_100_BIN2_UTF8,
            Item VARCHAR(30) COLLATE Latin1_General_100_BIN2_UTF8,
            Quantity INT,
            UnitPrice DECIMAL(18,2),
            TaxAmount DECIMAL (18,2)
        ) AS [result]
    ```

    Now the results look like this:

    ![Azure portal with a cloud shell pane](./images/DP-203(2).png)

8. Publish the changes to your script, and then close the script pane.

### Task 2.3: Use SQL to query parquet files

While CSV is an easy format to use, it's common in big data processing scenarios to use file formats that are optimized for compression, indexing, and partitioning. One of the most common of these formats is *parquet*.

1. In the **files** tab contaning the file system for your data lake, return to the **sales** folder so you can see the **csv**, **json**, and **parquet** folders.

2. Right-click the **parquet (1)** folder, and then in the **New SQL script (2)** list on the toolbar, select **Select TOP 100 rows (3)**. 

    ![](./images/synapse-lab2-11.png)

3. In the **File type** list, select **Parquet format**, and then **Apply** the settings to open a new SQL script that queries the data in the folder. The script should look similar to this:

    ```SQL
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/parquet/**',
            FORMAT = 'PARQUET'
        ) AS [result]
    ```

4. Run the code, and note that it returns sales order data in the same schema as the CSV files you explored earlier. The schema information is embedded in the parquet file, so the appropriate column names are shown in the results.

5. Modify the code as follows (replacing *datalakexxxxxxx* with the name of your data lake storage account) and then run it.

    ```sql
    SELECT YEAR(OrderDate) AS OrderYear,
           COUNT(*) AS OrderedItems
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/parquet/**',
            FORMAT = 'PARQUET'
        ) AS [result]
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear
    ```

6. Note that the results include order counts for all three years - the wildcard used in the BULK path causes the query to return data from all subfolders.

    The subfolders reflect *partitions* in the parquet data, which is a technique often used to optimize performance for systems that can process multiple partitions of data in parallel. You can also use partitions to filter the data.

7. Modify the code as follows (replacing *datalakexxxxxxx* with the name of your data lake storage account) and then run it.

    ```sql
    SELECT YEAR(OrderDate) AS OrderYear,
           COUNT(*) AS OrderedItems
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/parquet/year=*/',
            FORMAT = 'PARQUET'
        ) AS [result]
    WHERE [result].filepath(1) IN ('2019', '2020')
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear
    ```

8. Review the results and note that they include only the sales counts for 2019 and 2020. This filtering is achieved by inclusing a wildcard for the partition folder value in the BULK path (*year=\**) and a WHERE clause based on the *filepath* property of the results returned by OPENROWSET (which in this case has the alias *[result]*).

9. Name your script **Sales Parquet query (1)**, and **Publish (2)** it. Then close the script pane. 

    ![](./images/synapse-lab2-13.png)

### Task 2.4: Use SQL to query JSON files

JSON is another popular data format, so it;s useful to be able to query .json files in a serverless SQL pool.

1. In the **files** tab containing the file system for your data lake, return to the **sales** folder so you can see the **csv**, **json**, and **parquet** folders.

2. Right-click the **json (1)** folder, and then in the **New SQL script (2)** list on the toolbar, select **Select TOP 100 rows (3)**. 

    ![](./images/synapse-lab2-14.png)

3. In the **File type** list, select **Text format**, and then **Apply** the settings to open a new SQL script that queries the data in the folder. The script should look similar to this:

    ```sql
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/json/',
            FORMAT = 'CSV',
            PARSER_VERSION = '2.0'
        ) AS [result]
    ```

    The script is designed to query comma-delimited (CSV) data rather then JSON, so you need to make a few modifications before it will work successfully.

4. Modify the script as follows (replacing *datalakexxxxxxx* with the name of your data lake storage account) to:
    - Remove the parser version parameter.
    - Add parameters for field terminator, quoted fields, and row terminators with the character code *0x0b*.
    - Format the results as a single field containing the JSON row of data as an NVARCHAR(MAX) string.

    ```sql
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/json/',
            FORMAT = 'CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b',
            ROWTERMINATOR = '0x0b'
        ) WITH (Doc NVARCHAR(MAX)) as rows
    ```

5. Run the modified code and observe that the results include a JSON document for each order.

6. Modify the query as follows (replacing *datalakexxxxxxx* with the name of your data lake storage account) and run the modified code so that it uses the JSON_VALUE function to extract individual field values from the JSON data.

    ```sql
    SELECT JSON_VALUE(Doc, '$.SalesOrderNumber') AS OrderNumber,
           JSON_VALUE(Doc, '$.CustomerName') AS Customer,
           Doc
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/json/',
            FORMAT = 'CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b',
            ROWTERMINATOR = '0x0b'
        ) WITH (Doc NVARCHAR(MAX)) as rows
    ```

7. Name your script **Sales JSON query**, and publish it. Then close the script pane.

## Task 3: Access external data in a database

So far, you've used the OPENROWSET function in a SELECT query to retrieve data from files in a data lake. The queries have been run in the context of the **master** database in your serverless SQL pool. This approach is fine for an initial exploration of the data, but if you plan to create more complex queries it may be more effective to use the *PolyBase* capability of Synapse SQL to create objects in a database that reference the external data location.

In this task, you will be creating an external data source and use SQL script to create the database with tables.

### Task 3.1: Create an external data source

By defining an external data source in a database, you can use it to reference the data lake location where the files are stored.

1. In Synapse Studio, on the **Develop (1)** page, select **+ (2)** menu and then select **SQL script (3)**. 

    ![](./images/synapse-lab2-17.png) 

2. In the new script pane, add the following code (replacing *datalakexxxxxxx* with the name of your data lake storage account) to create a new database and add an external data source to it and run it.

    ```sql
    CREATE DATABASE Sales
      COLLATE Latin1_General_100_BIN2_UTF8;
    GO;

    Use Sales;
    GO;

    CREATE EXTERNAL DATA SOURCE sales_data WITH (
        LOCATION = 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/'
    );
    GO;
    ```

3. Modify the script properties to change its name to **Create Sales DB**, and publish it.

4. Ensure that the script is connected to the **Built-in** SQL pool and the **master** database, and then run it.

5. Switch back to the **Data** page and use the **&#8635;** button at the top right of Synapse Studio to refresh the page. Then view the **Workspace** tab in the **Data** pane, where a **SQL database** list is now displayed. Expand this list to verify that the **Sales** database has been created. 

    ![](./images/synapse-lab2-20.png)

6. Expand the **Sales (1)** database, its **External Resources (2)** folder, and the **External data sources (3)** folder under that to see the **sales_data (4)** external data source you created. 

    ![](./images/synapse-lab2-21.png)

7. In the **...** menu for the **Sales** database, select **New SQL script** > **Empty script**. Then in the new script pane, enter and run the following query:

    ```sql
    SELECT *
    FROM
        OPENROWSET(
            BULK 'csv/*.csv',
            DATA_SOURCE = 'sales_data',
            FORMAT = 'CSV',
            PARSER_VERSION = '2.0'
        ) AS orders
    ```

    The query uses the external data source to connect to the data lake, and the OPENROWSET function now only need to reference the relative path to the .csv files.

8. Modify and re-run the code as follows to query the parquet files using the data source.

    ```sql
    SELECT *
    FROM  
        OPENROWSET(
            BULK 'parquet/year=*/*.snappy.parquet',
            DATA_SOURCE = 'sales_data',
            FORMAT='PARQUET'
        ) AS orders
    WHERE orders.filepath(1) = '2019'
    ```

### Task 3.2: Create an external table

The external data source makes it easier to access the files in the data lake, but most data analysts using SQL are used to working with tables in a database. Fortunately, you can also define external file formats and external tables that encapsulate rowsets from files in database tables.

1. Replace the SQL code with the following statement to define an external data format for CSV files, and an external table that references the CSV files, and run it:

    ```sql
    CREATE EXTERNAL FILE FORMAT CsvFormat
        WITH (
            FORMAT_TYPE = DELIMITEDTEXT,
            FORMAT_OPTIONS(
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '"'
            )
        );
    GO;

    CREATE EXTERNAL TABLE dbo.orders
    (
        SalesOrderNumber VARCHAR(10),
        SalesOrderLineNumber INT,
        OrderDate DATE,
        CustomerName VARCHAR(25),
        EmailAddress VARCHAR(50),
        Item VARCHAR(30),
        Quantity INT,
        UnitPrice DECIMAL(18,2),
        TaxAmount DECIMAL (18,2)
    )
    WITH
    (
        DATA_SOURCE =sales_data,
        LOCATION = 'csv/*.csv',
        FILE_FORMAT = CsvFormat
    );
    GO
    ```

2. Refresh and expand the **External tables** folder in the **Data** pane and confirm that a table named **dbo.orders** has been created in the **Sales** database. 

    ![](./images/synapse-lab2-23.png)

3. In the **...** menu for the **dbo.orders** table, select **New SQL script** > **Select TOP 100 rows**. 

4. Run the SELECT script that has been generated, and verify that it retrieves the first 100 rows of data from the table, which in turn references the files in the data lake.

## Task 4: Visualize query results

Now that you've explored various ways to query files in the data lake by using SQL queries, you can analyze the results of these queries to gain insights into the data. Often, insights are easier to uncover by visualizing the query results in a chart; which you can easily do by using the integrated charting functionality in the Synapse Studio query editor.

In this task, you will work on retriving the data using SQL script and visualize using the chart tool.

1. In Synapse Studio, on the **Data** page, Sales (SQL) database in the **...** menu, select **SQL script** and hit on empty script.

2. Ensure that the script is connected to the **Built-in** SQL pool and the **Sales** database.

3. Enter and run the following SQL code:

    ```sql
    SELECT YEAR(OrderDate) AS OrderYear,
           SUM((UnitPrice * Quantity) + TaxAmount) AS GrossRevenue
    FROM dbo.orders
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear;
    ```
4. In the **Results** pane, select **Chart** and view the chart that is created for you, which should be a line chart.


5. Change the **Category column** to **OrderYear** so that the line chart shows the revenue trend over the three year period from 2019 to 2021:

    ![A line chart showing revenue by year](./images/DP500-1-48.png)

6. Switch the **Chart type** to **Column** to see the yearly revenue as a column chart:

    ![A column chart showing revenue by year](./images/DP500-1-49.png)

7. Experiment with the charting functionality in the query editor. It offers some basic charting capabilities that you can use while interactively exploring data, and you can save charts as images to include in reports. However, functionality is limited compared to enterprise data visualization tools such as Microsoft Power BI.

 > **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:
 > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
 > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
 > - If you need any assistance, please contact us at cloudlabs-support@spektrasystems.com. We are available 24/7 to help you out.

 <validation step="1d10d0ee-5647-48b0-9de8-6c71a9ca1ac5" />

## Summary

In this lab, you have explored how to query data effectively from various files, enabling you to extract meaningful insights. Additionally, you have learned to visualize this data, transforming raw information into intuitive visual representations that aid in decision-making. Furthermore, you integrated external data sources into your database.

## Review

In this lab, you have accomplished the following:
- Query the data in files
- Access external data in a database
- Visualize the query results

## You have successfully completed the lab.

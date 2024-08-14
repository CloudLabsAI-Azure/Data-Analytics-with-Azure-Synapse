# Lab 05: Transform data using Spark in Synapse Analytics

## Lab Scenario

Data *engineers* often use Spark notebooks as one of their preferred tools to perform *extract, transform, and load (ETL)* or *extract, load, and transform (ELT)* activities that transform data from one format or structure to another. In this lab, you will explore the Spark notebook in Azure Synapse Analytics to transform data into files.

### Objectives
  
After completing this lab, you will be able to:

+ Task 1: Provision an Azure Synapse Analytics workspace.
+ Task 2: Use a Spark notebook to transform data.


### Estimated timing: 30 minutes

### Architecture Diagram

   ![Azure portal with a cloud shell pane](./Lab-Scenario-Preview/media/lab6.png)

## Task 1: Provision an Azure Synapse Analytics workspace

In this task, you'll use a Spark notebook in Azure Synapse Analytics to transform data in files.

You'll need an Azure Synapse Analytics workspace with access to data lake storage and a Spark pool.

In this task, you'll use a combination of a PowerShell script and an ARM template to provision an Azure Synapse Analytics workspace.


1. In Azure Portal, click on the **Cloud Shell** button **[\>_]** to the right of the search bar at the top of the page to create a new Cloud Shell, select  ***PowerShell*** environment and click on **Create storage** if prompted. The cloud shell provides a command line interface in a pane at the bottom of the Azure portal, as shown here:

    ![Azure portal with a cloud shell pane](./images/cloud-shell.png)

2. Note that you can resize the cloud shell by dragging the separator bar at the top of the pane, or by using the **&#8212;**, **&#9723;**, and **X** icons at the top right of the pane to minimize, maximize, and close the pane. For more information about using the Azure Cloud Shell, see the [Azure Cloud Shell documentation](https://docs.microsoft.com/azure/cloud-shell/overview).


3. If You dont have precreated storage account, select mount storage account and subscription.

    ![Azure portal with a cloud shell pane](./images/cloudshell_image1.png)

4. Select i want to create a storage, click on **Next**. provide all the details, give unique storage account name and in file share section write **None**. Click on **Create**.

   ![Azure portal with a cloud shell pane](./images/cloudshell_image2.png)

   ![Azure portal with a cloud shell pane](./images/cloudshell_image3.png)

5. In the PowerShell pane, enter the following commands to clone this repo:

    ```
    rm -r synapse -f
    git clone https://github.com/CloudLabsAI-Azure/Data-Analytics-with-Azure-Synapse synapse
    ```

6. After the repo has been cloned, enter the following commands to change to the folder for this exercise and run the **setup.ps1** script it contains:

    ```
    cd synapse/Allfiles/labs/06
    ./setup.ps1
    ```

7. If prompted, choose which subscription you want to use (this will only happen if you have access to multiple Azure subscriptions).

8. When prompted, enter a suitable password to be set for the Azure Synapse SQL pool.

    > **Note**: Be sure to remember this password!

9. Wait for the script to complete - this typically takes around 10 minutes, but in some cases may take longer. While you are waiting, review the [Apache Spark in Azure Synapse Analytics Core Concepts](https://learn.microsoft.com/azure/synapse-analytics/spark/apache-spark-concepts) article in the Azure Synapse Analytics documentation.

## Task 2: Use a Spark notebook to transform data

In this task, you will be downloading a precreated notebook and using the spark notebook to transform data using the downloaded notebook.

1. After the deployment script has completed, in the Azure portal, go to the **data-XXXX** resource group that it created, and notice that this resource group contains your Synapse workspace, a Storage account for your data lake, and an Apache Spark pool.

2. Select your Synapse workspace, and in its **Overview** page, in the **Open Synapse Studio** card, select **Open** to open Synapse Studio in a new browser tab, sign in if prompted.

   ![](./images/labimg7.png)

3. On the left side of Synapse Studio, use the **&rsaquo;&rsaquo;** icon to expand the menu - this reveals the different pages within Synapse Studio that you'll use to manage resources and perform data analytics tasks.

4. On the **Manage** page, select the **Apache Spark pools** tab and note that a Spark pool with a name similar to **sparkxxxxxxx** has been provisioned in the workspace.

   ![](./images/labimg13.png)

5. On the **Data** **(1)** page, view the **Linked** **(2)** tab and verify that your workspace includes a link to your Azure Data Lake Storage Gen2 storage account, which should have a name similar to **synapse*xxxxxxx* (Primary - datalakexxxxxxx)**.

   ![](./images/labimg3.png)

6. Expand your storage account and verify that it contains a file system container named **files (Primary)**.

7. Select the **files** container, and note that it contains folders named **data** and **synapse**. The synapse folder is used by Azure Synapse, and the **data** folder contains the data files you are going to query.

8. Open the **data** folder and observe that it contains .csv files for three years of sales data.

   ![](./images/labimg14.png)

9. Right-click any of the files and select **Preview** to see the data it contains. Note that the files contain a header row, so you can select the option to display column headers.

10. Close the preview. Then download the **Spark Transform.ipynb** from https://github.com/CloudLabsAI-Azure/Data-Analytics-with-Azure-Synapse/blob/main/Allfiles/labs/06/notebooks/Spark%20Transform.ipynb

    > **Note**: It's best to copy this text using the ***ctrl+a*** then ***ctrl+c*** and pasting into a tool using ***ctrl+v***, such as, notepad and then using file, save as **Spark Transform.ipynb** with a filetype of ***all files***.

11. Then on **Develop** page, click on the **+** icon, then select Import option.

    ![Spark Notebook import](./image/../images/dp-203-lab6-1.png)

12. Select the file you just downloaded and saved as **Spark Transfrom.ipynb**.

13. Go to your storage account, click on **Access Control (IAM)**  and click on add role assignment. Search for **Storage Blob Data Contributor** Role under job function roles. 

    ![Spark Notebook import](./image/../images/Str_01.png)

15. Select the role Blob contributor as shown in the screenshot and click on Next. under members click on **Select members**.

    ![Spark Notebook import](./image/../images/Str_02.png)

16. Provide the odl user name, click on it and click on select. click on **Next** twice and click on **Review + assign**.

    ![Spark Notebook import](./image/../images/Str_03.png)

17. Go back to your notebook, attach the notebook to your **sparkxxxxxxx** Spark pool.

    >**Note**: If you don't find the **Attach to** option, kindly collapse the **Develop** pane to view the **Attach to** option next to the outline option.

18. Review the notes in the notebook and run the each code cells.

    > **Note**: The first code cell will take a few minutes to run because the Spark pool must be started. Subsequent cells will run more quickly.


<validation step="29d3bdbe-a709-4d66-8667-d5b765e92d45" />

  **Congratulations** on completing the lab! Now, it's time to validate it. Here are the steps:

  > - Navigate to the Lab Validation tab, from the upper right corner in the lab guide section.
  > - Hit the Validate button for the corresponding task. If you receive a success message, you have successfully validated the lab. 
  > - If not, carefully read the error message and retry the step, following the instructions in the lab guide.
  > - If you need any assistance, please contact us at labs-support@spektrasystems.com.

## Summary

In this lab, you have provisioned an Azure Synapse Analytics workspace and utilized a Spark notebook to transform data, equipping you with the skills to manage and process large datasets efficiently within the Synapse environment.

## Review

In this lab,  you have accomplished the following:
- Provision an Azure Synapse Analytics workspace
- Use a Spark notebook to transform data

## You have successfully completed the lab.

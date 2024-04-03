# Azure-Synapse-Serverless-SQL-Pool-
Use Azure Synapse serverless SQL pool to query files in a data lake
## Overview
This project aims to utilise **Azure Synapse Analytics** to query data in a **data lake**. Azure Synapse Analytics provides **serverless SQL pools** thatn enables the querying of data files in common file formats such as **delimited text** and **Parquet**. In this exercise, we will make use of a combination of a **PoweShell script** and **an ARM templaate** to provision an **Azure Analytics Synapse Workspace**.
1. Sign into the [Azure portal](https://portal.azure.com).<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/b1d89f36-7c58-431f-99e5-56d6bcd0cbf4)<p>
2. Click on the [>_] button located to the right of the search bar at the top of the page in the Azure portal to initiate the creation of a new Cloud Shell. Choose a PowerShell environment and set up storage if requested.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/3bb22cc4-ab37-4e48-807f-2a3f57a03db5)<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/27878dee-f8de-46ec-9dd7-d5df6481768c)<p>
3. In the PowerShell pane, type in the following commands to clone this repo:
```
rm -r dp500 -f
git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst dp500
```
4. After the repo has been cloned, enter the following commands to change to the folder for this lab and run the setup.ps1 script it contains:
```
cd dp500/Allfiles/01
./setup.ps1
```
5. Enter a suitable password when prompted to be set up for the **Azure Synapse SQL pool**.
6. If prompted, choose which subscription you want to use (this will only happen if you have access to multiple Azure subscriptions).
7. Wait for the script to complete the set up.

## Query Data in Files 
The script in the **./setup.ps1** sets up **an Azure Synapse Analytics workspace** and **an Azure Storage account** to store the **data lake**, and then uploads some data files to the data lake.
### View Files in the Data Lake
1. After the script finishes, in the Azure portal, navigate to the dp500- xxxxxxx resource group it generated, and choose your Synapse workspace.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/29be4743-9d87-4cb5-beb3-6dee45319f6f)<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/7893ee4d-b63a-4b3a-ad1d-6fc988f8296c)<p>
2. On the Overview page for your Synapse workspace, click "Open" on the Open Synapse Studio card to launch Synapse Studio in a new browser tab, and sign in if prompted.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/518ee938-edb2-4e3a-9f0d-cc06d8bf0bed)<p>
3. On the left side of Synapse Studio, click the ›› icon to expand the menu, which will display the various pages used to manage resources and perform data analytics tasks within Synapse Studio.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/2cfc60a5-2d7f-47d5-a1ba-8a3ceac7d67e)<p>
4.Navigate to the Data page, go to the Linked tab, and confirm that your workspace has a link to your Azure Data Lake Storage Gen2 storage account, which should have a name similar to "synapse xxxxxxx (Primary - datalake xxxxxxx)".<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/2f96beb2-730c-47b6-94a1-4b2906b2be7a)<p>
5. Expand the storage account and ensure that it includes a file system container named "files".<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/15560ddf-b05a-4468-b23e-042c4f00d2f1)<p>
6. Choose the "files" container, and observe that it includes a folder named "sales" containing the data files we intend to query.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/c7dca12c-0c87-4e89-ba01-fdb6bd47378f)<p>
7. Open the "sales" folder and the "csv" folder within it, and note that the latter contains .csv files for three years of sales data.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/4ba49e08-fb47-4a1a-9005-16f9bf37d40a)<p>
8. Right-click on any of the files and choose **Preview** to view the data it contains. Please note that the files do not include a header row, so you can deselect the option to display column headers.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/14c6e8aa-5b81-4d2c-8846-e0749f032ed2)<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/4c496b68-6803-4067-a459-e35098aec13c)<p>
9. Close the preview, and then use the ↑ button to navigate back to the sales folder.
10. Open the **json** folder within the sales folder, and note that it contains sample sales orders in .json files. Preview any of these files to view the JSON format used for a sales order.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/cc702d87-d6cb-4b5c-9392-e03eac3f309a)<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/51db3075-1f6b-4303-ae9e-b0c93d6d8ed2)<p>
11. Close the preview, and then use the ↑ button to navigate back to the sales folder.
12. Open the **parquet** folder within the sales folder, and note that it includes a subfolder for each year (2019-2021), with each containing a file named **orders.snappy.parquet** that holds the order data for that year.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/e8fa1c8c-838e-45b4-a492-a04ceb2d0041)<p>
13.Return to the sales folder so you can see the csv, json, and parquet folders.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/431b5e2f-a4d9-40d1-9682-5c4ba7718dc4)<p>

### Use SQL to Query CSV Files
1. Choose the csv folder, and then from the New SQL script list on the toolbar, select "Select TOP 100 rows".<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/ca55d278-9e54-4fc9-a950-d4ebcd927617)<p>
2. In the File type list, select Text format, and then apply the settings to open a new SQL script that queries the data in the folder.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/24cf00b1-d4e8-4ffa-9293-049b66eb71c0)<p>
3. In the Properties pane for SQL Script 1, change the name to **Sales CSV query**, and adjust the result settings to display **All rows**. Then, click "Publish" on the toolbar to save the script, and use the Properties button (which looks similar to 🗏) at the right end of the toolbar to hide the Properties pane.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/cda69889-94b7-4909-a5cf-41879da5e007)<p>

Review the SQL code that has been generated, which should be similar to this:
```
SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK 'https://datalake7zr8296.dfs.core.windows.net/files/sales/csv/**',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0'
    ) AS [result]
```
This code utilizes the OPENROWSET to read data from the CSV files in the sales folder and fetches the first 100 rows of data.
5. In the **Connect to** list, make sure **Built-in** is selected - this represents the built-in SQL Pool that was created with your workspace.
6. On the toolbar, click the ▷ Run button to execute the SQL code, and examine the results, which should resemble this:<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/beeb4532-dad4-48c2-a501-f335d53b466a)<p>
7. Note that the results consist of columns named C1, C2, and so on. In this example, the CSV files do not include the column headers. While it's possible to work with the data using the generic column names that have been assigned, or by ordinal position, it will be easier to understand the data if you define a tabular schema. To accomplish this, add a WITH clause to the OPENROWSET function as shown here (replacing "datalakexxxxxxx" with the name of your data lake storage account), and then rerun the query:<p>
```
SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK 'https://datalake7zr8296.dfs.core.windows.net/files/sales/csv/**',
        FORMAT = 'CSV',
        PARSER_VERSION = '2.0'
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
    ) AS [result]
```
Now, our new table with headers:<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/095f5089-cdef-4561-b64f-22b35f0689c1)<p>
8. Save the changes to your script by clicking **Publish**, and then close the script pane.
### Query Parquet Files with SQL
Although CSV is simple to work with, it is typical in large-scale data processing situations to employ file formats that are designed for compression, indexing, and partitioning. Parquet is one of the most frequently used formats for this purpose.
1. Go back to the files tab containing the file system for your data lake, and navigate to the sales folder to view the csv, json, and parquet folders.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/86008cf9-a61b-485b-b1cc-5d332650e23a)<p>
2. Select the parquet folder, and then in the New SQL script list on the toolbar, select Select TOP 100 rows.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/a130e866-56a8-4af6-99bd-2b1b7f4bf64e)<p>
3. In the "File type" list, choose "Parquet format", and then apply the settings to open a new SQL script that queries the data in the folder. The script should resemble this:
```
-- This is auto-generated code
SELECT
    TOP 100 *
FROM
    OPENROWSET(
        BULK 'https://datalake7zr8296.dfs.core.windows.net/files/sales/parquet/**',
        FORMAT = 'PARQUET'
    ) AS [result]
```
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/05c6f938-ceb6-4e32-a5c0-dc9c386c89fe)<p>
4. Run the code, and observe that it retrieves sales order data with the same schema as the CSV files you previously examined. The schema information is embedded in the Parquet file, so the correct column names are displayed in the results.<p>
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/081939f5-4974-4f11-8110-f2d2cc41aa51)<p>
5.Modify the code as follows (replacing "datalakexxxxxxx" with the name of your data lake storage account) and then run it.
```
SELECT YEAR(OrderDate) AS OrderYear,
       COUNT(*) AS OrderedItems
FROM
    OPENROWSET(
        BULK 'https://datalake7zr8296.dfs.core.windows.net/files/sales/parquet/**',
        FORMAT = 'PARQUET'
    ) AS [result]
GROUP BY YEAR(OrderDate)
ORDER BY OrderYear
```
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/119e6d22-6950-4296-8080-366dc71bb30c)<p>
Note that the results include order counts for all three years - the wildcard used in the BULK path causes the query to return data from all subfolders.
The subfolders reflect partitions in the Parquet data, which is a technique often used to optimize performance for systems that can process multiple partitions of data in parallel. You can also use partitions to filter the data.
6. Modify the code as follows (replacing datalakexxxxxxx with the name of your data lake storage account) and then run it.
```
SELECT YEAR(OrderDate) AS OrderYear,
       COUNT(*) AS OrderedItems
FROM
    OPENROWSET(
        BULK 'https://datalake7zr8296.dfs.core.windows.net/files/sales/parquet/year=*/',
        FORMAT = 'PARQUET'
    ) AS [result]
WHERE [result].filepath(1) IN ('2019', '2020')
GROUP BY YEAR(OrderDate)
ORDER BY OrderYear
```
![image](https://github.com/JonesKwameOsei/Azure-Synapse-Serverless-SQL-Pool-/assets/81886509/de7896ee-a6f3-4db7-977e-dd86542a91e6)<p>









































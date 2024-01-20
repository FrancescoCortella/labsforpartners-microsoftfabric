In the previous section of the lab, you used Databricks to work on CDC and data quality of one dimension (customer). The lab will now fork into two sections to showcase the following scenarios:

2.1 Databricks + Fabric (here)

2.2 Databrics + PowerBI (ask Databricks team)

# **LAB 3 part 2.1: Fabric & Databricks Better Together**

In the previous section of the lab, you used Databricks to work on CDC and data quality of one dimension (customer). Let’s assume that you have followed the same process to build the other dimensions, which you have written in a gold layer in ADLS gen2, following the medallion framework.

In this part of the lab, you will make use of:
1. The shortcut capability of Fabric to connect to the ADLS gen2
2. PowerBI dashboard within Fabric leveraging direct-lake and v-order
3. Data Activator to set up alerts when your data meets specific conditions

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/3f267f60-d97c-4970-8477-fce698bc3861)

## **Create a workspace**
In this step, you create a Fabric workspace. The workspace contains all the items needed for this lakehouse tutorial, which includes lakehouse, dataflows, Data Factory pipelines, the notebooks, Power BI semantic models, and reports.
1.	Sign in to Power BI  https://msit.powerbi.com/
2.	Select Workspaces and New workspace.

  	![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/b6cf9bc6-a0a1-4137-a6a1-028c2c737651)
 
3. Fill out the Create a workspace form with the following details:
    •	Name: Enter **Fabric Databricks Lab**, and any extra characters to make the name unique.
    •	Description: Enter an optional description for your workspace.
   	![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/f80f4d1d-1a7d-435c-9f68-e40543326157)

## **Create a lakehouse**
1.	In the Power BI service, select Workspaces from the left-hand menu.
2.	To open your workspace, enter its name in the search textbox located at the top and select it from the search results.
3.	From the experience switcher located at the bottom left, select Data Engineering.
   
     ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/20fa7181-4e9e-4b66-856c-82a64096ed9c)
 
4.	In the Data Engineering tab, select Lakehouse to create a lakehouse.
5.	In the New lakehouse dialog box, enter LH_gold in the Name field.

    ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/446cc383-ebdb-4437-9a74-720f0c6849f0)

6.	Select Create to create and open the new lakehouse.


## **Setting up a shortcut**
1.	To create a shortcut, open Lakehouse Explorer, click on the three dots next to Files and select New Shortcut.
   ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/af5c7703-bf1b-4b5e-81b4-cdc68b89d8db)

2.	Select Azure Data Lake Storage Gen2 on external source, and connect to the resource where you have written the gold layer. 
3.	Complete URL with https://adlsmsdbpartners.dfs.core.windows.net/, and select Account Key as Authentication kind. The account key is the following string: eFvWqJCfctoJkSbqsfXYdggfkz/Lx+O8rcQqMPxnuBxdwy4pDJtIjzBx9pFVvTeIuCTn4Rg+5chj+AStkO4Gvg==
   ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/909f8196-7b5e-40eb-9119-4c12d7afad70)

4.	Use gold_shortcut as shortcut Name and enter /gold-container in the Sub Path. **This is the ADLS container where your tables would land after Databricks ETL operations.**
   
   ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/00fb8552-48a4-4485-916e-80dc621dc0c3)

5. You should be able to see the parquet files in the Files section. Next step is **load them into new Tables.**

6. If you completed all the steps correctly you should able to view the data you stored in the gold-container **under tables**:

   <img width="173" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/51bd060c-c560-4760-883e-541cd8a2ecf1">


**NOTE: Thanks to the shortcut, you are now able to view these tables without any data duplication. You can now use all the Fabric engines on top of the tables which are still stored only in the ADLS**.

## **V-ORDER**



## **Build a report**
Power BI is natively integrated in the whole Fabric experience. This native integration brings a unique mode, called **DirectLake**, of accessing the data from the lakehouse to **provide the most performant query and reporting experience**. DirectLake mode is a groundbreaking new engine capability to analyze very large semantic models in Power BI. The technology is based on the idea of loading parquet-formatted files directly from a data lake without having to query a data warehouse or lakehouse endpoint, and without having to import or duplicate data into a Power BI semantic model. DirectLake is a fast path to load the data from the data lake straight into the Power BI engine, ready for analysis.

In traditional DirectQuery mode, the Power BI engine directly queries the data from the source for each query execution, and the query performance depends on the data retrieval speed. DirectQuery eliminates the need to copy data, ensuring that any changes in the source are immediately reflected in query results. On the other hand, in the import mode, the performance is much better because the data is readily available in memory without having to query the data from the source for each query execution, however the Power BI engine must first copy the data into the memory at data refresh time. Any changes to the underlying data source are picked up during the next data refresh(in scheduled as well as on-demand refresh).

DirectLake mode now eliminates this import requirement by loading the data files directly into memory. Because there's no explicit import process, it's possible to pick up any changes at the source as they occur, thus combining the advantages of DirectQuery and import mode while avoiding their disadvantages. DirectLake mode is therefore the ideal choice for analyzing very large semantic models and semantic models with frequent updates at the source.

1. From your lakehouse, select SQL analytics endpoint from the Lakehouse drop-down menu at the top right of the screen.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/cf30f4d3-d0d0-484c-a322-262f2c383d76)


From the SQL endpoint pane, you should be able to see all the tables you created. If you don't see them yet, select the Refresh icon at the top. Next, select the Model tab at the bottom to open the default Power BI semantic model.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/7458d5d6-0090-4eb4-9f5e-d5b085281284)


For this data model, you need to define the relationship between different tables so that you can create reports and visualizations based on data coming across different tables. From the fact_sale table, drag the CityKey field and drop it on the CityKey field in the dimension_city table to create a relationship. The New relationship dialog box appears.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/73827355-3783-43d1-9d66-69457720a536)


In the New relationship dialog box:

Table 1 is populated with fact_sale and the column of CityKey.

Table 2 is populated with dimension_city and the column of CityKey.

Cardinality: Many to one (*:1)

Cross filter direction: Single

Leave the box next to Make this relationship active selected.

Select the box next to Assume referential integrity.

Select OK.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/c8344f44-83af-4ff5-bb15-f5fd0a99a2e3)


 Note

When defining relationships for this report, make sure you have a many to one relationship from the fact_sale table (Table 1) to the dimension_* tables (Table 2) and not vice versa.

Next, add these relationships with the same New relationship settings as shown above but with the following tables and columns:

StockItemKey(fact_sale) - StockItemKey(dimension_stock_item)
Salespersonkey(fact_sale) - EmployeeKey(dimension_employee)
CustomerKey(fact_sale) - CustomerKey(dimension_customer)
InvoiceDateKey(fact_sale) - Date(dimension_date)
After you add these relationships, your data model is ready for reporting as shown in the following image:

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/5326090f-e8b0-4bc2-ba0a-9b7f47e36f1c)


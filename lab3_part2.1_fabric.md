In the previous section of the lab, you used Databricks to work on CDC and data quality, while running your ETL operations. The lab will now fork into two sections to showcase the following scenarios:

Lab 3: 2.1 Databricks + Fabric (here)

Lab 3: 2.2 Databrics + PowerBI (ask Databricks team)

# **LAB 3 part 2.1: Fabric & Databricks Better Together**

In the previous section of the lab, you used Databricks to work on CDC and data quality of one dimension (customer). Let’s assume that you have followed the same process to build the other dimensions (city, date, employee, stock item, and fact_sale), which you have written in a gold layer in ADLS gen2, following the medallion framework.

In this part of the lab, you will make use of:
1. The shortcut capability of Fabric to connect to the ADLS gen2
2. PowerBI experience within Fabric leveraging direct-lake and v-order
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
5.	In the New lakehouse dialog box, enter **LH_gold** in the Name field.

    ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/446cc383-ebdb-4437-9a74-720f0c6849f0)

6.	Select Create to create and open the new lakehouse.


## **Setting up a shortcut**
1.	To create a shortcut, open Lakehouse Explorer, click on the three dots next to Files and select New Shortcut.
   ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/af5c7703-bf1b-4b5e-81b4-cdc68b89d8db)

2.	Select Azure Data Lake Storage Gen2 on external source, and connect to the resource where the gold layer is written. 
3.	Complete URL with **https://adlsmsdbpartners.dfs.core.windows.net/**, and select **Account Key** as Authentication kind. The account key is the following string: **eFvWqJCfctoJkSbqsfXYdggfkz/Lx+O8rcQqMPxnuBxdwy4pDJtIjzBx9pFVvTeIuCTn4Rg+5chj+AStkO4Gvg==**
   ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/909f8196-7b5e-40eb-9119-4c12d7afad70)

4.	Use gold_shortcut as shortcut Name and enter /gold-container in the Sub Path. **For the sake of the lab, we assume this is the ADLS container where your tables land after Databricks ETL operations.**
   
   ![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/00fb8552-48a4-4485-916e-80dc621dc0c3)

5. You should be able to see the parquet files in the Files section. Next step is **load them into new Tables.**

6. If you completed all the steps correctly you should able to view the data stored in the ADLS gold-container **under tables**:

   <img width="173" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/51bd060c-c560-4760-883e-541cd8a2ecf1">


**NOTE: Thanks to the shortcut, Fabric is now pointing to the root location where these tables are stored, without any data duplication. You can now use all the Fabric engines on top of the tables which are only stored in the ADLS, and not stored in the Fabric one-lake**.

## **V-ORDER**

**What is v-order?**

V-Order is a write time optimization to the parquet file format that enables lightning-fast reads under the Microsoft Fabric compute engines, such as Power BI, SQL, Spark and others.

Power BI and SQL engines make use of Microsoft Verti-Scan technology and V-Ordered parquet files to achieve in-memory like data access times. Spark and other non-Verti-Scan compute engines also benefit from the V-Ordered files with an average of 10% faster read times, with some scenarios up to 50%.

V-Order works by applying special sorting, row group distribution, dictionary encoding and compression on parquet files, thus requiring less network, disk, and CPU resources in compute engines to read it, providing cost efficiency and performance. V-Order sorting has a 15% impact on average write times but provides up to 50% more compression.

It's 100% open-source parquet format compliant; all parquet engines can read it as a regular parquet files. Delta tables are more efficient than ever; features such as Z-Order are compatible with V-Order. Table properties and optimization commands can be used on control V-Order on its partitions.

V-Order is applied at the parquet file level. Delta tables and its features, such as Z-Order, compaction, vacuum, time travel, etc. are orthogonal to V-Order, as such, are compatible and can be used together for extra benefits.

**V-Order is enabled by default in Microsoft Fabric**. You can open a notebook and test it by going back to your workspace. When there, create new and select Notebook.

<img width="200" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/3b5d1480-1b0a-4a84-ad88-8439cd3c8833"><img width="202" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/d5c3b48a-de2d-4988-893c-31a43cad84f4">

Hit add lakehouse and follow the steps to link the notebook to LH_gold.


Once the notebook is created, if you run the following command spark.conf.get('spark.sql.parquet.vorder.enabled') , you will get back "true" as a proof that v-order is enabled.

<img width="771" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/9ae38fd6-f754-4e3b-bcd8-3588d172aa95">



## **Build a report**
Power BI is natively integrated in the whole Fabric experience. This native integration brings a unique mode, called **DirectLake**, of accessing the data from the lakehouse to **provide the most performant query and reporting experience**. DirectLake mode is a groundbreaking new engine capability to analyze very large semantic models in Power BI. The technology is based on the idea of loading parquet-formatted files directly from a data lake without having to query a data warehouse or lakehouse endpoint, and without having to import or duplicate data into a Power BI semantic model. DirectLake is a fast path to load the data from the data lake straight into the Power BI engine, ready for analysis.

In traditional DirectQuery mode, the Power BI engine directly queries the data from the source for each query execution, and the query performance depends on the data retrieval speed. DirectQuery eliminates the need to copy data, ensuring that any changes in the source are immediately reflected in query results. On the other hand, in the import mode, the performance is much better because the data is readily available in memory without having to query the data from the source for each query execution, however the Power BI engine must first copy the data into the memory at data refresh time. Any changes to the underlying data source are picked up during the next data refresh(in scheduled as well as on-demand refresh).

DirectLake mode now eliminates this import requirement by loading the data files directly into memory. Because there's no explicit import process, it's possible to pick up any changes at the source as they occur, thus combining the advantages of DirectQuery and import mode while avoiding their disadvantages. DirectLake mode is therefore the ideal choice for analyzing very large semantic models and semantic models with frequent updates at the source.

1. From your lakehouse, select SQL analytics endpoint from the Lakehouse drop-down menu at the top right of the screen.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/cf30f4d3-d0d0-484c-a322-262f2c383d76)


2. From the SQL endpoint pane, you should be able to see all the tables you created. If you don't see them yet, select the Refresh icon at the top. Next, select the Model tab at the bottom to open the default Power BI semantic model.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/7458d5d6-0090-4eb4-9f5e-d5b085281284)


3. For this data model, you need to define the relationship between different tables so that you can create reports and visualizations based on data coming across different tables. From the fact_sale table, drag the CityKey field and drop it on the CityKey field in the dimension_city table to create a relationship. The New relationship dialog box appears.

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


Note: When defining relationships for this report, make sure you have a many to one relationship from the fact_sale table (Table 1) to the dimension_* tables (Table 2) and not vice versa.

Next, add these relationships with the same New relationship settings as shown above but with the following tables and columns:

StockItemKey(fact_sale) - StockItemKey(dimension_stock_item)

Salespersonkey(fact_sale) - EmployeeKey(dimension_employee)

CustomerKey(fact_sale) - CustomerKey(dimension_customer)

InvoiceDateKey(fact_sale) - Date(dimension_date)


After you add these relationships, your data model is ready for reporting as shown in the following image:

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/5326090f-e8b0-4bc2-ba0a-9b7f47e36f1c)

4. Select New report to start creating reports/dashboards in Power BI. On the Power BI report canvas, you can create reports to meet your business requirements by dragging required columns from the Data pane to the canvas and using one or more of available visualizations.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/9e5b8369-f86f-4a46-a6b2-046a0be390c1)

Add a title:

In the Ribbon, select Text box.

Type in KPIs dashboard.

5. Build a column chart:

On the Visualizations pane, select the Stacked column chart visual.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/66d97169-fe9d-4fc3-9f0b-f7806b9fa976)


On the Data pane, expand fact_sales and check the box next to Profit. This selection adds the field to the Y-axis.

On the Data pane, expand dimension_employee and check the box next to Employee. This selection adds the field to the X-axis. 

6. Click anywhere on the blank canvas (or press the Esc key) so the chart is no longer selected. Enrich the dashboard with any other plot you have in mind.

![image](https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/ff9255d1-a564-412e-b256-f0a4d3cdb38c)

7. From the ribbon, select File > Save.

Enter the name of your report as Summary Reporting.

Select Save.


## **Data Activator**

Data Activator is a no-code experience in Microsoft Fabric for automatically taking actions when patterns or conditions are detected in changing data. It monitors data in Power BI reports and Eventstreams items, for when the data hits certain thresholds or matches other patterns. It then automatically takes appropriate action such as alerting users or kicking off Power Automate workflows.

Data Activator allows customers to build a digital nervous system that acts across all their data, at scale and in a timely manner. Business users can describe business conditions in a no-code experience to launch actions such as Email, Teams notifications, Power Automate flows and call into third party action systems. Business users can self-serve their needs and reduce their reliance on internal IT and/or developer teams, either of which is often costly and hinders agility. Customer organizations don’t need a developer team to manage and maintain custom in-house monitoring or alerting solutions.

It is now time to make use of data activator to convert insights into actions. Click on the top right corner of your plot and select **create an alert**.
<img width="669" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/270f912f-4398-4e50-8d5e-cf14101b9e30">

In this scenario, you want to keep track of your employees KPIs, and more specifically, have insights on which employees are responsible for driving more profit for your company. In this context, we want to reward the salespeople that are driving more than 2bn. Select sum of profit as measure, becomes greater than as condition, and 2,000,000,000 as threshold. Name this item as **sales employee award**, keep start my alert box thicked and hit Create.

<img width="251" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/60727cf7-f4ea-49d9-bf26-3e683f7f5313">

After few minutes, you will find in your email box the alerts triggered by data activator:

<img width="239" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/4f7a4b2f-e4ca-46a6-b9bd-60be6973fcd2">

The emails will contain the name of the employee that won the sales award by beating the threshold (see example below):

<img width="435" alt="image" src="https://github.com/FrancescoCortella/labsforpartners-microsoftfabric/assets/135111177/29fab47a-eca6-4a7c-a942-a9386f1be13f">

To edit your Reflex, you can go back to your workspace click on it. You will be able to add other triggers or change conditions.

This is just a simple use-case, but many other scenarios could be implemented with Data Activator. Just to give you few examples:

1. Run ads when same-store sales decline.
2. Alert store managers to move food from failing grocery store freezers before it spoils.
3. Retain customers who had a bad experience by tracking their journey through apps, websites etc.
4. Help logistics companies find lost shipments proactively by starting an investigation workflow when package status isn’t updated for a certain length of time.
5. Alert account teams when customers fall into arrears, with customized time or value limits per customer.
6. Track data pipeline quality, either rerunning jobs or alerting when pipelines fail or anomalies are detected. (**this could be achieved if you shortcut the data quality table you created on the databricks lab on Fabric**).

## Conclusion
We hope you enjoyed the lab! Feel free to share any feedback or reach out for help with your customers: fcortella@microsoft.com

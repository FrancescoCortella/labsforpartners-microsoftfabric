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


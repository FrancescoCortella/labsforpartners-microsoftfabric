In the previous section of the lab, you used Databricks to work on CDC and data quality of one dimension (customer). The lab will now fork into two sections to showcase the following scenarios:

2.1 Databricks + Fabric (here)

2.2 Databrics + PowerBI (ask Databricks team)

# **LAB 3 part 2.1: Fabric & Databricks Better Together**

In the previous section of the lab, you used Databricks to work on CDC and data quality of one dimension (customer). Let’s assume that you have followed the same process to build the other dimensions, which you have written in a gold layer in ADLS gen2, following the medallion framework.

In this part of the lab, you will make use of:
1. The shortcut capability of Fabric to connect to the ADLS gen2
2. PowerBI dashboard within Fabric leveraging direct-lake and v-order
3. Data Activator to set up alerts when your data meets specific conditions

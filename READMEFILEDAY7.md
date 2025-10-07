#### Azure Synapse Analytics

Azure Synapse Analytics is a cloud-based data platform from Microsoft that helps you collect, store, analyze, and understand large amounts of data — all in one place.

Example :

Imagine a giant, smart warehouse in the cloud where you can:  
  * Store data (structured like Excel, or unstructured like logs or JSON files)  
  * Clean and organize it (using pipelines and scripts)  
  * Run reports or ask questions (using SQL or other tools)  
  * Analyze trends or build dashboards (to help with business decisions)  
  * Do machine learning or real-time processing (if needed)  


| Dimension                                       | Traditional Data Warehouse (On‐Prem / Fixed‑Cloud DW)                                                                                                             | Azure Synapse Analytics                                                                                                                                                                                                                                                       |
| ----------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Architecture / Compute & Storage Separation** | Often tightly coupled compute+storage; scaling may require provisioning more server hardware or VM sizes; scaling tends to be coarse.                             | Synapse offers separation of compute and storage (especially in serverless modes), allowing flexible scaling; has both **provisioned compute** (Dedicated SQL Pools) and **serverless compute** (SQL on demand for lake‑based queries). ([Microsoft Azure][1])                |
| **Flexibility & Workload Types**                | Primarily structured data; often optimized for OLAP‑style reporting and querying with fixed schemas; less native support for semi/unstructured data or streaming. | Synapse supports a broader range: structured, semi‑structured (via Spark, external tables, data lake files), streaming telemetry data, real‑time ingestion; multiple compute engines (SQL, Spark, Azure Data Explorer). ([Microsoft Azure][1])                                |
| **Cost Model**                                  | Typically pay for hardware, licensing, always‑on servers; scaling up can be expensive; under‑utilization is common.                                               | More options: pay‑as‑you‑use (serverless SQL), dedicated compute for predictable workloads, scale compute up/down; separation of storage vs compute helps cost control. ([GetOnData][2])                                                                                      |
| **Time to Insights / Elasticity**               | Slow to adapt: provisioning, adding hardware, adjusting for new data types or volumes can require planning and lead‑time.                                         | Faster: Synapse Studio gives unified workspace; pipelines, serverless queries, and on‑demand compute let you explore/adapt more quickly. ([TECHCOMMUNITY.MICROSOFT.COM][3])                                                                                                   |
| **Data Types and Sources**                      | Structured data predominates; semi‑structured maybe via extensions; fewer native big‑data / lake integrations.                                                    | Strong support for data lakes (e.g. connecting to ADLS Gen2), Parquet/JSON/etc.; can combine data in multiple storage types; supports Spark, external tables, streaming data. ([GetOnData][2])                                                                                |
| **Development & Operational Complexity**        | Well understood patterns; however, maintenance, patching, scaling require operational effort.                                                                     | More moving parts: multiple engines, permissions/access across environments, cost management, performance tuning across different compute modes; but unified tools help.                                                                                                      |
| **Performance / Concurrency**                   | Good when sized properly; on‑prem can have predictable performance; but scaling concurrency often means more hardware.                                            | Synapse provides “workload isolation” and “intelligent workload management” to help concurrent query performance; but some trade‑offs depending on whether using dedicated vs serverless compute. ([Microsoft Azure][1])                                                      |
| **Security, Governance, Compliance**            | Mature in many enterprises; physical control, audit, etc.; but often requires manual implementations for data lakes etc.                                          | Synapse includes built‑in security and compliance: encryption (at rest & in transit), row/column‑level security, dynamic data masking, RBAC, integration with Azure Active Directory, etc. Also good integration with governance tools (e.g. Purview). ([Microsoft Azure][1]) |

[1]: https://azure.microsoft.com/en-gb/products/synapse-analytics//?utm_source=chatgpt.com "Azure Synapse Analytics | Microsoft Azure"
[2]: https://getondata.com/what-is-azure-synapse-a-complete-beginners-guide/?utm_source=chatgpt.com "What is Azure Synapse? A Complete Beginner’s Guide - GetOnData"
[3]: https://techcommunity.microsoft.com/blog/educatordeveloperblog/azure-synapse-a-step-by-step-beginner%E2%80%99s-guide/4197933?utm_source=chatgpt.com "Azure Synapse: A Step-by-Step Beginner’s Guide | Microsoft Community Hub"



•	Create a notebook under > Develop > Notebook
•	Create Apache spark pool before running the notebook (approximately takes 30 seconds) 
•	Create managed identity and assign blob contributor or similar role to it and attach the managed identity to synapse


`file_path = "abfss://sourcedata@steastus2dev001.dfs.core.windows.net/payers.csv"`

`df = spark.read.option("header", "true").csv(file_path)`
`df.show(5)`


`df.printSchema()`
`df.describe().show()`
`df.select("column1", "column2").show(5)`
``


`from pyspark.sql.functions import col`
#### Remove rows with nulls in important columns
`df_clean = df.dropna(subset=["patient_id", "age"])`

#### Cast columns to correct data types
`df_casted = df.withColumn("ZIP", col("ZIP").cast("int"))`
`df.printSchema()`


`output_path = "abfss://raw@steastus2dev001.dfs.core.windows.net/data/processed/patients_cleaned.parquet"`

`df_casted.write.mode("overwrite").parquet(output_path)`


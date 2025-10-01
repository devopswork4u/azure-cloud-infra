### Azure Datalake gen1 vs Gen 2

**1. Architecture and Foundation**  
Gen1: Built on a proprietary architecture designed specifically for big data analytics.  
Gen2: Built on top of Azure Blob Storage, combining the capabilities of Data Lake and object storage.  

**2. Storage Hierarchy**

Gen1: Uses a hierarchical file system (HDFS-like) for organizing data in directories and files.  
Gen2: Also supports a hierarchical namespace, but it’s optional and built into Blob Storage.  

**3. Performance**

Gen1: Good performance but limited scalability in some scenarios.  
Gen2: Higher performance and scalability, optimized through Blob Storage infrastructure.  

**4. Security and Access Control**

Gen1: Uses Access Control Lists (ACLs) for security.  
Gen2: Supports ACLs and also Azure RBAC (Role-Based Access Control) for fine-grained access.  

**5. Integration with Azure Ecosystem**

Gen1: Limited integration with Azure services.  
Gen2: Deeply integrated with modern Azure analytics and AI services like Azure Synapse, Azure Databricks, Power BI, etc.  

**6. Cost Efficiency**

Gen1: More expensive due to dedicated architecture and lack of tiered storage.  
Gen2: Cheaper with support for storage tiers (Hot, Cool, Archive), making it cost-efficient.  

**7. Data Redundancy and Availability**

Gen1: Limited redundancy options (LRS or GRS).  
Gen2: Inherits Blob Storage redundancy models — LRS, GRS, ZRS, etc., offering better resilience.  

**8. Compatibility with Blob APIs**

Gen1: Not compatible with Blob Storage REST APIs.  
Gen2: Fully compatible with Blob APIs, enabling wider tool and SDK support.  

**9. Global Availability**

Gen1: Available in fewer regions.  
Gen2: Available in more regions globally, as it rides on the Blob Storage infrastructure.  

**10. Future Support**

Gen1: Deprecated — No new features; limited support and being phased out.  
Gen2: Actively developed and recommended by Microsoft for all new Data Lake implementations.  

#### Azure Data Lake Gen1 vs Gen2 Comparison

| Feature                | Data Lake Gen1        | Data Lake Gen2              |
|------------------------|-----------------------|-----------------------------|
| **Architecture**       | Proprietary           | Based on Azure Blob Storage |
| **Hierarchical Namespace** | Yes               | Yes (optional)              |
| **Performance**        | Moderate              | High                        |
| **Security**           | ACLs only             | ACLs + Azure RBAC           |
| **Azure Integration**  | Limited               | Deep and broad              |
| **Cost Efficiency**    | Higher cost           | Lower cost with tiering     |
| **Redundancy Options** | LRS/GRS               | LRS/GRS/ZRS                 |
| **API Compatibility**  | No Blob API support   | Full Blob API support       |
| **Global Availability**| Limited               | Wide                        |
| **Support Status**     | Deprecated            | Actively supported          |


### Azure Datalake overview and Data trasformation cycle

<img width="1164" height="616" alt="image" src="https://github.com/user-attachments/assets/bee6fd6f-3290-40a7-9baa-fa67212af3b9" />

[Full Terraform code for infra deployment](https://github.com/devopswork4u/tf_data_transformation)


**Step 1 :** Download sample data from `https://mavenanalytics.io/data-playground`  
**Step 2 :** Create the data factory `datafactory-demo-001` in azure  
**Step 3 :** Create storage account for Datalake Gen2 `sadatalakedemo001`  
**Step 4 :** Create container under `sadatalakedemo001` storage account for datalake as example `vehicle or rawdata`  

### Data factory pipeline creation   

**Step 1 :**  Data factory `datafactory-demo-001`  studio  
**Step 2 :**  configure integration runtime  

**Step 3 :**  After installing integration runtime using key , perform test connection from data factory studio      

**Step 4 :**  Configure linked service type as "filesystem" (to read the local folder data)  

**Step 5 :**  Create pipeline source dataset by selecting the link and sink dataset as datalake (note sink dataset should be configured using azure integration runtime)    



### Intragration runtime settting in case permission issue:  
* if file access issue `cd "C:\Program Files\Microsoft Integration Runtime\5.0\Shared"`  
* run `./dmgcmd -DisableLocalFolderPathValidation`  

### Databricks configurations 
 * Create secret using browser --> "/secrets/createScope" with keyvault URL and keyvault ID
 * Create a secret in AKV for storage account key
 * 
#### Create a new notebook and connect to datalake using below : 
`dbutils.fs.mount(
    source="wasbs://vehicledata@sadatalakedemo002.blob.core.windows.net/",
    mount_point="/mnt/vehicledata",
    extra_configs={
        "fs.azure.account.key.sadatalakedemo002.blob.core.windows.net": dbutils.secrets.get('dbScope', 'saSecret')
    }
)`

##### Now create two more container and create the filesystem by using above command 
    * container name silver
    * container name gold
    * create data format ``

1. to list the imported csv from Datalake to databricks filesystems `dbutils.fs.ls("/mnt/vehicledata")`
2. `dbutils.fs.ls("/mnt/vehicledata")`  
3. Now print the schema `location_df.printSchema()`
4. Change the schema format to integer for `location_df = spark.read.format("csv").option("header", "true").option("inferSchema", "true").load("/mnt/vehicledata/locations.csv")`
5. Now print the schema again `location_df.printSchema()`
6. to print `location_df.show()`
7. Change the column data to integer and remove the comma `location_df=location_df.withColumn("population",regexp_replace("population",",","").cast("int"))`
8. move the formated data to Datalake new stage container `location_df.write.option("header","true").csv("/mnt/silver/location.csv")`  


    

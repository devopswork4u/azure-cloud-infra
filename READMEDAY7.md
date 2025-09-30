## Azure Datalake overview and Data trasformation cycle

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



### Intragration runtime settting:  
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


    

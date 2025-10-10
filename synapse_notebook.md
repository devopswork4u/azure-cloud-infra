#### create data format from Json hosted on DataLake
`df = spark.read.option("multiline", "true").json("abfss://rawdata@<storageaccount>.dfs.core.windows.net/demo.json")`

#### validate Json on Synapse workspace 
`df.show()`

#### select Specific columns from on Synapse workspace 
`df_formated = df.select("Id", "NAME", "ADDRESS", "CITY","STATE_HEADQUARTERED", "ZIP", "PHONE")`

#### validate selected data on Synapse workspace 
`df_formated.show()`

####
`output_path = "abfss://rawdata@<storageaccount>.dfs.core.windows.net/formated/"`

`df_formated.coalesce(1).write.mode("overwrite").option("header", True).csv(output_path)`


#### All Command to import data and show on Synapse

`from pyspark.sql.functions import col`

`df = spark.read.option("multiline", "true").json("abfss://rawdata@saseastus2dev001.dfs.core.windows.net/company.json")`

df_formated = df.select("Id", "NAME", "ADDRESS", "CITY","STATE_HEADQUARTERED", "ZIP", "PHONE")`

`output_path = "abfss://sourcedata@steastus2dev001.dfs.core.windows.net/formated/"`

`df_formated.coalesce(1).write.mode("overwrite").option("header", True).csv(output_path)`

`df_formated.show()`

`df_formated.printSchema()`

#### Filter null record

``



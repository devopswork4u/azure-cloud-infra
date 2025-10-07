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


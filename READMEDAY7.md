### Azure Datalake overview and Data trasformation cycle

**Step 1 :** Download sample data from `https://mavenanalytics.io/data-playground`  
**Step 2 :** Create the data factory `datafactory-demo-001` in azure  
**Step 3 :** Create storage account for Datalake Gen2 `sadatalakedemo001`  
**Step 4 :** Create container under `sadatalakedemo001` storage account for datalake as example `vehicle or rawdata`  

#### Data factory pipeline creation   

**Step 1 :**  Data factory `datafactory-demo-001`  studio  
**Step 2 :**  configure integration runtime  

    <img width="842" height="571" alt="image" src="https://github.com/user-attachments/assets/4ae8f526-4e99-4547-892d-92a397bff0cf" />  
    
**Step 3 :**  After installing integration runtime using key , perform test connection from data factory studio      

**Step 4 :**  Configure linked service type as "filesystem" (to read the local folder data)  

**Step 5 :**  Create pipeline source dataset by selecting the link and sink dataset as datalake (note sink dataset should be configured using azure integration runtime)    

    <img width="1340" height="787" alt="image" src="https://github.com/user-attachments/assets/1de8884c-1774-45f9-a334-0ffc876994eb" />


#### Intragration runtime settting:  
* if file access issue `cd "C:\Program Files\Microsoft Integration Runtime\5.0\Shared"`  
* run `./dmgcmd -DisableLocalFolderPathValidation`  

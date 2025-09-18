# azure-cloud-infra

## 1. Azure Cloud Core Concepts
Azure is Microsoft’s cloud computing platform offering Infrastructure as a Service (IaaS),
Platform as a Service (PaaS), and Software as a Service (SaaS), Also CaaS (Container as Service). It provides scalable, pay-as-you-
go solutions for computing, storage, networking, databases, machine learning, analytics, and
many more.

# Traditional On-Premises vs. Cloud Computing: Key Differences

### 1. Infrastructure Ownership
  * On-Premises: Organizations own, manage, and maintain all hardware and software on-site. This includes physical servers, networking equipment, and data centers.
  * Cloud Computing (e.g., Microsoft Azure): Infrastructure is hosted and managed by a cloud provider. Users access resources over the internet on a subscription or pay-as-you-go basis.

### 2. Scalability
  * On-Premises: Scaling requires purchasing and installing new hardware, which is time-consuming and expensive. Overprovisioning is common to meet future demand.
  * Azure Cloud: Instantly scale resources up or down based on demand. Whether running a small test application or a global enterprise system, Azure provides automatic and elastic scalability without overinvesting.

### 3. Global Reach
  * On-Premises: Geographic expansion means setting up new data centers in different locations, which is costly and complex.
  * Azure Cloud: Azure has a vast global network of data centers in over 60 regions worldwide. Applications can be deployed closer to users, improving performance and meeting data residency requirements.

### 4. Cost-Efficiency
  * On-Premises: High upfront **capital expenditure** for servers, facilities, and IT staff. Costs are often fixed regardless of usage.
  * Azure Cloud: Operates on a pay-as-you-go or subscription-based model, converting CapEx to operational expenditure (OpEx). You pay only for what you use, reducing waste and increasing financial flexibility.

## Summary of Azure Advantages:
  * Scalability: Rapid, on-demand scaling without hardware limitations.
  * Global Reach: Presence in data centers worldwide for high availability and low latency.
  * Cost-Efficiency: Lower upfront costs, pay-as-you-go pricing, and better resource utilization.

## Azure Hosting Models
* Infrastructure as a Service (IaaS):
  Provides virtual machines, storage, and networking. Users have full control over the operating system, applications, and configurations.

* Platform as a Service (PaaS):
  Offers a ready-to-use platform to develop, test, deploy, and manage applications without worrying about the underlying infrastructure.
  examples : Azure SQL Database, Azure Functions, logic App, WebApp, AKS, Keyvault

* Software as a Service (SaaS):
  Delivers fully managed software applications over the internet. Users simply access and use the software without managing any infrastructure or platform components.
  examples:  Office 365, Azure DevOps

## Azure Services
* Compute: Virtual Machines, App Services, Azure Kubernetes Service.
* Storage: Blob, Table, Queue, File Storage.
* Networking: VNet, Load Balancer, ExpressRoute.
* AI & Machine Learning, DevOps, Security.

## Subscribing to Microsoft Azure [How do I start using Azure Services]
  ### What You Need First:
  * A Microsoft account (e.g., Outlook, Hotmail, or any email registered with Microsoft)
  * A valid credit/debit card (for identity verification)
  * A phone number (for security/identity verification)
  
  ### Steps to Create an Azure Subscription:
   Step 1: Go to the Azure Website  https://azure.microsoft.com/en-us/pricing/purchase-options/azure-account/
    Click on “Start free” (to use the free trial) or “Sign in” if you already have a Microsoft account.
  
   Step 2: Sign In or Create a Microsoft Account
    If you don’t have one, click “Create one!” and follow the steps.
    If you have one, just sign in.
   
   Step 3: Start the Free Trial (Optional)

### New users are eligible for:
 $200 USD credit for the first 30 days. Free limited services for 12 months (like Linux/Windows VMs, Storage, etc.)

## Choose Subscription Type
### Subscription Type	Description
  * Free Trial	$200 credit for 30 days + free services for 12 months
  * Pay-As-You-Go	Only pay for what you use, no upfront cost
  * Student Subscription	Free credits for students (no card required) – Azure for Students
  * Enterprise Agreement	For large businesses – negotiated pricing
  * Microsoft Customer Agreement (MCA)	Default for most organizations

### Ways to login in Azure 
  * **Azure portal** - https://portal.azure.com
  
  * **Azure CLI (az cli )** - with GUI prompt `az login`
    * To set a subscription - `az account set --subscription "<subscription_id>"`
  
  * **PowerShell** -
    * To install AZ module `Install-Module -Name Az -AllowClobber -Scope CurrentUser`
    * Import AZ module `Import-Module Az`
    * Login to Azure `Connect-AzAccount`
  
  * **Azure Cloud shell**


### Download az cli here -
  * (Windows) `https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-windows?view=azure-cli-latest&pivots=winget`
  * (Mac) `brew update && brew install azure-cli`

#### Create a resource group using portal and Az CLI or Terraform 
* Resource group Name : rg-shared-demo-001 (using portal)
* Resource group Name : rg-shared-demo-002 (using az cli)
* Resource group Name : rg-shared-demo-003 (using terraform)

## 2. Storage
Azure Storage is a highly scalable, secure, and durable cloud storage solution for both structured and unstructured data. 
It supports multiple storage types to meet a wide range of data and application requirements.

### Type of Storage
* Blob Storage: For unstructured data like images, videos, backups, and documents.
  * Access Tiers: **Hot**: Frequent access, **Cool**: Infrequent access, **Archive**: Rarely accessed data with long retrieval times.
  * Security: Use SAS tokens for fine-grained, time-limited access.
    
* File Storage: Fully managed shared file systems using SMB protocol.
   example : Shared file systems in case of  enterprise app migration or in operational requirement for having shared storage.
  
* Table Storage: NoSQL key-value store for structured, non-relational data.
* Queue Storage: Message storage for decoupled communication between components.

| Tier         | Description                                           | Use Cases                           |
| ------------ | ----------------------------------------------------- | ----------------------------------- |
| **Standard** | Backed by HDDs; cost-effective, good for general use  | Backup, archives, infrequent access |
| **Premium**  | Backed by SSDs; low-latency, high-performance storage | High IOPS workloads, databases      |

| SKU Name    | Replication Type               | Description                                        |
| ----------- | ------------------------------ | -------------------------------------------------- |
| **LRS**     | Locally Redundant Storage      | 3 copies within a single data center in a region   |
| **ZRS**     | Zone-Redundant Storage         | 3 copies across **availability zones** in a region |
| **GRS**     | Geo-Redundant Storage          | 3 copies in primary + 3 in paired region           |
| **GZRS**    | Geo-Zone-Redundant Storage     | ZRS in primary + geo-replication to paired region  |
| **RA-GRS**  | Read-Access Geo-Redundant      | GRS with **read access** to the secondary region   |
| **RA-GZRS** | Read-Access Geo-Zone Redundant | GZRS with read access to the secondary region      |

| Feature                   | LRS               | ZRS               | GRS                                |
| ------------------------- | ----------------- | ----------------- | --------------------------------   |
| Redundancy Scope          | Single zone       | Across 3 zones    | Across 2 regions                   |
| Number of Copies          | 3                 | 3                 | 6 (3 primary + 3 secondary)        |
| Cross-Region Protection   | ❌ No              | ❌ No              | ✅ Yes                            |
| Zone Failure Protection   | ❌ No              | ✅ Yes             | ✅ Yes (in secondary)             |
| Region Failure Protection | ❌ No              | ❌ No              | ✅ Yes                            |
| Read Access to Secondary  | ❌ No              | ❌ No              | ❌ No (unless RA-GRS)             |
| Use Cases                 | Dev/test, backups | App data, content | DR, regulatory, critical backups |

**Azure regions** : https://learn.microsoft.com/en-us/azure/reliability/regions-list

### Management of Azure Storage Tier : 

<img width="4092" height="6248" alt="image" src="https://github.com/user-attachments/assets/80c33e48-a172-4aac-a4fc-649d820c271b" />


#### Create a storage account using portal and Az CLI or Terraform 
* Storage account : sashareddemo001 (using portal)
* Storage account : sashareddemo002 (using az cli)  
  `az storage account create --name sashareddemo002 --resource-group rg-shared-demo-002 --location eastus --sku Standard_LRS --kind StorageV2 --access-tier Hot`  
* Storage account : sa-shared-demo-003 (using terraform) --optional

#### Upload a file to Azure storage blob using a .NET simple program 
Step 1 : Create a project using console  
`dotnet new console -n BlobUploader
cd BlobUploader
`  
Step 2 : install SDK package for blobs  
`dotnet add package Azure.Storage.Blobs`  

Step 3 : replace the code in Program.cs as below  

<pre> 
using Azure.Storage.Blobs;  
using System;  
using System.IO;  
using System.Threading.Tasks;  
  
class Program  
{  
    static async Task Main(string[] args)  
    {  
        // Replace with your values  
        string connectionString = "<your-storage-connection-string>";
        string containerName = "mycontainer";
        string blobName = "myfile.txt";
        string localFilePath = @"/Users/yourname/Downloads/myfile.txt"; // Adjust this

        // Create BlobServiceClient
        BlobServiceClient blobServiceClient = new BlobServiceClient(connectionString);

        // Get container and create if it doesn't exist
        BlobContainerClient containerClient = blobServiceClient.GetBlobContainerClient(containerName);
        await containerClient.CreateIfNotExistsAsync();

        // Get BlobClient
        BlobClient blobClient = containerClient.GetBlobClient(blobName);

        // Upload file
        using FileStream uploadFileStream = File.OpenRead(localFilePath);
        await blobClient.UploadAsync(uploadFileStream, overwrite: true);
        uploadFileStream.Close();

        Console.WriteLine($"✅ File uploaded to: {blobClient.Uri}");
    }
}
</pre>





Step 4: run `dotnet run`

  
  

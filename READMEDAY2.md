## Azure Networking 
Azure Networking provides a robust, flexible, and secure foundation for building cloud-based applications and services. At the core of Azure networking are Virtual Networks (VNets) — logically isolated networks within the Azure cloud that enable you to securely connect Azure resources to each other, the internet, and on-premises networks.

#### Key Features of Azure Virtual Networks (VNets)
* Isolation & Segmentation: VNets provide a dedicated and isolated network boundary for your Azure resources, ensuring that your cloud environment is secure and segmented from other tenants.
* Secure Communication: VNets enable you to control inbound and outbound traffic through features such as Network Security Groups (NSGs), Application Security Groups, and firewall integration.
* Hybrid Connectivity: Azure VNets can be extended to your on-premises network using VPN Gateway or Azure ExpressRoute, enabling hybrid cloud architectures with seamless connectivity.

| Component                        | Description                                                                                         |
| -------------------------------- | --------------------------------------------------------------------------------------------------- |
| **Network Interface Card (NIC)** | Virtual network interface that connects a VM or resource to a subnet within a VNet.                 |
| **Network Security Group (NSG)** | Acts as a virtual firewall controlling inbound and outbound traffic rules at subnet or NIC level.   |
| **User Defined Routes (UDR)**    | Custom routing tables that direct traffic flow within or outside your VNet based on defined routes. |
| **VPN Gateway**                  | Provides secure, encrypted connectivity between Azure VNets and on-premises networks.               |

### Creating a VNet and Subnet
You can create VNets and subnets through the Azure Portal ,CLI or using PowerShell and Terraform/ARM for automation.
Portal : Navigate using Azure portal  

#### Azure CLI Vnet creation :  
`az network vnet create --name vnet-shared-clidemo-001 --resource-group rg-shared-demo-001 --location "East US" --address-prefix 10.190.189.0/22`   

##### Create AppSubnet
`az network vnet subnet create --resource-group rg-shared-demo-001 --vnet-name vnet-shared-clidemo-001 --name appsubnet --address-prefix 10.190.189.0/24`

##### Create DatabaseSubnet
`az network vnet subnet create --resource-group rg-shared-demo-001 --vnet-name vnet-shared-clidemo-001 --name sqldbsubnet --address-prefix 10.190.191.0/24`

#### PowerShell Vnet Creation :   
` New-AzVirtualNetwork  -Name "vnet-shared-pshelldemo-001" -ResourceGroupName "rg-shared-demo-001" -Location "East US" -AddressPrefix "10.189.190.0/22" `

#### Create AppSubnet
`Add-AzVirtualNetworkSubnetConfig -Name "appsubnet" -AddressPrefix "10.189.190.0/24" -VirtualNetwork (Get-AzVirtualNetwork -Name "vnet-shared-pshelldemo-001" -ResourceGroupName "rg-shared-demo-001") | Set-AzVirtualNetwork`

#### Create DatabaseSubnet
`Add-AzVirtualNetworkSubnetConfig -Name "sqldbsubnet" -AddressPrefix "10.189.191.0/24" -VirtualNetwork (Get-AzVirtualNetwork -Name "vnet-shared-pshelldemo-001" -ResourceGroupName "rg-shared-demo-001") | Set-AzVirtualNetwork`

### Creating Network Security Groups (NSGs) and Network Interfaces (NICs)

**NSGs:** Define granular security rules to permit or deny traffic to resources within the VNet. You can associate NSGs with subnets or individual NICs.

**NICs:** Attach NICs to virtual machines to connect them to specific subnets in your VNet.

### Managing Public IPs and DNS

**Public IP Addresses:** Azure supports both static and dynamic public IP allocation, enabling external connectivity to Azure resources.

**DNS Zones:** Azure DNS lets you create custom domain names and manage DNS zones for your resources, allowing for easy domain name resolution.

### Virtual Machines (VMs)
#### Introduction to Infrastructure as a Service (IaaS)

Azure IaaS allows you to provision fully customizable virtual machines (VMs) in the cloud. With Azure VMs, you gain complete control over the operating system, installed applications, networking, and storage, just as you would with a physical server — but with the flexibility, scalability, and high availability of the cloud.

| Feature         | Azure VM                                 | Web App (PaaS)             | Azure Function (Serverless)   |
| --------------- | ---------------------------------------- | -------------------------- | ----------------------------- |
| **Control**     | Full control over OS, apps, storage      | Limited (platform-managed) | Minimal (code only)           |
| **Scalability** | Manual or via VM Scale Sets              | Automatic scaling          | Event-driven auto-scaling     |
| **Cost**        | Higher (you manage the full stack)       | Moderate                   | Low (pay-per-execution)       |
| **Use Case**    | Legacy apps, custom setups, full control | Web hosting, APIs          | Lightweight tasks, automation |

#### Creating a Windows VM
 * You can deploy Windows VMs through multiple interfaces:
   * Azure Portal: GUI-based, best for first-time users.
   * Azure PowerShell: Ideal for scripting and automation.
   * Azure CLI: Cross-platform command-line tool for DevOps workflows.
     
#### Deploy from a Custom Image
 Use the captured image to create identical VMs (e.g., pre-installed applications or baseline configurations).

#### Availability Sets for High Availability

Azure Availability Sets are used to increase resilience and uptime:
* Fault Domains	Physical hardware separation (power, network)
* Update Domains	Logical separation during patching and maintenance

##### Fault Domains (FDs)
Represent physical separation of resources in the data center.

Each Fault Domain is isolated in terms of:  
 Power supply  
 Cooling  
 Network switches  

If a hardware failure occurs in one FD (e.g., power outage), VMs in other FDs remain unaffected.
Azure typically offers 2 or 3 fault domains depending on the region.VMs in an availability set are spread across multiple domains to avoid simultaneous downtime.

#### Update Domains (UDs)

Represent logical groupings of VMs that Azure updates independently during maintenance events.
When Azure performs planned updates (like OS patching or host updates), it only restarts one UD at a time.
This ensures at least a portion of your service remains available.
Azure provides up to 20 update domains per availability set.

### Imagine you deployed 6 VMs in an Availability Set with 3 Fault Domains and 2 Update Domains:
| Availability Set            |


| FD 0         | FD 1            | FD 2          |
| ------------ | -------------   |  -------------|
|  UD 0        | UD 1            | UD 0          |
| VM1          | VM2             | VM3           |
|  UD 1        | UD 0            | UD 1          |
| VM4          | VM5             | VM6          |


#### Conclusion :   
* Fault Domains (FD 0–2): Ensure that the VMs are physically isolated from one another.
* Update Domains (UD 0–1): Ensure that VMs are not rebooted at the same time during planned maintenance.


<img width="3052" height="1936" alt="image" src="https://github.com/user-attachments/assets/106d4206-0175-4bc0-8f72-9a2dd265c6a7" />



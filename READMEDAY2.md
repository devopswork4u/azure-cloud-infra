## Azure Networking 
Azure Networking provides a robust, flexible, and secure foundation for building cloud-based applications and services. At the core of Azure networking are Virtual Networks (VNets) — logically isolated networks within the Azure cloud that enable you to securely connect Azure resources to each other, the internet, and on-premises networks.

### Key Features of Azure Virtual Networks (VNets)
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

 

---
lab:
    title: 'Manage Azure resources by Using Azure PowerShell'
    module: 'Module - Azure Administration'
---

# Lab - Manage Azure resources by Using Azure PowerShell
# Student lab manual

## Lab scenario

Now that you explored the basic Azure administration capabilities associated with provisioning resources and organizing them based on resource groups by using the Azure portal and Azure Resource Manager templates, you need to carry out the equivalent task by using Azure PowerShell. To avoid installing Azure PowerShell modules, you will leverage PowerShell environment available in Azure Cloud Shell.

## Objectives

In this lab, you will:

+ Task 1: Start a PowerShell session in Azure Cloud Shell
+ Task 2: Create a resource group and an Azure managed disk by using Azure PowerShell
+ Task 3: Configure the managed disk by using Azure PowerShell

## Estimated timing: 20 minutes

## Instructions

### Exercise 1

#### Task 1: Start a PowerShell session in Azure Cloud Shell

In this task, you will open a PowerShell session in Cloud Shell. 

1. In the portal, open the **Azure Cloud Shell** by clicking on the icon in the top right of the Azure Portal.

1. If prompted to select either **Bash** or **PowerShell**, select **PowerShell**. 

    >**Note**: If this is the first time you are starting **Cloud Shell** and you are presented with the **You have no storage mounted** message, select the subscription you are using in this lab, and click **Create storage**. 

1. If prompted, click **Create storage**, and wait until the Azure Cloud Shell pane is displayed. 

1. Ensure **PowerShell** appears in the drop-down menu in the upper-left corner of the Cloud Shell pane.

#### Task 2: Create a resource group and an Azure managed disk by using Azure PowerShell

In this task, you will create a resource group and an Azure managed disk by using Azure PowerShell session within Cloud Shell

1. To create a resource group in the same Azure region as the **az104-03b-rg1** resource group you created in the previous lab, from the PowerShell session within Cloud Shell, run the following:

   ```pwsh
   $location = (Get-AzResourceGroup -Name az104-03b-rg1).Location

   $rgName = 'az104-03c-rg1'

   New-AzResourceGroup -Name $rgName -Location $location
   ```
1. To retrieve properties of the newly created resource group, run the following:

   ```pwsh
   Get-AzResourceGroup -Name $rgName
   ```
1. To create a new managed disk with the same characteristics as those you created in the previous labs of this module, run the following:

   ```pwsh
   $diskConfig = New-AzDiskConfig `
    -Location $location `
    -CreateOption Empty `
    -DiskSizeGB 32 `
    -Sku Standard_LRS

   $diskName = 'az104-03c-disk1'

   New-AzDisk `
    -ResourceGroupName $rgName `
    -DiskName $diskName `
    -Disk $diskConfig
   ```

1. To retrieve properties of the newly created disk, run the following:

   ```pwsh
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

#### Task 3: Configure the managed disk by using Azure PowerShell

In this task, you will managing configuration of the Azure managed disk by using Azure PowerShell session within Cloud Shell. 

1. To increase the size of the Azure managed disk to **64 GB**, from the PowerShell session within Cloud Shell, run the following:

   ```pwsh
   New-AzDiskUpdateConfig -DiskSizeGB 64 | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. To verify that the change took effect, run the following:

   ```pwsh
   Get-AzDisk -ResourceGroupName $rgName -Name $diskName
   ```

1. To verify the current SKU as **Standard_LRS**, run the following:

   ```pwsh
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

1. To change the disk performance SKU to **Premium_LRS**, from the PowerShell session within Cloud Shell, run the following:

   ```pwsh
   New-AzDiskUpdateConfig -Sku Premium_LRS | Update-AzDisk -ResourceGroupName $rgName -DiskName $diskName
   ```

1. To verify that the change took effect, run the following:

   ```pwsh
   (Get-AzDisk -ResourceGroupName $rgName -Name $diskName).Sku
   ```

#### Clean up resources

#### Clean up resources

   >**Note**: Remember to remove any newly created Azure resources that you no longer use. Removing unused resources ensures you will not see unexpected charges.

1. In the Azure portal, open the **PowerShell** session within the **Cloud Shell** pane.

1. List all resource groups created throughout the labs of this module by running the following command:

   ```pwsh
   Get-AzResourceGroup -Name 'az104-03*'
   ```

1. Delete all resource groups you created throughout the labs of this module by running the following command:

   ```pwsh
   Get-AzResourceGroup -Name 'az104-03*' | Remove-AzResourceGroup -Force -AsJob
   ```

    >**Note**: The command executes asynchronously (as determined by the -AsJob parameter), so while you will be able to run another PowerShell command immediately afterwards within the same PowerShell session, it will take a few minutes before the resource groups are actually removed.

#### Review

In this lab, you have:

- Started a PowerShell session in Azure Cloud Shell
- Created a resource group and an Azure managed disk by using Azure PowerShell
- Configured the managed disk by using Azure PowerShell

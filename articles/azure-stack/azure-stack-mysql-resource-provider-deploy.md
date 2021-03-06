---
title: Use MySQL databases as PaaS on Azure Stack | Microsoft Docs
description: Learn how you can deploy the MySQL Resource Provider and provide MySQL databases as a service on Azure Stack
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: JeffGo
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 4e9da524ef9dfa2d5b7150bc6a888536a1435dfd
ms.contentlocale: pt-br
ms.lasthandoff: 07/11/2017


---

# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Use MySQL databases on Microsoft Azure Stack


You can deploy a MySQL resource provider on Azure Stack. After you deploy the resource provider, you can create MySQL servers and databases through Azure Resource Manager deployment templates and provide MySQL databases as a service. MySQL databases, which are common on web sites, support many website platforms. As an example, after you deploy the resource provider, you can create WordPress websites from the Azure Web Apps platform as a service (PaaS) add-on for Azure Stack.

To deploy the MySQL provider on a system that does not have internet access, you can copy the file [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.9.9.msi) to a local share and provide that share name when prompted (see below). You must also install the Azure and Azure Stack PowerShell modules.


## <a name="mysql-server-resource-provider-adapter-architecture"></a>MySQL Server Resource Provider Adapter architecture

The resource provider is made up of three components:

- **The MySQL resource provider adapter VM**, which is a Windows virtual machine running the provider services.
- **The resource provider itself**, which processes provisioning requests and exposes database resources.
- **Servers that host MySQL Server**, which provide capacity for databases, called Hosting Servers. 

This release no longer creates a MySQL instance. You must create them and/or provide access to external SQL instances. You can visit the [Azure Stack Quickstart Gallery](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/mysql-standalone-server-windows) for an example template that can create a MySQL server for you or download and deploy a MySQL Server from the Marketplace.

## <a name="deploy-the-resource-provider"></a>Deploy the resource provider

1. If you have not already done so, register your development kit and download the Windows Server 2016 Datacenter - Eval image downloadable through Marketplace Management. You can also use a script to create a [Windows Server 2016 image](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image).

2. [Download the MySQL resource provider binaries file](https://aka.ms/azurestackmysqlrp) and extract it on the development kit host.

3. Sign in to the development kit host, and extract the MySQL RP installer file to a temporary directory.

4. The Azure Stack root certificate is retrieved and a self-signed certificate is created as part of this process. 

    __Optional:__ If you need to provide your own, prepare the certificates and copy to a local directory if you wish to customize the certificates (passed to the installation script). You need the following:

    a. A wildcard certificate for *.dbadapter.\<region\>.\<external fqdn\>. This certificate must be trusted, such as would be issued by a certificate authority (that is, the chain of trust must exist without requiring intermediate certificates.) (A single site certificate can be used with the explicit VM name you provide during install.)

    b. The root certificate used by the Azure Resource Manager for your instance of Azure Stack. If it is not found, the root certificate will be retrieved.

5. Open a **new** elevated PowerShell console and change to the directory where you extracted the files. Use a new window to avoid problems that may arise from incorrect PowerShell modules already loaded on the system.

6. If you have installed any versions of the AzureRm or AzureStack PowerShell modules other than 1.2.9 or 1.2.10, you will be prompted to remove them or the install will not proceed. This includes versions 1.3 or greater.

7. Run DeployMySqlProvider.ps1.

This script performs these steps:

* If necessary, download a compatible version of Azure PowerShell.
* Download the MySQL connector binary (this can be provided offline).
* Upload the certificate and all other artifacts to an Azure Stack storage account.
* Publish gallery packages so that you can deploy MySQL databases through the gallery.
* Deploy a virtual machine (VM) that hosts your resource provider.
* Register a local DNS record that maps to your resource provider VM.
* Register your resource provider with the local Azure Resource Manager.

Either specify at least the required parameters on the command line, or, if you run without any parameters, you are prompted to enter them. 

Here's an example you can run from the PowerShell prompt (but change the account information and portal endpoints as needed):


```
# Install the AzureRM.Bootstrapper module
Install-Module -Name AzureRm.BootStrapper -Force

# Installs and imports the API Version Profile required by Azure Stack into the current PowerShell session.
Use-AzureRmProfile -Profile 2017-03-09-profile
Install-Module -Name AzureStack -RequiredVersion 1.2.10 -Force

# Download the Azure Stack Tools from GitHub and set the environment
cd c:\
Invoke-Webrequest https://github.com/Azure/AzureStack-Tools/archive/master.zip -OutFile master.zip
Expand-Archive master.zip -DestinationPath . -Force

# This endpoint may be different for your installation
Import-Module C:\AzureStack-Tools-master\Connect\AzureStack.Connect.psm1
Add-AzureRmEnvironment -Name AzureStackAdmin -ArmEndpoint "https://adminmanagement.local.azurestack.external" 

# For AAD, use the following
$tenantID = Get-AzsDirectoryTenantID -AADTenantName "<your directory name>" -EnvironmentName AzureStackAdmin

# For ADFS, replace the previous line with
# $tenantID = Get-AzsDirectoryTenantID -ADFS -EnvironmentName AzureStackAdmin

$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ("admin@mydomain.onmicrosoft.com", $AdminPass)

# change this as appropriate
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files 
# and adjust the endpoints
<extracted file directory>\DeployMySQLProvider.ps1 -DirectoryTenantID $tenantID -AzCredential $AdminCreds -VMLocalCredential $vmLocalAdminCreds -ResourceGroupName "MySqlRG" -VmName "MySQLRP" -ArmEndpoint "https://adminmanagement.local.azurestack.external" -TenantArmEndpoint "https://management.local.azurestack.external" -DefaultSSLCertificatePassword $PfxPass -DependencyFilesLocalPath
 ```

### <a name="deploymysqlproviderps1-parameters"></a>DeployMySqlProvider.ps1 parameters

You can specify these parameters in the command line. If you do not, or any parameter validation fails, you are prompted to provide the required ones.

| Parameter Name | Description | Comment or Default Value |
| --- | --- | --- |
| **DirectoryTenantID** | The Azure or ADFS Directory ID (guid) | _required_ |
| **ArmEndpoint** | The Azure Stack Administrative Azure Resource Manager Endpoint | _required_ |
| **TenantArmEndpoint** | The Azure Stack Tenant Azure Resource Manager Endpoint | _required_ |
| **AzCredential** | Azure Stack Service Admin account credential (use the same account as you used for deploying Azure Stack) | _required_ |
| **VMLocalCredential** | The local administrator account of the MySQL resource provider VM | _required_ |
| **ResourceGroupName** | Resource Group for the items created by this script |  _required_ |
| **VmName** | Name of the VM holding the resource provider |  _required_ |
| **AcceptLicense** | Skips the prompt to accept the GPL License  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | |
| **DependencyFilesLocalPath** | Path to a local share containing [mysql-connector-net-6.9.9.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.9.9.msi). If you provide them, certificate files must be placed in this directory as well. | _optional_ |
| **DefaultSSLCertificatePassword** | The password for the .pfx certificate | _required_ |
| **MaxRetryCount** | Each operation is retried if there is a failure | 2 |
| **RetryDuration** | Timeout between retries, in seconds | 120 |
| **Uninstall** | Remove the resource provider | No |
| **DebugMode** | Prevents automatic cleanup on failure | No |


Depending on the system performance and download speeds, installation may take as little as 20 minutes or as long as several hours. You must refresh the admin portal if the MySQLAdapter blade is not available.

> [!NOTE]
> If the installation takes more than 90 minutes, it may fail and you will see a failure message on the screen and in the log file. The deployment is retried from the failing step. Systems that do not meet the recommended memory and core specifications may not be able to deploy the MySQL RP.


## <a name="provide-capacity-by-connecting-to-a-mysql-hosting-server"></a>Provide capacity by connecting to a MySQL hosting server

1. Sign in to the Azure Stack portal as a service admin.

2. Click **Resource Providers** &gt; **MySQLAdapter** &gt; **Hosting Servers** &gt; **+Add**.

    The **MySQL Hosting Servers** blade is where you can connect the MySQL Server Resource Provider to actual instances of MySQL Server that serve as the resource provider’s backend.

    ![Hosting Servers](./media/azure-stack-mysql-rp-deploy/mysql-add-hosting-server-2.png)

3. Fill the form with the connection details of your MySQL Server instance. Provide the fully qualified domain name (FQDN) or a valid IPv4 address, and not the short VM name. This installation no longer provides a default MySQL instance. The size provided helps the resource provider manage the database capacity. It should be close to the physical capacity of the database server.

    > [!NOTE]
    > As long as the MySQL instance can be accessed by the tenant and admin Azure Resource Manager, it can be placed under control of the resource provider. The MySQL instance __must__ be allocated exclusively to the RP.

4. As you add servers, you must assign them to a new or existing SKU to allow differentiation of service offerings. For example, you could have an enterprise instance providing database capacity and automatic backup, reserve high-performance servers for individual departments, etc. The SKU name should reflect the properties so that tenants can place their databases appropriately and all hosting servers in a SKU should have the same capabilities.

    ![Create a MySQL SKU](./media/azure-stack-mysql-rp-deploy/mysql-new-sku.png)


>[!NOTE]
SKUs can take up to an hour to be visible in the portal. You cannot create a database until the SKU is created.


## <a name="create-your-first-mysql-database-to-test-your-deployment"></a>Create your first MySQL database to test your deployment


1. Sign in to the Azure Stack portal as service admin.

2. Click the **+ New** button &gt; **Data + Storage** &gt; **MySQL Database (preview)**.

3. Fill in the form with the database details.

    ![Create a test MySQL database](./media/azure-stack-mysql-rp-deploy/mysql-create-db.png)

4. Select a SKU.

    ![Select a SKU](./media/azure-stack-mysql-rp-deploy/mysql-select-a-sku.png)

5. Create a login setting. The login setting can be reused or a new one created. This contains the user name and password for the database.

    ![Create a new database login](./media/azure-stack-mysql-rp-deploy/create-new-login.png)

    The connections string includes the real database server name. Copy it from the portal.

    ![Get the connection string for the MySQL database](./media/azure-stack-mysql-rp-deploy/mysql-db-created.png)

> [!NOTE]
> The length of the user names cannot exceed 32 characters with MySQL 5.7 or 16 characters in earlier editions. This is a limitation of the MySQL implementations.


## <a name="add-capacity"></a>Add capacity

Add capacity by adding additional MySQL servers in the Azure Stack portal. If you wish to use another instance of MySQL, click **Resource Providers** &gt; **MySQLAdapter** &gt; **MySQL Hosting Servers** &gt; **+Add**.


## <a name="making-mysql-databases-available-to-tenants"></a>Making MySQL databases available to tenants
Create plans and offers to make MySQL databases available for tenants. Add the Microsoft.MySqlAdapter service, add a quota, etc.

![Create plans and offers to include databases](./media/azure-stack-mysql-rp-deploy/mysql-new-plan.png)

## <a name="removing-the-mysql-adapter-resource-provider"></a>Removing the MySQL Adapter Resource Provider

To remove the resource provider, it is essential to first remove any dependencies.

1. Ensure you have the original deployment package that you downloaded for this version of the Resource Provider.

2. All tenant databases must be deleted from the resource provider (this will not delete the data). This should be performed by the tenants themselves.

3. Tenants must unregister from the namespace.

4. Administrator must delete the hosting servers from the MySQL Adapter

5. Administrator must delete any plans that reference the MySQL Adapter.

6. Administrator must delete any quotas associated to the MySQL Adapter.

7. Rerun the deployment script with the -Uninstall parameter, Azure Resource Manager endpoints, DirectoryTenantID, and credentials for the service administrator account.




## <a name="next-steps"></a>Next steps


Try other [PaaS services](azure-stack-tools-paas-services.md) like the [SQL Server resource provider](azure-stack-sql-resource-provider-deploy.md) and the [App Services resource provider](azure-stack-app-service-overview.md).


# Deploying Azure Infrastructure on Windows Server Core

## Introduction

In this lab, you will use remote access (RDP) to log in to a Windows Server Core VM and install Azure command-line tools in order to deploy basic infrastructure (i.e., a storage account).

## Solution

Log in to the Azure portal using the credentials provided on the lab page. Be sure to use an incognito or private browser window to ensure you're using the lab account rather than your own.

### Remote (RDP) in to Windows Virtual Machine

Use the Remote Desktop client (available from Microsoft for Windows clients natively and [Mac clients here](https://docs.microsoft.com/en-us/windows-server/remote/remote-desktop-services/clients/remote-desktop-mac)).

#### Remote Desktop Client for Windows

1. In the Azure portal, click **Virtual machines** in the left-hand menu.
1. Click the listed **lab-vm** virtual machine.
1. Click **Connect** > **RDP**.
1. Click **Download RDP File**.
1. Open the file in your Remote Desktop app.
1. Edit the desktop to:
    - Make sure you are _not_ in an admin session.
    - Make sure the remote session does _not_ launch in full-screen mode.
    - Set the resolution to something that will give you room to maneuver in both the remote and local desktops (e.g., 1024 x 768).
 1. Log in to the virtual machine using the `azlab-VM` credentials provided on the lab page.

> **Note**: If you're using a Mac, instead of downloading the RDP file, you will need to copy the public IP address of the virtual machine in the Azure portal and use that to access the virtual machine via your RDP client with the steps below.

#### Remote Desktop Client for Mac

1. Open your Remote Desktop app.
1. Click **Add PC**.
1. Under **PC name**, enter the public IP address of the virtual machine in the Azure portal.
1. Under **User account**, ensure **Ask when required** is selected.
1. Under **Friendly name**, enter a name for the virtual machine.
1. Click **Add**.
1. Click on the virtual machine to remote in.
1. Log in to the virtual machine using the `azlab-VM` credentials provided on the lab page.
1. On the pop-up informing you about connecting to the RDP host, click **Continue**.
1. Next to `Enter number to select an option`, enter `15` to exit `SConfig` and open the command line (PowerShell).
1. In the upper right corner, click on the **Maximize** icon to expand the window.

### Install Deployment Tools (Azure PowerShell Recommended)

1. Check to see whether the Azure CLI is installed:
   ```
   az
   ```

1. Check to see whether the Az PowerShell module is installed:
   ```
   Get-Module -Name "Az*"
   ```

1. Install the Azure `Az.Storage` submodule:
   ```
   Install-Module -Name Az.Storage -Force
   ```

1. When asked whether or not to import the NuGet provider, enter `Y`.

1. Clear your screen:
   ```
   clear
   ```

1. Confirm `Az.Storage` is installed:
   ```
   Get-Command -Name "*AzStorage*"
   ```

1. Look for the new `Az.Storage` account:
   ```
   Get-Help New-AzStorageAccount -Examples
   ```

1. Clear your screen:
   ```
   clear
   ```

### Connect to Azure

1. Connect to the Azure tenant:
   ```
   Connect-AzAccount -DeviceCode
   ```

1. Copy the URL provided and paste it in a new incognito tab on the same browser as the Azure portal.
1. On the **Enter code** pop-up, enter the code provided from the previous command to authenticate.
1. On the **Pick an account** pop-up, select **Cloud Student**.
1. When asked if you're trying to sign in to the Microsoft Azure PowerShell, click **Continue**.
1. Once you receive the message stating you successfully signed in, close the window.
1. Navigate back to the terminal. You should see that you successfully logged in to the Azure tenant.

### Deploy Basic Infrastructure Using CLI

1. Use the `Get-Help` module to ensure you know the switches you will need:
   ```
   Get-Help New-AzStorageAccount
   ```

1. Clear your screen:
   ```
   clear
   ```

1. Deploy the storage account:
   ```
   New-AzStorageAccount -ResourceGroupName <INSERT_RESOURCE_GROUP_NAME> -Name <GLOBALLY_UNIQUE_STORAGE_ACCOUNT_NAME> -Location <DEFAULT_REGION> -SkuName Standard_LRS
   ```

 > **Note:** To retrieve the name of the resource group, navigate back to the Azure portal, click on the resource name in the breadcrumb trail, and copy the name in the top-left corner above **Resource group**.

4. Navigate back to the Azure portal and click on the **Refresh** icon in the toolbar; it can take between 30-60 seconds for deployments to appear in the Azure portal after deploying from the command line.

## Conclusion

Congratulations — you've completed this hands-on lab!

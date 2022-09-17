---
lab:
    title: 'Lab: Implement failover clustering'
    module: 'Module 6: High availability in Windows Server'
---

# Lab: Implement failover clustering

## Scenario

Contoso Ltd. has many services and applications that must be available to internal and external users who work in different time zones around the world. As one of the senior network administrators at Contoso, you're responsible for implementing failover clustering on the servers that are running Windows Server 2022 to provide high availability for network services and applications. You're also responsible for planning the failover cluster configuration and deploying applications and services on the failover cluster.

## Objectives

After completing this lab, you'll be able to:

- Configure iSCSI storage.
- Configure a failover cluster.
- Deploy and configure a highly available file server
- Validate the deployment of the highly available file server.

## Estimated time: **60 minutes**

## Exercise 1: Configuring iSCSI storage

### Scenario

Contoso has applications and services that it needs to make highly available. You decide to use Internet SCSI (iSCSI) storage for failover clustering. First, you'll install failover clustering on LON-SVR2 and LON-SVR3. Next, you'll configure iSCSI storage to support your failover cluster.

The main tasks for this exercise are to:

- Install Failover Clustering.
- Configure iSCSI virtual disks.

### Task 1: Install Failover Clustering

1. Sign in to **LON-ADM1** as **Contoso\\Administrator** with the password of **Pa55w.rd**.

1. On **LON-ADM1**, select **Start**, and then select **Windows PowerShell ISE**. 

1. In the **Administrator: Windows PowerShell ISE** command prompt window, enter the following command, and then select Enter:

    ```powershell
    Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
    ```

    > This command installs the Failover Clustering feature on LON-ADM1. Wait until the installation process is complete and a command prompt appears.

1. On **LON-ADM1**, repeat step 2 to open a new PowerShell ISE session that you'll use to connect to the **LON-SVR2** server.

1. In the new **Administrator:Windows PowerShell ISE** window, enter the following command, and then select Enter:

    ```powershell
    $cred=Get-Credential
    ```

1. When prompted, provide the credentials as **Contoso\\Administrator** with password **Pa55w.rd**.

1. In the new **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

    ```powershell
    $sess = New-PSSession -Credential $cred -ComputerName lon-svr2.contoso.com
    ```

1. After running the previous command, enter the following command, and then select Enter:

   ```powershell
   Enter-PSSession $sess
   ```

1. Verify that **`lon-svr2.contoso.com`** appears at the beginning of the command prompt.

    > You are now connected to LON-SVR2 using PowerShell remoting.

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR2**, enter the following command, and then select Enter:

   ```powershell
   Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
   ```

1. On **LON-ADM1**, repeat step 2 to open a third PowerShell ISE session that you'll use to connect to the **LON-SVR3** server.

1. To install the Failover Clustering feature on **LON-SVR3**, in the third **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter.

     ```powershell
     $cred=Get-Credential
     ```

1. When prompted, provide the credentials as **Contoso\\Administrator** with password **Pa55w.rd**.

1. In the third **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

     ```powershell
        $sess = New-PSSession -Credential $cred -ComputerName lon-svr3.contoso.com
     ```

1. After running the previous command, enter the following command, and then select Enter:

    ```powershell
    Enter-PSSession $sess
    ```

1. Verify that **`lon-svr3.contoso.com`** appears at the beginning of the command prompt.

     > You are now also connected to LON-SVR3 by using PowerShell remoting.

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR3**, enter the following command, and then select Enter:

      ```powershell
      Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
      ```

1. In the **Administrator: Windows PowerShell ISE** window for **LON-ADM1**, enter the following command, and then select Enter:

      ```powershell
      Add-WindowsFeature FS-iSCSITarget-Server
      ```

      > This command installs the iSCSI Target Server role on LON-ADM1. Wait until the installation finishes and the command prompt returns.

      > Verify that the Failover clustering feature installation is complete on LON-SVR2 and LON-SVR3. If not, wait until the installation is complete and then continue with the following steps.

1. In the PowerShell window for **LON-SVR2**, enter the following command, and then select Enter:

     ```powershell
     Restart-Computer
     ```

1. In the PowerShell window for **LON-SVR3**, enter the following command, and then select Enter:

     ```powershell
     Restart-Computer
     ```

1. In the PowerShell window for **LON-ADM1**, enter the following command, and then select Enter:

     ```powershell
     Restart-Computer
     ```

     > Wait for 3–4 minutes for all three servers to restart before continuing with the next task.

### Task 2: Configure iSCSI virtual disks

1. Sign in to **LON-ADM1** as **Contoso\\Administrator** with password **Pa55w.rd**.

1. Select **Start**, and then select **Windows PowerShell ISE**.

1. In the **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

    ```powershell
    New-IscsiVirtualDisk c:\Storage\disk1.VHDX –size 10GB
    ```

1. In the **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

    ```powershell
    New-IscsiVirtualDisk c:\Storage\disk2.VHDX –size 10GB
    ```

1. In the **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

    ```powershell
    New-IscsiVirtualDisk c:\Storage\disk3.VHDX –size 10GB
    ```

1. To open another **Windows PowerShell ISE** window to connect to **LON-SVR2**, select **Start**, and then select **Windows PowerShell ISE**.

1. In the new **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

    ```powershell
    $cred=Get-Credential
    ```

1. When prompted, provide the credentials as **Contoso\\Administrator** with password **Pa55w.rd**.

1. In the new **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

     ```powershell
     $sess = New-PSSession -Credential $cred -ComputerName lon-svr2.contoso.com
     ```

1. After running the previous command, enter the following command, and then select Enter:

     ```powershell
     Enter-PSSession $sess
     ```

     > Verify that **`lon-svr2.contoso.com`** appears at the beginning of the command prompt. You are now connected to LON-SVR2 by using PowerShell remoting.

1. To open another **Windows PowerShell ISE** window to connect to **LON-SVR3**, select **Start**, and then select **Windows PowerShell ISE**.

1. In the third **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

     ```powershell
     $cred=Get-Credential
     ```

1. When prompted, provide the credentials as **Contoso\\Administrator** with password **Pa55w.rd**.

1. In the third **Administrator: Windows PowerShell ISE** window, enter the following command, and then select Enter:

     ```powershell
     $sess = New-PSSession -Credential $cred -ComputerName lon-svr3.contoso.com
     ```

1. After running the previous command, enter the following command, and then select Enter:

     ```powershell
     Enter-PSSession $sess
     ```

1. Verify that **`lon-svr3.contoso.com`** appears at the beginning of the command prompt.

     > **Note:** You should have three **Windows PowerShell ISE** windows opened. Ensure that you always use the proper PowerShell session window that's connected to the server where you want to run a command.

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR2**, start the Microsoft iSCSI Initiator service by entering the following commands, each followed by selecting Enter:

   ```powershell
   Start-Service msiscsi

   Set-Service msiscsi -startuptype "automatic"
   ```

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR3**, start the Microsoft iSCSI Initiator service by entering the following commands, each followed by selecting Enter:

   ```powershell
   Start-Service msiscsi

   Set-Service msiscsi -startuptype "automatic"
   ```

1. On **LON-ADM1**, in the **Administrator: Windows PowerShell ISE** window for the local machine, enter:

   ```powershell
   New-IscsiServerTarget iSCSI-MOD6 -InitiatorIds "IQN:iqn.1991-05.com.microsoft:lon-svr2.contoso.com","IQN:iqn.1991-05.com.microsoft:lon-svr3.contoso.com"
   ```

### Results

After completing this exercise, you successfully installed the Failover Clustering feature and configured the Internet SCSI (iSCSI) Target Server.

## Exercise 2: Configuring a failover cluster

### Scenario

In this exercise, you'll configure a failover cluster. You'll implement the core components for failover clustering. You'll validate the cluster and then create the failover cluster.

The main tasks for this exercise are to:

1. Connect clients to the iSCSI targets.
1. Initialize the disks.
1. Validate and create a failover cluster.

### Task 1: Connect clients to the iSCSI targets

1. In the PowerShell window for **LON-ADM1**, enter each of the following commands, each followed by selecting Enter:

    ```powershell
    Add-IscsiVirtualDiskTargetMapping iSCSI-MOD6 c:\Storage\Disk1.VHDX
    
    Add-IscsiVirtualDiskTargetMapping iSCSI-MOD6 c:\Storage\Disk2.VHDX
    
    Add-IscsiVirtualDiskTargetMapping iSCSI-MOD6 c:\Storage\Disk3.VHDX
    ```

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR2**, enter the following commands, and after each, select Enter:

    ```powershell
    New-iSCSITargetPortal –TargetPortalAddress LON-ADM1.contoso.com
    
    Connect-iSCSITarget –NodeAddress iqn.1991-05.com.microsoft:lon-adm1-iscsi-mod6-target
    
    Get-iSCSITarget | fl
    ```

1. Verify that after running the last command, the value for the *IsConnected* variable is True.

    > This validates the LON-SVR2 is now connected to the iSCSI Target server.

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR3**, enter the following commands, and then select Enter after each:

    ```powershell
    New-iSCSITargetPortal –TargetPortalAddress LON-ADM1.contoso.com
    
    Connect-iSCSITarget –NodeAddress iqn.1991-05.com.microsoft:lon-adm1-iscsi-mod6-target
    
    Get-iSCSITarget | fl
    ```

1. Verify that after you run the last command, the value for the *IsConnected* variable is True.

    > This validates the LON-SVR3 is now connected to the iSCSI Target server.

### Task 2: Initialize the disks

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR2**, enter the following command, and then select Enter:

    ```powershell
    Get-Disk
    ```

    > Ensure that three disks are in the **Offline** operational status. These should be disks with numbers 4, 5, and 6.

1. In the **Administrator: Windows PowerShell ISE** window for **LON-SVR2**, enter the following commands, selecting Enter after each:

    ```powershell
    Get-Disk | Where OperationalStatus -eq 'Offline' | Initialize-Disk -PartitionStyle MBR
    
    New-Partition -DiskNumber 4 -Size 5gb -AssignDriveLetter
    
    New-Partition -DiskNumber 5 -Size 5gb -AssignDriveLetter
    
    New-Partition -DiskNumber 6 -Size 5gb -AssignDriveLetter
    
    Format-Volume -DriveLetter E -FileSystem NTFS
    
    Format-Volume -DriveLetter F -FileSystem NTFS
    
    Format-Volume -DriveLetter G -FileSystem NTFS
    ```

### Task 3: Validate and create a failover cluster

1. Switch to **LON-SVR2**, and then sign in as **Contoso\Administrator** with password **Pa55w.rd**.

    > **Note:** You must sign in on **LON-SVR2** because you can't run cluster commands over remote PowerShell.

1. At the SConfig menu, enter `15` to Exit to the command line and then select Enter.

1. At the command prompt on **LON-SVR2**, enter **`Test-Cluster LON-SVR2, LON-SVR3`**, and then select Enter.

    > Wait for a few minutes for the cluster validation test to complete. You can expect a few warning messages to appear, but there should be no errors.

1. At the command prompt on **LON-SVR2**, enter the following command, and then select Enter:

    ```powershell
    New-Cluster -Name WFC2022 -Node lon-svr2 -StaticAddress 172.16.0.125
    ```

    > You should receive a cluster name as a result. It should display **Name WFC2022**. Verify that there are no errors.

1. At the command prompt on **LON-SVR2**, enter the following command, and then select Enter:

    ```powershell
    Add-ClusterNode -Name LON-SVR3
    ```

    > When a command prompt appears, verify that no errors are reported.

### Results

After completing this exercise, you installed and configured the Failover Clustering feature.

## Exercise 3: Deploying and configuring a highly available file server

### Scenario

After you have created a cluster infrastructure, you decide to configure a highly available file server and implement settings for failover and failback.

The main tasks for this exercise are to:

1. Add the file server application to the failover cluster.
1. Add a shared folder to a highly available file server.
1. Configure the failover and failback settings.

### Task 1: Add the file server application to the failover cluster

1. Switch to **LON-ADM1**.

1. On **LON-ADM1**, open **Server Manager**, and then from the **Tools** menu, select **Failover Cluster Manager**.

1. In the **Failover Cluster Manager** console, select **Connect to Cluster**.

1. To connect to the cluster that you created in the previous exercise, in the **Select Cluster** window, enter **WFC2022**, and then select **OK**.

1. Expand **`WFC2022.contoso.com`**, select **Roles**, and then notice that no roles display. This is because no cluster roles are configured yet.

1. Select **Nodes**, and then notice that the **LON-SVR2** and **LON-SVR3** nodes both display a status of **Up**.

1. Expand **Storage**, and then select **Disks**. 

   > Notice that three cluster disks have a status of **Online**.

1. On the **Failover Cluster Manager** page, right-click or access the context menu for **Roles**, and then select **Configure Role**.

1. On the **Before You Begin** page, select **Next**.

1. On the **Select Role** page, select **File Server**, and then select **Next**.

1. On the **File Server Type** page, select **File Server for general use**, and then select **Next**.

1. On the **Client Access Point** page, in the **Name** box, enter **FSCluster**.

1. In the **Address** box, enter **172.16.0.130**, and then select **Next**.

1. On the **Select Storage** page, select **Cluster Disk 1** and **Cluster Disk 2**, and then select **Next**.

1. On the **Confirmation** page, select **Next**.

1. On the **Summary** page, select **Finish**.

1. In the **Storage** node, select **Disks**.

   > Verify that three cluster disks are online. **Cluster Disk 1** and **Cluster Disk 2** should be assigned to **FSCluster**.

### Task 2: Add a shared folder to a highly available file server

1. On **LON-ADM1**, in **Failover Cluster Manager**, select **Roles**, select **FSCluster**, and then in the Actions pane, select **Add File Share**.
1. On the **Select Profile** page, select **SMB Share - Quick**, and then select **Next**.
1. On the **Share Location** page, select **Next**.
1. On the **Share Name** page, enter **Docs** for the share name, and then select **Next**.
1. On the **Other Settings** page, select **Next**.
1. On the **Permissions** page, select **Next**.
1. On the **Confirmation** page, select **Create**.
1. On the **View results** page, select **Close**.

### Task 3: Configure the failover and failback settings

1. **On LON-ADM1**, in the **Failover Cluster Manager** console, select **Roles**, select **FSCluster**, and then select **Properties**.

1. Select the **Failover** tab, and then select **Allow failback**.

1. Select **Failback between**, and then enter:
    - **4** in the first text box
    - **5** in the second text box.

1. Select the **General** tab.

1. Under **Preferred owners**, select both **LON-SVR2** and **LON-SVR3**.

1. Select the **LON-SVR3** object, select **Up**, and then select **OK**.

    > LON-SVR3 is now the first preferred owner.

### Results

After completing this exercise, you configured a highly available file server.

## Exercise 4: Validating the deployment of the highly available file server

### Scenario

In implementing a failover cluster, you want to perform failover and failback tests. Additionally, you want to change the witness disk in the quorum.

The main tasks for this exercise are to:

1. Validate the highly available file server deployment.
1. Validate the failover and quorum configuration for the File Server role.

### Task 1: Validate the highly available file server deployment

1. On **LON-ADM1**, open File Explorer, and then try to access the **\\\FSCluster** location.

1. In **FSCluster**, open the Docs folder.

   > Verify that you can access the **Docs** folder.

1. Inside the **Docs** folder, right-click or access the context menu in an empty area of the folder, select **New**, and then select **Text Document**.

1. To accept the default name of the document as **New Text Document.txt**, select Enter.

1. In the **Failover Cluster Manager** console, right-click or access the context menu for **FSCluster**, select **Move**, select **Select Node**, choose the available node from the **Cluster nodes** list, and then select **OK**.

1. On **LON-ADM1**, in File Explorer, verify that you can still access the **\\\FSCluster** location.

### Task 2: Validate the failover and quorum configuration for the File Server role

1. On **LON-ADM1**, in **Failover Cluster Manager**, determine the current owner for the **FSCluster** role. If you select **Roles** and observe the value in the **Owner Node** column, it will be **LON-SVR2** or **LON-SVR3**.
1. Select **Nodes**, and then right-click or access the context menu for the node that's the current owner of the **FSCluster** role.
1. Select **More Actions**, and then select **Stop Cluster Service**.
1. Try to access **\\\FSCluster** from **LON-ADM1** to verify that **FSCluster** has moved to another node and that the **\\\FSCluster** location is still available.
1. Select **Nodes**, and then right-click or access the context menu for the node that has the status of **Down**.
1. Select **More Actions**, and then select **Start Cluster Service**.
1. In the **Failover Cluster Manager** console, right-click or access the context menu for the **`WFC2022.Contoso.com`** cluster, select **More Actions**, and then select **Configure Cluster Quorum Settings**.
1. On the **Before you begin** page, select **Next**.
1. On the **Select Quorum Configuration Options** page, select **Use default quorum configuration**, and then select **Next**.
1. On the **Confirmation** page, select **Next**, and then select **Finish**.
1. Browse to the **Disks** node, select the disk marked **Disk Witness in Quorum**, and then in the Actions pane, select **Take Offline**.
1. In the Offline Cluster Disk prompt, select **Yes**.
1. Verify that **FSCluster** is still available by trying to access it from **LON-ADM1**.
1. In the **Failover Cluster Manager** console, select the disk marked **Disk Witness in Quorum**, and then in the Actions pane, select **Bring Online**.
1. On **LON-ADM1**, close all open windows.

### Results

After completing this exercise, you validated high availability with Failover Clustering.

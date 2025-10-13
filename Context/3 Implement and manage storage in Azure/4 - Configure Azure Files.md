Azure Files offers fully managed file shares in the cloud that are accessible via industry standard protocols. Azure File Sync is a service that allows you to cache several Azure Files shares on an on-premises Windows Server or cloud virtual machine.
# Compare Storage for File Shares and Blob Data
Azure files offer fully managed file shares in the cloud. Can be access via SMD, NFS, and HTTP protocols. Clients can connect from Windows, Linux and macOS devices.
## Compare Azure Files to Azure Blob Storage
It's important to understand when to use Azure Files to store data in file shares rather than using Azure Blob Storage to store data as blobs. The following table compares different features of these services and common implementation scenarios.

| Azure Files (file shares)                                                                                                                                                                                                                                                                                                                         | Azure Blob Storage (blobs)                                                                                                                                                                                                         |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Azure Files provides the SMB and NFS protocols, client libraries, and a REST interface that allows access from anywhere to stored files.                                                                                                                                                                                                          | Azure Blob Storage provides client libraries and a REST interface that allows unstructured data to be stored and accessed at a massive scale in block blobs.                                                                       |
| - Files in an Azure Files share are true directory objects.  <br>- Data in Azure Files is accessed through file shares across multiple virtual machines.                                                                                                                                                                                          | - Blobs in Azure Blob Storage are a flat namespace.  <br>- Blob data in Azure Blob Storage is accessed through a container.                                                                                                        |
| _**Azure Files** is ideal to lift and shift an application to the cloud that already uses the native file system APIs. Share data between the app and other applications running in Azure._  <br>  <br>_Azure Files is a good option when you want to store development and debugging tools that need to be accessed from many virtual machines._ | _**Azure Blob Storage** is ideal for applications that need to support streaming and random-access scenarios._  <br>  <br>_Azure Blob Storage is a good option when you want to be able to access application data from anywhere._ |
# Manage Azure file Shares
Azure Files offers two industry-standard file system protocols for mounting Azure file shares: the Server Message Block (SMB) protocol and the Network File System (NFS) protocol. Azure file shares don't support both the SMB and NFS protocols on the same file share, although you can create SMB and NFS Azure file shares within the same storage account.
## Types of Azure file shares
Azure Files supports two storage tiers: premium and standard. Standard file shares are created in general purpose (GPv2) storage accounts, while premium file shares are created in FileStorage storage accounts. The two storage tiers have the attributes described in the following table.

|Storage tier|Description|
|---|---|
|Premium|Premium file shares store data on solid-state drives (SSDs), and are available only in the FileStorage storage account kind. They provide consistent high performance and low latency, and are available in LRS redundancy, with ZRS available in some regions. Not available in all Azure regions.|
|Standard|Standard file shares store data on hard disk drives (HDDs) and deploy in the general-purpose version 2 (GPv2) storage account type. Provide performance for workloads such as general-purpose file shares and dev/test environments. Standard file shares are available for LRS, ZRS, GRS, and GZRS, in all Azure regions.|
## Types of authentication
There are three main authentications methods that Azure Files supports.

|Authentication method|Description|
|---|---|
|Identity-based authentication over SMB|Provides the same seamless single sign-on (SSO) experience when accessing Azure file shares as accessing on-premises file shares.|
|Access key|An access key is an older and less flexible option. An Azure storage account has two access keys that can be used when making a request to the storage account, including to Azure Files. Access keys are static and provide full control access to Azure Files. Access keys should be secured and not shared with users, because they bypass all access control restrictions. A best practice is to avoid sharing storage account keys and use identity-based authentication whenever possible.|
|A Shared Access Signature (SAS) token|SAS is a dynamically generated Uniform Resource Identifier (URI) that's based on the storage access key. SAS provides restricted access rights to an Azure storage account. Restrictions include allowed permissions, start and expiry time, allowed IP addresses from where requests can be sent, and allowed protocols. With Azure Files, a SAS token is only used to provide REST API access from code.|
## Creating SMB Azure file shares
There are two important settings that you need to be aware of when creating and configuring SMB Azure file shares.
- **Open port 445**. SMB communicates over TCP port 445. Make sure port 445 is open. Also, make sure your firewall isn't blocking TCP port 445 from the client machine. If you can't unblock port 445, then a VPN or ExpressRoute connection from on-premises to your Azure network is required, with Azure Files exposed on your internal network using private endpoints.
- **Enable secure transfer**. The `Secure transfer required` setting enhances the security of your storage account by limiting requests to your storage account from secure connections only. Consider the scenario where you use REST APIs to access your storage account. If you attempt to connect, and secure transfer required is enabled, you must connect by using HTTPS. If you try to connect to your account by using HTTP, and secure transfer required is enabled, the connection is rejected.
# Create file share snapshots
Azure Files provides the capability to take share [snapshots of file shares](https://learn.microsoft.com/en-us/azure/storage/files/storage-snapshots-files). File share snapshots capture a point-in-time, read-only copy of your data.
### File share snapshots
- The Azure Files share snapshot capability is provided at the file share level.
- Share snapshots are incremental in nature. Only data changed since the most recent share snapshot is saved.
- Incremental snapshots minimize the time required to create share snapshots and saves on storage costs.
- Even though share snapshots are saved incrementally, you only need to retain the most recent share snapshot to restore the share.
- You can retrieve a share snapshot for an individual file. This level of support helps with restoring individual files rather than having to restore to the entire file share.
- If you delete a file share that has share snapshots, all of its snapshots are deleted along with the share.
### Things to consider when using file share snapshots
There are several benefits to using file share snapshots and having access to incremental point-in-time data storage. As you review the following suggestions, think about how you can implement file share snapshots in your Azure Files storage solution.

|Benefit|Description|
|---|---|
|**_Protect against application error and data corruption_**|Applications that use file shares perform operations like writing, reading, storage, transmission, and processing. When an application is misconfigured or an unintentional bug is introduced, accidental overwrite or damage can happen to a few data blocks. To help protect against these scenarios, you can take a share snapshot before you deploy new application code. When a bug or application error is introduced with the new deployment, you can go back to a previous version of your data on that file share.|
|**_Protect against accidental deletions or unintended changes_**|Imagine you're working on a text file in a file share. After the text file is closed, you lose the ability to undo your changes. In this scenario, you need to recover a previous version of your file. You can use share snapshots to recover previous versions of the deleted or renamed file.|
|**_Support backup and recovery_**|After you create a file share, you can periodically create a snapshot of the file share to use it for data backup. A share snapshot, when taken periodically, helps maintain previous versions of data that can be used for future audit requirements or disaster recovery.|
# Implement soft delete for Azure files
Azure Files offers [soft delete for file shares](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-prevent-file-share-deletion?toc=%2Fazure%2Fstorage%2Ffile-sync). Soft delete lets you recover deleted files and file shares.
- Soft delete for file shares is enabled at the storage account level.
- Soft delete transitions content to a soft deleted state instead of being permanently erased.
- Soft delete lets you configure the retention period. The retention period is the amount of time that soft deleted file shares are stored and available for recovery.
- Soft delete provides a retention period between 1 and 365 days.
- Soft delete can be enabled on either new or existing file shares.
# Azure Storage Explorer
[Azure Storage Explorer](https://learn.microsoft.com/en-us/azure/storage/storage-explorer/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) is a standalone application that makes it easy to work with Azure Storage data on Windows, macOS, and Linux. With Azure Storage Explorer, you can access multiple accounts and subscriptions, and manage all your Storage content.
Azure Storage Explorer lets you connect to different storage accounts.
- Connect to storage accounts associated with your Azure subscriptions.
- Connect to storage accounts and services that are shared from other Azure subscriptions.
- Connect to and manage local storage by using the Azure Storage Emulator.
# Azure File Sync
[Azure File Sync](https://learn.microsoft.com/en-us/azure/storage/file-sync/file-sync-introduction) enables you to cache several Azure Files shares on an on-premises Windows Server or cloud virtual machine. You can use Azure File Sync to centralize your organization's file shares in Azure Files, while keeping the flexibility, performance, and compatibility of an on-premises file server.
- Azure File Sync transforms Windows Server into a quick cache of your Azure Files shares.
- You can use any protocol that's available on Windows Server to access your data locally with Azure File Sync, including SMB, NFS, and FTPS.
- Azure File Sync supports as many caches as you need around the world.
#### Cloud tiering
Cloud tiering is an optional feature of Azure File Sync. Frequently accessed files are cached locally on the server while all other files are tiered to Azure Files based on policy settings.
- When a file is tiered, Azure File Sync replaces the file locally with a pointer. A pointer is commonly referred to as a _reparse point_. The parse point represents a URL to the file in Azure Files.
- When a user opens a tiered file, Azure File Sync seamlessly recalls the file data from Azure Files without the user needing to know that the file is stored in Azure.
- Cloud tiering files have greyed icons with an offline `O` file attribute to let the user know when the file is only in Azure
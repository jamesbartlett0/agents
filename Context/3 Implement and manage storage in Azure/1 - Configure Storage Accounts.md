# Implement Azure Storage

Azure Storage is Microsoft's cloud storage solution for data storage scenarios. It provides a file system for the cloud, a messaging store for reliable messaging, and a NoSQL store.

Azure Storage is an AI-ready service that you can use to store files, messages, tables, and other types of information. You use Azure storage for applications like file shares. Devs use Azure Storage for working data - such as websites, mobile apps, and desktop applications.

Azure storage supports three categories of data:
- structured
	- Structured data is stored in a relational format that has a shared schema. Structured data is often contained in a database table with rows, columns, and keys. Tables are an autoscaling NoSQL store.
- unstructured
	- Unstructured data is the least organized. The format of unstructured data is referred to as _nonrelational_.
- virtual machine data
	- Virtual machine data storage includes disks and files. Disks are persistent block storage for Azure IaaS virtual machines. Files are fully managed file shares in the cloud.

# Explore Azure Storage Services

![[Pasted image 20251010164119.png]]

## Azure Blob Storage
Blob Storage is optimized for storing massive amounts of unstructured or _nonrelational_ data, such as text or binary data. Blob Storage is ideal for:
- Serving images or documents directly to a browser.
- Storing files for distributed access.
- Streaming video and audio.
- Storing data for backup and restore, disaster recovery, and archiving.
- Storing data for analysis by an on-premises or Azure-hosted service.
## Azure Files
[Azure Files](https://learn.microsoft.com/en-us/azure/storage/files/storage-files-introduction) enables you to set up highly available network file shares. Shares can be accessed by using the Server Message Block (SMB) protocol and the Network File System (NFS) protocol. Multiple virtual machines can share the same files with both read and write access. You can also read the files by using the REST interface or the storage client libraries.
## Azure Queue Storage
[Azure Queue Storage](https://learn.microsoft.com/en-us/azure/storage/queues/storage-queues-introduction) is used to store and retrieve messages. Queue messages can be up to 64 KB in size, and a queue can contain millions of messages. Queues are used to store lists of messages to be processed asynchronously.
## Azure Table Storage
[Azure Table storage](https://learn.microsoft.com/en-us/azure/storage/tables/table-storage-overview) is a service that stores nonrelational structured data (also known as structured NoSQL data) in the cloud, providing a key/attribute store with a schemaless design. Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve. Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data. In addition to the existing Azure Table Storage service, there's a new Azure Cosmos DB Table API offering that provides throughput-optimized tables, global distribution, and automatic secondary indexes.

# Determine Storage Account Types

General purpose storage accounts have two basic types;
- Standard
	- backed by HDD
	- Lowest cost per GB
	- Recommended for applications that require bulk storage or where data is infrequently accessed
- Premium
	- Backed by SSD
	- Fastest consistent performance
	- Recommended for VM disks with I/O intensive applications like databases

|Storage account|Supported services|Recommended usage|
|---|---|---|
|[**Standard** **general-purpose v2**](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-upgrade)|Blob Storage (including Data Lake Storage), Queue Storage, Table Storage, and Azure Files|Standard storage account for most scenarios, including blobs, file shares, queues, tables, and disks (page blobs).|
|[**Premium** **block blobs**](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-block-blob-premium)|Blob Storage (including Data Lake Storage)|Premium storage account for block blobs and append blobs. Recommended for applications with high transaction rates. Use Premium block blobs if you work with smaller objects or require consistently low storage latency. This storage is designed to scale with your applications.|
|[**Premium** **file shares**](https://learn.microsoft.com/en-us/azure/storage/files/storage-how-to-create-file-share)|Azure Files|Premium storage account for file shares only. Recommended for enterprise or high-performance scale applications. Use Premium file shares if you require support for both Server Message Block (SMB) and NFS file shares.|
|[**Premium** **page blobs**](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-blob-pageblob-overview)|Page blobs only|Premium high-performance storage account for page blobs only. Page blobs are ideal for storing index-based and sparse data structures, such as operating systems, data disks for virtual machines, and databases.|
# Replication Strategies

Azure Storage replication copies your data to protect from planned to unplanned events.
## Locally redundant storage
Locally redundant storage is the lowest-cost replication option and offers the least durability compared to other strategies. If DC level disaster occurs, all replicas might be lost
Despise its limitations LRS is recommended for:
- Application stores that can be easily reconstructed
- Non-essential live-feed data
- Governance requirements that require location restrictions
## Zone redundant storage
Zone redundant storage synchronously replicates data across three storage clusters in a single region.
ZRS :
- Isn't currently available in all regions
- Changing to ZRS from another data replication option requires the physical data movement from a single storage stamp to multiple stamps within a region
## Geo-redundant storage
Geo-redundant storage replicates your data to a secondary region (hundreds of miles away from the primary location of the source data).
If you implement GRS, you have two related options to choose from:
- **GRS** replicates your data to another data center in a secondary region. The data is available to be read only if Microsoft initiates a failover from the primary to secondary region.
- **Read-access geo-redundant storage** (RA-GRS) is based on GRS. RA-GRS replicates your data to another data center in a secondary region, and also provides you with the option to read from the secondary region. With RA-GRS, you can read from the secondary region regardless of whether Microsoft initiates a failover from the primary to the secondary.
## Geo-Zone redundant Storage
Geo-zone-redundant storage combines the high availability of zone-redundant storage with protection from regional outages as provided by geo-redundant storage. Data in a GZRS storage account is replicated across three Azure availability zones in the primary region, and also replicated to a secondary geographic region for protection from regional disasters. Each Azure region is paired with another region within the same geography, together making a regional pair.
With a GZRS storage account, you can continue to read and write data if an availability zone becomes unavailable or is unrecoverable. Additionally, your data is also durable during a complete regional outage or during a disaster in which the primary region isn't recoverable. GZRS is designed to provide at least 99.99999999999999% (16 9's) durability of objects over a given year. GZRS also offers the same scalability targets as LRS, ZRS, GRS, or RA-GRS. You can optionally enable read access to data in the secondary region with read-access geo-zone-redundant storage (RA-GZRS).

# Access Storage
Every object you store in Azure storage has a unique URL address. If your storage account name is _mystorageaccount_, default endpoints for your storage account are formed for the Azure services as shown in the following table:

| Service               | Default endpoint                                    |
| --------------------- | --------------------------------------------------- |
| **Container service** | `//`**`mystorageaccount`**`.blob.core.windows.net`  |
| **Table service**     | `//`**`mystorageaccount`**`.table.core.windows.net` |
| **Queue service**     | `//`**`mystorageaccount`**`.queue.core.windows.net` |
| **File service**      | `//`**`mystorageaccount`**`.file.core.windows.net`  |

You can configure a [custom domain](https://learn.microsoft.com/en-us/azure/storage/blobs/storage-custom-domain-name) to access blob data in your Azure storage account. As we reviewed, the default endpoint for Azure Blob Storage is `\<storage-account-name>.blob.core.windows.net`.

# Secure Storage Endpoints

To access network restriction access for storage accounts, use the Firewall and virtual networks settings.

**Things to know about configuring service endpoints**
Here are some points to consider about configuring service access settings:
- You can configure the service to allow access to one or more public IP ranges.
- Subnets and virtual networks must exist in the same Azure region or region pair as your storage account.


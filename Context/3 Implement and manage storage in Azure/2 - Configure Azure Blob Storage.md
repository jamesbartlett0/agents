# Implement Azure Blob Storage
Azure Blob Storage is a service that stores unstructured data in the cloud as objects or blobs. Blob stands for Binary Large Object. Blob Storage is also referred to as *object storage* or *container storage*.
![[Pasted image 20251013105405.png]]

- Blob Storage can store any type of text or binary data. Some examples are text documents, images, video files, and application installers.
- Blob Storage uses three resources to store and manage your data:
    - An Azure storage account
    - Containers in an Azure storage account
    - Blobs in a container
- To implement Blob Storage, you configure several settings:
    - Blob container options.
    - Blob types and upload options.
    - Blob Storage access tiers.
    - Blob lifecycle rules.
    - Blob object replication options.

There are many common uses for Blob Storage. Consider the following scenarios and think about your own data needs:
- **Consider browser uploads**. Use Blob Storage to serve images or documents directly to a browser.
- **Consider distributed access**. Blob Storage can store files for distributed access, such as during an installation process.
- **Consider streaming data**. Stream video and audio by using Blob Storage.
- **Consider archiving and recovery**. Blob Storage is a great solution for storing data for backup and restore, disaster recovery, and archiving.
- **Consider application access**. You can store data in Blob Storage for analysis by an on-premises or Azure-hosted service.

# Create blob containers

Let's look at the configuration characteristics of containers and blobs.
- All blobs must be in a container.
- Containers organize your blob storage.
- A container can store an unlimited number of blobs.
- An Azure storage account can contain an unlimited number of containers.
- You must create a storage container before you can begin to upload data.
## Configure a Container
Containers require a name
- lower case, numbers, and hyphens
- must begin with a letter or number
- minimum length, max 63
Access level
- Private (default)
- Blob; allow public read for the blobs only
- Container: allow public read for entire container
# Assign blob access tiers
#### Hot tier
The Hot tier is optimized for frequent reads and writes of objects in the Azure storage account. A good usage case is data that is actively being processed. An online tier optimized for storing data that is accessed or modified frequently. The hot tier has the highest storage costs, but the lowest access costs.
#### Cool tier
The Cool tier is optimized for storing large amounts of infrequently accessed data. This tier is intended for data that remains in the Cool tier for at least 30 days. A usage case for the Cool tier is short-term backup and disaster recovery datasets and older media content. This content shouldn't be viewed frequently, but it needs to be immediately available. Storing data in the Cool tier is more cost-effective. The cool tier has lower storage costs and higher access costs compared to the hot tier.
#### Cold tier
The Cold tier is also optimized for storing large amounts of infrequently accessed data. This tier is intended for data that can remain in the tier for at least 90 days. The cold tier has lower storage costs and higher access costs compared to the cool tier.
#### Archive tier
The Archive tier is an offline tier that's optimized for data that can tolerate several hours of retrieval latency. Data must remain in the Archive tier for at least 180 days or be subject to an early deletion charge. Data for the Archive tier includes secondary backups, original raw data, and legally required compliance information. This tier is the most cost-effective option for storing data. Accessing data is more expensive in the Archive tier than accessing data in the other tiers.
# Add blob lifecycle management rules
Every dataset has a unique lifecycle. You can use Azure Blob Storage lifecycle management policy rules to accomplish several tasks.
- Transition blobs to a cooler storage tier (Hot to Cool, Hot to Archive, Cool to Archive) to optimize for performance and cost.
- Delete current versions of a blob, previous versions of a blob, or blob snapshots at the end of their lifecycles.
- Apply rules to an entire storage account, to select containers, or to a subset of blobs using name prefixes or blob index tags as filters.
In the Azure portal, you create lifecycle management policy rules for your Azure storage account by specifying several settings. For each rule, you create **If - Then** block conditions to transition or expire data based on your specifications. As you review these details, consider how you can set up lifecycle management policy rules for your data sets.
# Determine blob object replication
Object replication copies blobs in a container asynchronously according to policy rules that you configure.
Replication includes the blob content, metadata properties, and versions.
There are many benefits to using blob object replication. Consider the following scenarios and think about how replication can be a part of your Blob Storage strategy.
- **Consider latency reductions**. Minimize latency with blob object replication. You can reduce latency for read requests by enabling clients to consume data from a region that's in closer physical proximity.
- **Consider efficiency for compute workloads**. Improve efficiency for compute workloads by using blob object replication. With object replication, compute workloads can process the same sets of blobs in different regions.
- **Consider data distribution**. Optimize your configuration for data distribution. You can process or analyze data in a single location and then replicate only the results to other regions.
- **Consider costs benefits**. Manage your configuration and optimize your storage policies. After your data is replicated, you can reduce costs by moving the data to the Archive tier by using lifecycle management policies.
# Manage blobs
A blob can be any type of data and any file size. Azure offers three types of blobs:
- block blob (default): Consists of data that are assembled to make a blob. Ideal for text and binary data, like files, images, and videos
- page blob: Can be up to 8Tb in size. Azure virtual machines use page blobs for OS disks and data disks
- append blob: An append blob is similar to a block blob because the append blob also consists of blocks of data. The blocks of data in an append blob are optimized for _append_ operations. Append blobs are useful for logging scenarios, where the amount of data can increase as the logging operation continues.

For larger numbers of files, it's best to use a tool to upload. Review the following options and consider which tools would suit your configuration needs.

- [**Azure Storage Explorer**](https://learn.microsoft.com/en-us/azure/storage/storage-explorer/vs-azure-tools-storage-manage-with-storage-explorer). Upload, download, and manage blobs, files, queues, and tables, as well as Azure Data Lake Storage entities and managed disks. You can also view, edit, and manage resources, preview data, and configure storage permissions and access controls.
- [**AzCopy**](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10). An easy-to-use command-line tool for Windows and Linux. You can copy data to and from Blob Storage, across containers, and across storage accounts.
- [**Azure Data Box Disk**](https://learn.microsoft.com/en-us/azure/databox/data-box-disk-overview). A service for transferring on-premises data to Blob Storage when large datasets or network constraints make uploading data over the wire unrealistic. You can use Azure Data Box Disk to request solid-state disks (SSDs) from Microsoft. You can copy your data to those disks and ship them back to Microsoft to be uploaded into Blob Storage.
# Determine Blob Storage pricing

In general, the cost of block blob storage depends on:
- Volume of data stored per month.
- Quantity and types of operations performed, along with any data transfer costs.
- Data redundancy option selected.
You can use the Azure Pricing Calculator to estimate your storage costs.
Review the following billing considerations for an Azure storage account and Blob Storage.
- **Performance tiers**. The Blob Storage tier determines the amount of data stored and the cost for storing that data. As the performance tier gets cooler, the per-gigabyte cost decreases.
- **Data access costs**. Data access charges increase as the tier gets cooler. For data in the Cool and Archive tiers, you're billed a per-gigabyte data access charge for reads.
- **Transaction costs**. There's a per-transaction charge for all tiers. The charge increases as the tier gets cooler.
- **Geo-replication data transfer costs**. This charge only applies to accounts that have geo-replication configured, including GRS and RA-GRS. Geo-replication data transfer incurs a per-gigabyte charge.
- **Outbound data transfer costs**. Outbound data transfers incur billing for bandwidth usage on a per-gigabyte basis. This billing is consistent with general-purpose Azure storage accounts.
- **Changes to the storage tier**. If you change the account storage tier from Cool to Hot, you incur a charge equal to reading all the data existing in the storage account. Changing the account storage tier from Hot to Cool incurs a charge equal to writing all the data into the Cool tier (GPv2 accounts only).

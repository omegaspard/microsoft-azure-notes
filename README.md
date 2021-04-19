# microsoft-azure-notes

# Azure 

## Storage on Azure: Azure Blob Storage

- Object storage like S3 or Google cloud storage or Ozonne.

### What's a Blob

Blob means **Binary Large Object**. It was designed for common storage purpose such as personal data storage (images, videos, documents...). So it works with structured and structured data types.

The blobs are stored in **containers** (equivalent to Aws S3 buckets). It's the equivalent of directories. Azure associated each object to an address and those address are linked to a storage account. The address is in a .net format.

### Storage tiers of Azure Blob Storage

GPv2 means General Purpose V2.

The storage of the data depends of it's access.
The "colder" the data is the more expensive the access become.
The dynamism of the data is divided with the following labels: Premium, Hot, Cool, Archive.

The cold data tier have a low storage cost and a high access cost (Archive and Cool).
	- Cool: Latency in milliseconds, data that are 30 days old.
	- Archive: Latency <15 hours, data that are 180 days old.
The hot data tier have high storage cost and lower access cost (Premium and Hot).
	- Premium: Single digit milliseconds latency.
	- Hot: Milliseconds latency.

 The storage differs with the type of account we chose:
	- GPv2: Basic storage account. Can store blobs and files. Support queues and tables.
	- GPv1: Legacy version of GPv2.
	- Block blob storage accounts: Blob only storage, same feature than GPv2 but with higher needs. (high transactions rates, small objects, low storage latency)
	- Blob storage accounts: Only store blob.

#### Azure high availibity

Fails over the differents nodes is manage by replicating data over multiple tiems over different on the cluster (like in HDFS).
We can chose redundancy for our data with different options types.

Region means stuff like eu-west, eu-est, us-west etc....

Node failure are managed in every of the solution.
To my understanding those are more to handle massive disaster like a fire in a datacenter or a major events that make the data unavailable in a whole region.

- LRS, Locally Redondant Storage. Basically like HDFS. Replicate data over different nodes in the cluster. Fault tolerence within a datacenter. Data not accessible if the whole datacenter is offline or down, LRS support all storage tiers.
- ZRS, Zone Redundant Storage, same than above but over datacenter in different zones, so if one fails, we get our data from other datacenter. Storage tiers,GPv2, BlockBlobStorage, FileStorage.
- GRS, Geo-Redundant Storage, Work with primary and secondary datacenter in the same region. If the first fails, we switch to the second one in read only. Storage tiers, GPv2, GPv1, BlobStorage.
- RA-GRS, Read Access Geo-Redundant Storage data in 2 data center in 2 zones. We can access the 2 data center freely (not like the on above). GPv2.
- GZRS, Geo-zone redundant storage = GRS + ZRS, replicated data to 3 region and datacenter. The whole replication package basically. (Consistency and Availabilibity)
- RA-GZRS, ead-access geo-zone-redundant storage, ??? "Read access even if failure in the primary region".

#### Cost

It varies depending on the type of storage account and tiers, and also replication strategy.
There are also treshold depending of the amount we store.

https://www.apptio.com/blog/essential-guide-azure-blob-storage-pricing/

### Queues

The Azure queues for data ingestion are based on the Azure Blob storage.
There are 4 of them:
- Azure Storage Queues: Very huge data storage capacity (200 TB)n unlimited concurent client, maximum TTL of 7 days (no persistence).
- Azure Service Bus Queues: Queue size limited to 80 GB, UNLIMITED PERSISTENCE, full transaction support, duplicate message detection,more control over the messages.
- Azure Service Bus Topics: On to many message delivery, dynmic subscription, filter support (to treat some messages a different ways), good to write to many listeners.
- Azure Event Hub: Large amount of data in short period of times (1 million per second), low latency, mulitple consumer management, order of messages persisted, no dead letter, no transaction support, no TTL.

The solution that seems to be according to our need is the **Azure servuce Bus queues**.

### Analytics on Azure.

Instead of Hadoop, Azure propose HDinsight. As Hadoop HDinsight is mix of different open-source analytics in the microsoft cloud.

- The HDInsight is based on the different Azure files storages and not HDFS.
- Can submit spark jobs with a REST API Apache Livy.
- Support Hive.
- Support HBase (2.1.6).
- Support Kafka (2.1.1).
- In .Net but provide a Java SDK.
- Support Ranger (manage data secrurity across the hadoop plateform, who can access what etc...).
- Support Yarn.
- Support Zookeeper (4.3.1).
- Support Phoenix.
- Support Spark(2.4.4).

Basically it seems that it proposes everything that we use currently.
The only danger I see is that it's more close to the .Net than Java it does propose a Java SDK though.
It also doesn't seem to support HDFS which make sens because as Hadoop is based on HDFS HDInsight is based on Azure Blob.

### Security

Kerberos is available.

https://docs.microsoft.com/en-us/azure/hdinsight/domain-joined/apache-domain-joined-architecture

## Conclusion

It seems that the differents solutions proposed by azure match the hadoops ones.


Links:

- https://www.apptio.com/blog/essential-guide-azure-blob-storage-pricing/
- https://www.snaplogic.com/glossary/azure-blob-storage#:~:text=Azure%20Blob%20storage%20is%20a,as%20images%20and%20multimedia%20files.
- https://docs.microsoft.com/en-us/azure/storage/common/storage-redundancy
- https://www.todaysoftmag.com/article/1260/what-messaging-queue-should-i-use-in-azure
- https://docs.microsoft.com/en-us/java/api/overview/azure/hdinsight?view=azure-java-stable#sdk-installation
- https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-sdk-java-samples
- https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-40-component-versioning

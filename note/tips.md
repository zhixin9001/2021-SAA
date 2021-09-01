
### S3
- prefix
    - Amazon S3 automatically scales to high request rates.
    - Your application can achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket.
    - There are no limits to the number of prefixes in a bucket.
- S3 Transfer Acceleration (S3TA)
    - you pay only for transfers that are accelerated.
    - There are no S3 data transfer charges when data is transferred in from the internet.
    - takes advantage of Amazon CloudFront’s globally distributed edge locations.
- cloudfront, By design, delivering data out of CloudFront can be more cost-effective than delivering it from S3 directly to your users.
- version, Once you version-enable a bucket, it can never return to an unversioned state. 
- Multipart upload, independently, no order, retry, 100 MB
- types
    - Standard, no The minimum storage duration charge, and the other types at least 30days, no retrive fee
        - pay for used,  the pricing is $0.023 per GB per month. 
    - Intelligent-Tiering, automatic moving objects between four access tiers when access patterns change, cost savings, no retrive fee
        - two low latency access tiers optimized for frequent and infrequent access, 
        - and two optional archive access tiers designed for asynchronous access that are optimized for rare access.
        - Frequent Access tier -> 30 days, Infrequent Access tier -> 90 days, (if activated) Archive Access tier -> 180 days, Deep Archive Access tier
    - One Zone-Infrequent Access (S3 One Zone-IA), Infrequent but requires rapid access when needed. costs 20% less than S3 Standard-IA
        - the minimum storage duration is 30 days before you can transition objects from S3 Standard to S3 One Zone-IA or S3 Standard-IA
    - Standard-Infrequent Access (S3 Standard-IA),  costs more than S3 One Zone-IA, less availability
    - S3 Glacier, for data archiving, cheap, can upload objects directly or use lifecycle, Configurable retrieval times, from minutes to hours
    - S3 Glacier Deep Archive, lowest cost, Retrieval time within 12 hours
- retention
    - You can place a retention period on an object version either explicitly or through a bucket default setting. 
    - When you use bucket default settings, you don't specify a Retain Until Date. Instead, you specify a duration, in either days or years
    - Different versions of a single object can have different retention modes and periods.
### API Gateway
- restful api vs. websocket
    - RESTful APIs, HTTP-based, stateless
    - websocket, stateful, full-duplex, route based on message content

### Network
- types
    - AWS Direct Connect, establish a dedicated network connection from your premises to AWS. expensive, and takes a few days to a few months to setup
    - AWS Direct Connect plus VPN, combine one or more AWS Direct Connect with the Amazon VPC VPN. This combination provides an IPsec-encrypted, reduces network costs, increases bandwidth throughput, and provides a more consistent network.
    - AWS Site-to-Site VPN, securely connect your on-premises network or branch office site to your VPC. 
    - VPC transit gateway, a network transit hub that you can use to interconnect your VPC.
- transfer speed
    - AWS Global Accelerator, improve the performance, a good fit for non-HTTP use cases, Http also supported
    - Amazon CloudFront, CDN(content delivery network)
    - diff: AWS Global Accelerator & CloudFront both use the AWS global network and its edge locations around the world. CloudFront improves performance for both cacheable content and dynamic content (such as API acceleration and dynamic site delivery), while Global Accelerator improves performance for a wide range of applications over TCP or UDP. 

### Auto Scaling group
- policy
    - scheduled action
    - lifecycle hook
    - target tracking policy, choose a scaling metric and set a target value, ASG auto creates and manages the CloudWatch alarms
    - step scaling policy, step <-> threshold
    - simple scaling policies, like `step scaling policy`, but without step feature, 
        - Both require you to create CloudWatch alarms for the scaling policies. 
        - Both require you to specify the high and low thresholds for the alarms. 
        - Both require you to define whether to add or remove instances, and how many, or set the group to an exact size.
        - In most cases, step scaling policies are a better choice than simple scaling policies, even if you have only a single scaling adjustment.
The main difference between the policy types is the step adjustments that you get with step scaling policies. When step adjustments are applied, and they increase or decrease the current capacity of your Auto Scaling group, the adjustments vary based on the size of the alarm breach.

### AMI, snapshot
- When the new AMI is copied from region A into region B, it automatically creates a snapshot in region B because AMIs are based on the underlying snapshots. 

### Load Balancer
- Application Load Balancer
    - target type: Instance, private IP or a Lambda function

### Stortage
- Amazon FSx
    - Amazon FSx for Windows File Server, provides fully managed, highly reliable file storage that is accessible over the industry-standard Service Message Block (SMB) protocol. It is built on Windows Server
    - Amazon FSx for Lustre, makes it easy and cost-effective to launch and run the world’s most popular high-performance file system
        - integrates with Amazon S3
- Instance store
    - provides temporary block-level storage for your instance
    - You can specify instance store volumes for an instance only when you launch it. 
    - You can't detach an instance store volume from one instance and attach it to a different instance.
    - The data in an instance store persists only during the lifetime of its associated instance. I
    - data persist: reboot, lost: stop, terminate, hibernate
    - provide high random I/O performance at low cost
- EBS
    - EBS volumes behave like raw, unformatted block devices. 
    - You can mount these volumes as devices on your instances. 
    - EBS volumes that are attached to an instance are exposed as storage volumes that persist independently from the life of the instance.
    - Multi-Attach: a single Provisioned IOPS SSD (io1 or io2) volume to multiple instances(up to 16 Linux instances built on the Nitro System) that are in the same Availability Zone. 
    - $0.10 per GB-month of provisioned storage. 

- EFS
    - regional, across multiple az
    - pay only for the resources that you use. The EFS Standard Storage pricing is $0.30 per GB per month. 
- Snowball Edge Storage Optimized
    - 80 TB of usable HDD storage, 40 vCPUs, 1 TB of SATA SSD storage, and up to 40 Gb network connectivity
    - original Snowball device had 80TB of storage space.
- AWS Storage Gateway, cache, on premises access to virtually unlimited cloud storage
    - File Gateway, s3, offers SMB or NFS-based access to data in Amazon S3 with local caching. 
    - volume gw, cloud-based iSCSI block storage volumes 
    - tape gw, moving tape backups to the cloud. not nfs interface

### Cache
- ElastiCache

### DB
- DynamoDB 
    - a key-value, document database, multi-region, built-in security, backup and restore
    - DynamoDB Accelerator (DAX), a fully managed, highly available, in-memory cache, 10x performance
- Redshift
    - a fully-managed petabyte-scale cloud-based data warehouse 
    - designed for large scale data set storage and analysis.
- Elasticsearch
    - full-text search engine, with an HTTP web interface, schema-free JSON documents
- RDS (Relational Database Service)
    - makes it easy to set up, operate, and scale a relational database in the cloud.

- Amazon Kinesis Data Streams
    - real-time processing of streaming big data

### EC2 
- Placement groups
    - Cluster placement groups, within a single az, high-throughput
    - Partition placement groups, availablity, max 7 partitions per az, can be used to deploy large distributed and replicated workloads, such as HDFS, HBase, and Cassandra, across distinct racks. visibility: which instances are in which partitions. share information accross Partitions
    - Spread placement groups, can span multiple az in the same Region. You can have a maximum of seven running instances per Availability Zone per group. suitable for mixing instance types or launching instances over time.
- volume types
    - SSD-backed volumes, frequent read/write operations with small I/O size, IOPS(每秒的讀寫次數), 
        - General Purpose SSD — gp2/3, Provides a balance of price and performance. We recommend these volumes for most workloads.
        - Provisioned IOPS SSD — io1/io2/io2 Block Express ‡, Provides high performance for mission-critical, low-latency, or high-throughput workloads.
    - HDD-backed volumes, throughput (measured in MiB/s) is a better performance measure than IOPS. CANNOT be used as a boot volume
        - Throughput Optimized HDD, st1 — A low-cost HDD designed for frequently accessed, throughput-intensive workloads.
        - Cold HDD, sc1 — The lowest-cost HDD design for less frequently accessed workloads.
- Nitro System, a collection of AWS-built hardware and software components that enable high performance, high availability, and high security.

### KMS
- customer master key (CMK), enforces a waiting period. To delete a CMK in AWS KMS you schedule key deletion. You can set the waiting period from a minimum of 7 days up to a maximum of 30 days, default 30
- 

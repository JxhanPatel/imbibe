# Module 7 - Storage

In cloud computing, storage is not a one-size-fits-all solution. AWS categorizes storage based on how data is accessed, stored, and managed. Understanding the characteristics, use cases, and billing models of block, file, and object storage is critical for designing resilient and cost-effective architectures.



## 1. Introduction to Storage Types

Traditional IT storage maps directly to AWS cloud storage services. To architect effectively, you must understand the three primary storage types: **Block Storage**, **Object Storage**, and **File Storage**.

### Traditional IT vs. AWS Storage Mapping

| Traditional IT Component | AWS Storage Service | Storage Type | Primary Characteristic |
| --- | --- | --- | --- |
| Direct-Attached Storage (DAS) / Storage Area Network (SAN) | **Amazon EBS** | Block | High-throughput, low-latency, dedicated to a single EC2 instance. |
| Local Temporary Hard Drives | **Amazon EC2 Instance Store** | Block | Ephemeral (temporary) storage physically attached to the host computer. |
| Network Attached Storage (NAS) (NFS / SMB) | **Amazon EFS** / **Amazon FSx** | File | Shared storage accessible by multiple instances simultaneously via standard file protocols. |
| Backup Tapes, Archival Arrays, Unstructured File Servers | **Amazon S3** / **Amazon S3 Glacier** | Object | Flat hierarchy, highly scalable, accessed via HTTP/HTTPS API. |




## 2. Amazon Elastic Block Store (Amazon EBS)

**Amazon EBS** provides durable, block-level storage volumes for use with Amazon EC2 instances. Each EBS volume is automatically replicated within its Availability Zone (AZ) to protect you from component failure.

### Core Characteristics & Mechanisms

* **Block-Level Storage:** Data is stored in fixed-size chunks called blocks. If a single character in a file changes, only the affected block is updated, making it highly efficient for databases.
* **AZ Locked:** An EBS volume is created in a specific Availability Zone and can only be attached to an EC2 instance running in that *same* Availability Zone.
* **Lifecycle Flexibility:** EBS volumes persist independently of the running life of an EC2 instance. You can detach an EBS volume from one instance and attach it to another within the same AZ.
* **Encryption:** Amazon EBS transparently handles encryption of data at rest, data in transit between the instance and the volume, snapshots, and volumes created from snapshots. Encryption occurs on the host EC2 server, minimizing latency.

### Amazon EBS Volume Types

AWS optimizes EBS volumes for different workloads based on performance metrics: **IOPS** (Input/Output Operations Per Second) and **Throughput** (MB/s).

| Volume Type | Category | Core Use Cases | Performance Metric |
| --- | --- | --- | --- |
| **Provisioned IOPS SSD** (`io2`, `io2 Block Express`) | SSD-backed | NoSQL and relational databases (e.g., SAP HANA, Oracle, SQL Server). | Highest IOPS & Throughput. Supports **EBS Multi-Attach**. |
| **General Purpose SSD** (`gp3`, `gp2`) | SSD-backed | System boot volumes, virtual desktops, low-latency applications. | Balanced price and performance. `gp3` scales IOPS independent of storage size. |
| **Throughput Optimized HDD** (`st1`) | HDD-backed | Streaming workloads, big data, data warehouses, log processing. | High throughput (MB/s) for large, sequential datasets. Cannot be a boot volume. |
| **Cold HDD** (`sc1`) | HDD-backed | Throughput-oriented storage for infrequently accessed data. Lowest cost. | Optimized for large, sequential, cold workloads. Cannot be a boot volume. |

> ### ⚠️ Architectural Rule: EBS Multi-Attach
> 
> 
> By default, standard EBS volumes can only be attached to a **single** EC2 instance at a time. However, **Provisioned IOPS SSD (`io2`)** volumes support **EBS Multi-Attach**, allowing you to attach a single volume to multiple EC2 instances concurrently *within the same Availability Zone*. This requires a cluster-aware file system to prevent data corruption.

### Amazon EBS Snapshots

EBS snapshots are **incremental, point-in-time backups** stored automatically in Amazon S3 (though you do not manage the underlying S3 bucket directly).

* **Incremental Mechanism:** The first snapshot captures a full copy of the data. Subsequent snapshots copy *only the blocks that have changed* since the last snapshot.
* **Regional Availability:** While an EBS volume is confined to a specific AZ, its snapshots are tied to the region. You can restore a snapshot into *any* Availability Zone within that region, or copy the snapshot to a different AWS region for Disaster Recovery (DR).

### Real-World Use Case

* **Production Database Hosting:** Running a production Microsoft SQL Server on an EC2 instance. The data files require low latency and high transaction speeds, necessitating an `io2` or `gp3` EBS volume.

### Cost & Billing Principles

* You are billed for the **amount of storage provisioned** per month (e.g., $0.10 per GB-month), regardless of how much data you write to the disk.
* For `gp3` and Provisioned IOPS volumes, you also pay for the IOPS and throughput you explicitly provision beyond the baseline.




## 3. Amazon EC2 Instance Store

Unlike Amazon EBS, an **Amazon EC2 Instance Store** provides temporary block-level storage. This storage is located on disks physically attached to the host computer hosting the EC2 instance.

### Key Characteristics & The Ephemeral Nature

* **High Performance:** Because the storage is physically attached to the underlying hardware host, it offers ultra-low latency and exceptionally high random I/O performance compared to network-attached EBS.
* **Ephemeral Data:** Data in an instance store **does not persist** when the instance is stopped or terminated.

### Data Persistence Matrix

| Action on EC2 Instance | Data on EBS Volume | Data on Instance Store Volume |
| --- | --- | --- |
| **Reboot** | Persists | Persists |
| **Stop / Start** | Persists | **Deleted** (Host hardware changes) |
| **Terminate** | Persists (if "Delete on Termination" is disabled) | **Deleted** |
| **Underlying Hardware Failure** | Persists (due to EBS network replication) | **Deleted** |

> ### 💡 Tip
> 
> 
> Never use an Instance Store for critical, stateful data unless you handle replication at the application layer. If a question mentions "temporary files," "scratch space," "caches," or "buffers" requiring high performance, **Amazon EC2 Instance Store** is the correct architecture choice.

### Real-World Use Case

* **Distributed Hadoop Clusters / Spark Nodes:** Used as a high-speed buffer or scratch space to process chunks of big data where application-level replication handles data resiliency.




## 4. Amazon Simple Storage Service (Amazon S3)

**Amazon S3** is an object storage service offering industry-leading scalability, data availability, security, and performance.

### Object Storage Concepts

S3 stores data as **objects** within containers called **buckets**. It utilizes a flat structure rather than a hierarchical tree system of directories.

* **Bucket:** A logical container for objects. Bucket names must be **globally unique** across all AWS accounts worldwide.
* **Object Components:**
* **Key:** The "name" of the object (e.g., `images/2026/photo.jpg`). S3 uses delimiter characters (like `/`) to mimic a folder structure visually, but it remains a flat key-value store.
* **Value:** The actual data (bytes) being stored. Object sizes range from 0 bytes up to **5 TB**.
* **Metadata:** A set of name-value pairs containing information about the object (e.g., content type, creation date).
* **Version ID:** Unique identifier used when S3 Versioning is enabled.



### Amazon S3 Storage Classes

S3 provides various storage classes optimized for different access patterns and cost efficiencies.

| Storage Class | Availability / Durability | Minimum Storage Duration | Retrieval Fee | Core Use Case |
| --- | --- | --- | --- | --- |
| **S3 Standard** | 99.99% / 11 9s ($99.999999999\%$) | None | None | Active, frequently accessed data, website assets. |
| **S3 Standard-Infrequent Access (S3 Standard-IA)** | 99.9% / 11 9s | 30 Days | Per GB retrieved | Data accessed less frequently but requires rapid access when needed (e.g., monthly reports). |
| **S3 One Zone-Infrequent Access (S3 One Zone-IA)** | 99.5% / 11 9s (**Stored in 1 AZ**) | 30 Days | Per GB retrieved | Non-critical, reproducible data (e.g., secondary backups, image thumbnails). Cost is 20% lower than Standard-IA. |
| **S3 Glacier Flexible Retrieval** | 99.9% / 11 9s | 90 Days | Per GB retrieved | Archive data. Retrieval times range from **minutes (expedited) to 3-5 hours (standard)**. |
| **S3 Glacier Deep Archive** | 99.9% / 11 9s | 180 Days | Per GB retrieved | Long-term digital preservation. Lowest cost storage in AWS. Retrieval times within **12 hours**. |
| **S3 Intelligent-Tiering** | Variable / 11 9s | None | None (Small automation fee) | Data with **unknown or changing access patterns**. Automatically shifts data between frequent and infrequent access tiers without operational overhead. |

> ### Cost Optimization Note: 11 9s Durability
> 
> 
> All S3 storage classes (except One Zone-IA) are designed for **99.999999999% (11 9s) durability** of objects over a given year. This resilience is achieved by automatically replicating data across a minimum of **three physically separated Availability Zones** within an AWS Region.

### Lifecycle Management

S3 Lifecycle configurations allow you to automate data transitions to optimize costs:

* **Transition Actions:** Automatically migrate objects to cheaper storage classes based on age (e.g., Move from S3 Standard to S3 Standard-IA after 30 days, then to Glacier Deep Archive after 90 days).
* **Expiration Actions:** Automatically delete objects after a specified retention period (e.g., Delete application server access logs after 365 days).

### Security & Access Control

S3 is secure by default; all newly created buckets are private. Access can be granted via:

* **IAM Policies:** Attached to users or groups within your AWS account to govern what actions they can perform on S3.
* **Bucket Policies:** Resource-based policies attached directly to the bucket. Used to manage cross-account permissions or make objects public.
* **S3 Access Control Lists (ACLs):** Legacy resource-based access control used to grant permissions on individual objects.
* **Encryption:** Supports **Server-Side Encryption (SSE)** using S3-managed keys (SSE-S3), AWS KMS keys (SSE-KMS), or customer-provided keys (SSE-C).

### Real-World Use Case

* **Static Website Hosting & Data Lake Landing Zone:** Hosting client-side web assets (HTML, CSS, JS) directly from an S3 bucket configured for public access, or storing raw parquet data files ingestible by analytical engines like Amazon Athena.

### Cost & Billing Principles

S3 pricing models charge based on:

1. **Storage volume:** GBs stored per month (varies by storage class).
2. **Data transfer:** Data transferred *out* of the AWS Region (Data ingress/transfer-in is free).
3. **HTTP Requests:** Number of `PUT`, `COPY`, `POST`, `LIST`, and `GET` requests executed.



## 5. Amazon Elastic File System (Amazon EFS)

**Amazon EFS** provides a serverless, fully managed, elastic file system for use with AWS Cloud services and on-premises resources.

### Core Features & Architecture

* **Protocol Support:** Operates over the **Network File System version 4 (NFSv4)** protocol.
* **Multi-Instance Sharing:** Unlike standard EBS volumes, an Amazon EFS file system can be mounted concurrently by **hundreds or thousands of EC2 instances** across multiple Availability Zones within a region.
* **Elastic Capacity:** Automatically scales up or down as files are added or removed; you do not provision storage size in advance.

### Amazon EBS vs. Amazon EFS Comparison

| Feature | Amazon EBS | Amazon EFS |
| --- | --- | --- |
| **Access Type** | Block Storage | File Storage (POSIX compliant) |
| **Scalability** | Manual volume resizing required | Automatic, elastic scaling |
| **Instance Connectivity** | 1 instance per volume (except `io2` Multi-Attach) | Thousands of concurrent EC2 instances |
| **Scope** | Tied to a single Availability Zone (AZ) | Regional (accessible across all AZs) |
| **Performance Profile** | Ultra-low latency (single-digit ms) | Consistent low latency with scalable throughput |

### Real-World Use Case

* **Enterprise Content Management Systems (WordPress/Drupal):** A fleet of EC2 instances sitting behind an Application Load Balancer requires concurrent access to a shared directory containing theme files, user uploads, and code configurations.

### Cost & Billing Principles

* Billed for the average volume of storage used over the month (measured in GB-hours).
* Features Lifecycle Management to automatically move infrequently accessed files to **EFS Infrequent Access (EFS IA)**, reducing storage costs up to 92%.



## 6. AWS Shared Responsibility Model for Storage

Securing and maintaining storage resources is co-managed between AWS and the customer under the Shared Responsibility Model.

### AWS Responsibility ("Security OF the Cloud")

* Ensuring physical security of the data centers housing the physical hard disks and SSD arrays.
* Guaranteeing hardware lifecycle management and destroying retired storage media securely.
* Managing the infrastructure virtualization layer and automated replication protocols (e.g., EBS replication inside the AZ; S3 replication across three AZs).

### Customer Responsibility ("Security IN the Cloud")

* **Data Classification & Lifecycle:** Deciding what data goes into which storage class and establishing lifecycle rules.
* **Access Management:** Crafting IAM permissions, bucket policies, and security group rules to restrict storage access.
* **Encryption Configuration:** Enabling Server-Side Encryption (SSE) and managing encryption keys via AWS KMS.
* **Backup Management:** Configuring EBS backup snapshot schedules and testing disaster recovery runbooks.



## 7. Connecting Storage to the Well-Architected Framework

### 1. Operational Excellence

* **Automation:** Automate EBS snapshots via **Amazon Data Lifecycle Manager (DLM)** or **AWS Backup** to eliminate human error in operational backup routines.

### 2. Security

* **Least Privilege:** Implement explicit S3 Block Public Access configurations at the account level to avoid accidental data exposure. Enable S3 Object Lock for Write-Once-Read-Many (WORM) compliance to prevent accidental or malicious deletion.

### 3. Reliability

* **Design for Degradation:** Utilize S3 Cross-Region Replication (CRR) to replicate critical datasets dynamically to an entirely separate geographical region to ensure business continuity during catastrophic regional events.

### 4. Performance Efficiency

* **Right-Sizing Storage Types:** Do not deploy network-based storage when extreme performance is needed for ephemeral calculations. Match data access frequency to storage performance characteristics (e.g., using Provisioned IOPS SSDs for transactional databases instead of General Purpose SSDs).

### 5. Cost Optimization

* **Storage Tiering:** Implement S3 Lifecycle policies and EFS Lifecycle management to shift data automatically from hot storage tiers to cold archival storage tiers based on data age.

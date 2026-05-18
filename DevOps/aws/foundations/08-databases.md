# Module 8: Databases

AWS offers a broad range of purpose-built databases designed for different use cases, data models, and performance requirements. Understanding the distinctions between relational and non-relational databases, managed and unmanaged deployments, and specific AWS database services is critical for cloud architecture and exam preparation.



## 1. Relational vs. Non-Relational Databases

To choose the right database service, you must first understand the fundamental differences between data structures, scaling mechanisms, and access patterns.

| Characteristic | Relational Databases (SQL) | Non-Relational Databases (NoSQL) |
| --- | --- | --- |
| **Data Model** | Data is stored in **tables** with fixed rows and columns. | Data is stored in various models: **key-value, document, graph, wide-column, or in-memory**. |
| **Schema** | **Strict, predefined schema**. Data must be structured before writing. | **Flexible, dynamic schema**. Documents or items can have unique structures/attributes. |
| **Relationships** | Maintained via **Foreign Keys** and queried using complex `JOIN` operations. | Data is usually **denormalized**, meaning related data is nested within a single record or document. |
| **Scaling** | **Vertical Scaling** (Scale-Up) by adding more compute (CPU, RAM) or faster storage to a single instance. | **Horizontal Scaling** (Scale-Out) by partitioning and distributing data across multiple servers/shards. |
| **Transactions** | Highly optimized for **ACID** (Atomicity, Consistency, Isolation, Durability) guarantees. | Optimized for **BASE** (Basically Available, Soft state, Eventual consistency) or targeted ACID compliance. |
| **Primary Use Case** | ERP systems, CRM platforms, financial ledger systems, transactional applications. | Big data applications, real-time analytics, user profiles, content management, high-throughput shopping carts. |



## 2. Amazon Relational Database Service (Amazon RDS)

Amazon RDS is a fully managed service that makes it easy to set up, operate, and scale a relational database in the AWS Cloud. It automates time-consuming administration tasks such as hardware provisioning, database setup, patching, and backups.

### Supported Database Engines

Amazon RDS supports six familiar database engines:

* **Amazon Aurora** (AWS-native, MySQL/PostgreSQL-compatible)
* **MySQL**
* **PostgreSQL**
* **MariaDB**
* **Oracle**
* **Microsoft SQL Server**

### Managed vs. Unmanaged (The Shared Responsibility Model)

Deploying a database on AWS presents an architectural choice: run it yourself on Amazon EC2 (Unmanaged/Self-managed) or use Amazon RDS (Fully Managed).

* **Database on EC2 (Unmanaged):** You are responsible for OS patching, database software installation, backups, high availability, scaling, and database tuning. Provides maximum control over the underlying OS and file system.
* **Amazon RDS (Managed):** AWS handles OS and DB patching, automated backups, hardware scaling, and power/infrastructure redundancy. You are solely responsible for **application optimization, data schema design, and query tuning**.

### Amazon RDS High Availability: Multi-AZ Deployments

Multi-AZ (Availability Zone) deployments are designed for **High Availability (HA) and Disaster Recovery (DR)**.

* **Mechanism:** AWS automatically provisions and maintains a synchronous **Standby replica** in a different Availability Zone within the same Region. Writes are synchronously replicated from the Primary DB instance to the Standby replica to ensure data consistency.
* **Failover Process:** If the Primary instance fails (e.g., loss of AZ availability, hardware failure, OS crash), Amazon RDS automatically triggers a failover. The **DNS record** for your database endpoint is updated to point to the Standby replica.
* **Performance Impact:** The standby instance cannot accept active read traffic; it exists purely for failover capability.

> ### 💡 Tip
> 
> 
> Multi-AZ replication is **synchronous** and intended for **High Availability/Disaster Recovery**. It does *not* scale read performance. Failover is completely automated and requires no application code modification because the database connection string (endpoint) remains identical.

### Amazon RDS Scalability: Read Replicas

Read Replicas are used to scale the **read throughput** of a database and offload read queries from the primary instance.

* **Mechanism:** Amazon RDS uses the database engine's native **asynchronous replication** to update one or more Read Replicas from a source DB instance.
* **Scaling Read Traffic:** Applications connect to dedicated Read Replica endpoints to execute read-heavy queries (e.g., business intelligence reports, search filters).
* **Disaster Recovery Capability:** A Read Replica can be manually **promoted** to a standalone primary database instance if needed.
* **Deployment Options:** Read Replicas can be created within the same Availability Zone, across different Availability Zones, or even across **different AWS Regions** (Cross-Region Read Replicas) to minimize latency for global users.



## 3. Amazon Aurora

Amazon Aurora is a enterprise-class, fully managed relational database engine built for the cloud, compatible with MySQL and PostgreSQL.

### Architecture and Mechanics

Aurora features a decoupled architecture that separates the **compute layer** (which processes SQL queries) from the **storage layer**.

* **Distributed Storage:** Aurora automatically replicates your data storage volume across **three Availability Zones (AZs)**, maintaining **six copies** of your data (two copies per AZ).
* **Self-Healing Storage:** Storage is virtualized and grows automatically up to 128 TiB. It continuously monitors for disk failures and repairs damaged blocks seamlessly without interrupting database availability.
* **Aurora Replicas:** You can provision up to **15 Aurora Replicas** to scale read traffic. Because Aurora Replicas share the same underlying virtual storage volume as the primary instance, data replication lag is near-zero (typically milliseconds).
* **Aurora Serverless:** An on-demand, auto-scaling configuration where the database automatically starts up, shuts down, and scales compute capacity up or down based on actual application demand.



## 4. Amazon DynamoDB

Amazon DynamoDB is a fully managed, serverless, **NoSQL key-value and document database** designed to deliver single-digit millisecond performance at any scale.

### Core Concepts and Components

* **Tables:** Data is organized into collections of records called Tables.
* **Items:** A table contains multiple Items. An item is a collection of attributes that is uniquely identifiable among all other items (analogous to a row in SQL).
* **Attributes:** The fundamental data elements that build an item (analogous to a column in SQL). Attributes can be deeply nested documents (JSON).
* **Primary Key:** DynamoDB requires a primary key when creating a table to uniquely identify each item. It can be a simple **Partition Key** (Hash attribute) or a composite key consisting of a **Partition Key and Sort Key** (Hash + Range attribute).

### Scalability and Performance

* **Serverless:** No servers to provision, patch, or manage. It automatically scales throughput and storage up or down.
* **Single-Digit Millisecond Latency:** Designed to maintain consistent read and write response times regardless of data volume or request volume.
* **Data Partitioning:** DynamoDB automatically divides your data across multiple physical storage servers based on the Partition Key value to distribute request workloads.
* **DynamoDB Accelerator (DAX):** A fully managed, highly available, **in-memory cache** explicitly built for DynamoDB. It reduces read latencies from single-digit milliseconds to **microseconds** for extremely read-heavy tables.

### Global Tables

DynamoDB Global Tables provide a fully managed solution for deploying a **multi-region, multi-active database**. It automatically replicates data across your choice of AWS Regions, allowing applications to perform local reads and writes with ultra-low latency worldwide.



## 5. Other Purpose-Built AWS Database Services

Modern cloud architectures leverage specialized databases for non-relational, highly structured, or performance-critical use cases.

* **Amazon ElastiCache:** A fully managed, **in-memory data store and cache** compatible with Redis and Memcached. It is used to significantly improve application performance by caching frequently accessed data from primary relational databases or storing transient session states.
* **Amazon Neptune:** A fast, reliable, fully managed **graph database** service optimized for storing and navigating highly connected datasets (e.g., identity resolution graphs, social networks, fraud detection engines, recommendation engines).
* **Amazon DocumentDB:** A fully managed, scalable **document database** service designed to be compatible with MongoDB workloads, allowing developers to store and query semi-structured data using JSON format.
* **Amazon Quantum Ledger Database (QLDB):** A fully managed **ledger database** that provides a transparent, immutable, and cryptographically verifiable transaction log. It is ideal for systems tracking financial transactions, supply chains, or regulatory histories where data modification must be impossible to falsify.



## 6. Real-World Use Cases and Service Selection

Choosing a database service depends entirely on your access patterns, data structure, and business objectives.

| AWS Database Service | Ideal Use Case Strategy | Real-World Scenario |
| --- | --- | --- |
| **Amazon RDS** | Complex transactional data requiring strict schema consistency and standard SQL queries. | A legacy e-commerce platform migrating its core **ERP and financial ledger system** to the cloud. |
| **Amazon Aurora** | High-throughput, enterprise-scale relational applications that require automated storage scaling and maximum availability. | A global SaaS application requiring an open-source database engine that can seamlessly scale storage and survive an entire **AZ outage** without data loss. |
| **Amazon DynamoDB** | High-velocity, unpredictable web traffic requiring ultra-low latency reads/writes and horizontal scale. | A mobile gaming application storing millions of users' **real-time profiles, state data, and high scores**. |
| **Amazon ElastiCache** | Caching static web pages, database queries, or user session data to reduce database load. | An online ticketing platform protecting its primary RDS database from crashing during a **sudden flash sale** by caching seating charts. |
| **Amazon Neptune** | Tracking complex relationships, hierarchies, and interconnections between nodes. | A banking application running an advanced **fraud detection algorithm** by mapping connections between accounts, devices, and IP addresses. |



## 7. Cloud Economics and Pricing Models

Database costs can heavily impact your AWS bill. Understanding the drivers of database pricing is critical to maintaining a cost-efficient architecture.

### Amazon RDS & Aurora Pricing Factors

* **DB Instance Class:** Charged per hour based on the size (vCPU, RAM) and type of instance provisioned.
* **Storage Type & Provisioning:** Charged per GB-month based on the storage allocated (General Purpose SSD `gp3` vs. Provisioned IOPS `io1`/`io2`).
* **Data Transfer:** Inbound data transfer is free. Outbound data transfer across AWS Regions or to the internet incurs a cost. Transfers within the same AZ are free, but **Cross-AZ data replication** (such as traffic to Read Replicas in different AZs) incurs standard data transfer fees.
* **Backup Storage:** Automated backups and manual snapshots incur costs per GB-month once they exceed 100% of your total provisioned database storage for that region.

### Amazon DynamoDB Pricing Factors

DynamoDB features two distinct capacity management modes that change how you are billed:

* **Provisioned Capacity Mode:** You explicitly specify the number of **Read Capacity Units (RCUs)** and **Write Capacity Units (WCUs)** your application requires. You pay a predictable hourly rate for this provisioned throughput, regardless of whether your application uses it. This is most cost-effective for **predictable, stable traffic**.
* **On-Demand Capacity Mode:** You do not manage throughput. DynamoDB instantly scales up or down based on traffic patterns. You pay strictly per **million read and write requests**. This is ideal for **unpredictable, bursty, or low-traffic workloads**.



## 8. Mapping Concepts to the Well-Architected Framework

### Security Pillar

* **Encryption at Rest:** Ensure encryption is enabled during database creation using AWS Key Management Service (AWS KMS). This encrypts underlying storage, automated backups, and snapshots.
* **Network Isolation:** Always place Amazon RDS and ElastiCache instances inside **Private Subnets** within a VPC. Databases should never be given public IP addresses; only web/application servers should communicate with them.
* **IAM Authentication:** Utilize AWS Identity and Access Management (IAM) to manage database access privileges where supported, rather than embedding database root passwords into application configuration files.

### Reliability Pillar

* **Multi-AZ Deployments:** Always configure Multi-AZ for production workloads to ensure seamless, automated failover and data durability in the event of an infrastructure or Availability Zone outage.
* **Point-in-Time Recovery (PITR):** Enable automated backups in RDS and DynamoDB to allow your organization to restore data back to any specific second within a defined retention window (typically up to 35 days).

### Performance Efficiency Pillar

* **Right-Sizing:** Utilize AWS Compute Optimizer or Amazon CloudWatch metrics (CPU Utilization, Freeable Memory) to scale DB instance sizes up or down based on actual compute demands.
* **Database Offloading:** Use Amazon ElastiCache to handle static, read-heavy operations, preventing the primary relational database from processing repetitive, non-transactional database queries.

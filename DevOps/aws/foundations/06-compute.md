# Module 6: Compute

## 1. Overview of AWS Compute Services

Compute is the core engine that powers workloads in the cloud. AWS offers a diverse portfolio of compute services tailored to different architectural paradigms, ranging from traditional virtual machines to serverless functions.

Understanding the distinction between these paradigms is essential for optimizing performance, scalability, and cost efficiency.

### Traditional IT vs. AWS Compute Services

| Traditional IT Component | AWS Compute Equivalent | Key Cloud Advantage |
| --- | --- | --- |
| Physical On-Premises Servers | **Amazon EC2 (Elastic Compute Cloud)** | Swaps upfront capital expenditure (CapEx) for variable operational expenditure (OpEx); scales in minutes. |
| Virtual Machines (VMware/Hyper-V) | **Amazon EC2 Instances** | Completely programmable via APIs; wide variety of instance types optimized for different workloads. |
| On-Premises Hypervisor | **AWS Nitro System / Xen** | Managed entirely by AWS; isolates tenants securely and maximizes hardware performance. |
| On-Premises Container Cluster | **Amazon ECS / Amazon EKS** | Eliminates the need to install, master, and operate your own container orchestration software. |
| Bare-Metal Scripts / Cron Jobs | **AWS Lambda** | Eliminates server provisioning and management; runs code only when triggered, charging per millisecond. |

### The Compute Spectrum: IaaS vs. PaaS vs. Serverless

AWS compute options exist on a spectrum of control versus management abstraction.

```
[ More Control / More Management ] -----------------------------> [ Less Control / Automated Management ]
  Infrastructure as a Service (IaaS) -> Platform as a Service (PaaS) -> Serverless (FaaS / CaaS)
  (Amazon EC2)                          (AWS Elastic Beanstalk)          (AWS Lambda / AWS Fargate)

```

* **Infrastructure as a Service (IaaS):** **Amazon EC2** provides the highest level of control. You manage the guest operating system, runtime, data, and applications, while AWS manages the physical hardware, virtualization layer, and facilities.
* **Platform as a Service (PaaS):** **AWS Elastic Beanstalk** reduces management overhead. You upload application code, and the platform automatically handles provisioning, load balancing, auto-scaling, and health monitoring.
* **Serverless / Function as a Service (FaaS):** **AWS Lambda** removes the concept of an underlying server entirely from the user. You deploy raw code, and AWS dynamically scales execution context on demand. You pay *only* for the execution time used.


## 2. Amazon Elastic Compute Cloud (Amazon EC2)

Amazon EC2 provides secure, resizable compute capacity in the cloud as virtual machines known as **EC2 instances**.

### Key Concepts & Mechanisms

An EC2 instance is created from an **Amazon Machine Image (AMI)**. The AMI defines the initial state of the instance, including:

1. A root volume template (Operating System, application server, and applications).
2. Launch permissions that control which AWS accounts can use the AMI.
3. A block device mapping that specifies the volumes to attach to the instance upon launch.

> ### ⚠️ Architectural Rule: AMI Scoping
> 
> 
> AMIs are region-specific. If you need to launch an instance in a different AWS Region using an existing AMI, you must first copy the AMI to that destination region.

### EC2 Instance Types (The Categories)

AWS classifies EC2 instances into specialized families optimized for distinct resource vectors. A common mnemonic to remember these families is **FIGHT SNMDR** (or *Fight Some New Micro-Data Risks*).

| Instance Family | Primary Resource Focus | Common Real-World Use Cases |
| --- | --- | --- |
| **General Purpose (M, T)** | Balanced CPU, memory, and networking. | Web servers, small databases, development and testing environments. |
| **Compute Optimized (C)** | High-performance processors (CPU-bound). | Batch processing, media transcoding, high-performance web servers, scientific modeling. |
| **Memory Optimized (R, X, U)** | Large memory footprints (RAM-bound). | In-memory databases (e.g., SAP HANA, Redis), open-source databases, real-time big data analytics. |
| **Accelerated Computing (P, G, Inf)** | Hardware accelerators / GPUs / ASICs. | Machine Learning (ML) training and inference, 3D rendering, graphics-intensive applications. |
| **Storage Optimized (I, D, H)** | High, sequential read/write access to massive local datasets. | NoSQL databases (e.g., Cassandra, MongoDB), distributed file systems, data warehousing. |

### EC2 Pricing Models

Selecting the right purchasing option is a critical component of the **Cost Optimization** pillar of the AWS Well-Architected Framework.

```
                  ┌───────────────────────────────┐
                  │      EC2 Pricing Models       │
                  └───────────────┬───────────────┘
                                  │
         ┌────────────────────────┼────────────────────────┐
         ▼                        ▼                        ▼
┌─────────────────┐      ┌─────────────────┐      ┌─────────────────┐
│   On-Demand     │      │ Savings Plans / │      │  Spot Instances │
│                 │      │ Reserved Inst.  │      │                 │
│ • No commitment │      │ • 1 or 3 year   │      │ • Up to 90% off │
│ • Spiky/short   │      │   commitment    │      │ • Interruptible │
│   workloads     │      │ • Steady-state  │      │ • Stateless apps│
└─────────────────┘      └─────────────────┘      └─────────────────┘

```

* **On-Demand:** You pay for compute capacity by the second or hour, with zero long-term commitment.
* *Best Use Case:* Short-term, unpredictable workloads that cannot tolerate interruption, or initial application development phase.


* **Savings Plans & Reserved Instances (RI):** Provide up to a 72% discount compared to On-Demand pricing in exchange for a commitment to a consistent amount of compute usage (measured in $/hour) for a **1-year or 3-year term**.
* *Best Use Case:* Applications with stable, predictable, steady-state utilization profiles.


* **Spot Instances:** Allow you to bid on spare AWS compute capacity for up to a **90% discount** off On-Demand rates. However, AWS can reclaim these instances with a **2-minute notification** if they need the capacity back.
* *Best Use Case:* Fault-tolerant, stateless, flexible, or batch-processing workloads (e.g., big data analytics, CI/CD pipelines).


* **Dedicated Hosts:** A physical EC2 server fully dedicated for your use.
* *Best Use Case:* Meeting strict regulatory compliance mandates or accommodating complex, core-bound software licensing agreements (e.g., bring-your-own-license / BYOL for Microsoft or Oracle).



> ### 💡 Tip: Spot vs. On-Demand
> 
> 
> If an application can tolerate interruptions without losing progress (by using checkpoints), **Spot Instances** are always the most cost-effective choice. If a workload must run continuously without interruption, select **On-Demand** or **Savings Plans**.

---

## 3. Container Services on AWS

Containers package application code, configurations, and dependencies into a single object, ensuring software runs reliably across different compute environments. AWS offers two primary orchestrators and an abstraction layer to manage them.

### Amazon Elastic Container Service (Amazon ECS)

Amazon ECS is a highly scalable, high-performance container orchestration service natively integrated with AWS ecosystem features like IAM, Amazon VPC, and Route 53.

### Amazon Elastic Kubernetes Service (Amazon EKS)

Amazon EKS provides a managed Kubernetes service on AWS, eliminating the operational burden of deploying and securing a Kubernetes control plane (master nodes).

* *Why choose EKS over ECS?* Open-source standardization. If an organization already uses Kubernetes on-premises, EKS offers a hybrid path with identical tooling, manifests, and open-source plugins.

### Compute Backends: EC2 vs. AWS Fargate

Both ECS and EKS can deploy container workloads onto two distinct underlying compute types:

| Metric | EC2 Launch Type (Managed Infrastructure) | AWS Fargate Launch Type (Serverless) |
| --- | --- | --- |
| **Provisioning** | You provision, configure, and scale the underlying EC2 instances running the container daemon. | AWS provisions, manages, and patches the underlying infrastructure automatically. |
| **Pricing Model** | You pay for the running EC2 instances, regardless of whether containers utilize 100% of the capacity. | You pay for the exact amount of vCPU and memory resources allocated to the container *per second*. |
| **Operational Effort** | High (OS updates, Docker daemon maintenance, cluster node scaling). | Near-zero server management overhead. |

```
ECS / EKS Cluster (Control Plane Orchestration)
       │
       ├─► [EC2 Launch Type]  ──► You manage & scale the underlying VMs
       │
       └─► [Fargate Launch Type] ──► Serverless execution (AWS manages the host VMs)

```

---

## 4. Serverless Compute: AWS Lambda

AWS Lambda is an event-driven, serverless computing platform that executes code in response to events and automatically manages the underlying compute resources.

### Architectural Mechanism: Event-Driven Execution

Lambda code does not run continuously. It remains idle until invoked by an AWS service event trigger or an API call.

```
[ Event Source ] ──────► [ AWS Lambda Function ] ──────► [ Downstream Service ]
 (e.g., S3 Upload,         (Executes code, scales        (e.g., DynamoDB Write,
  API Gateway Call)         per millisecond)              SES Email Dispatch)

```

* **Triggers:** Examples include an object uploaded to an **Amazon S3** bucket, a modification to an **Amazon DynamoDB** table, an HTTP request via **Amazon API Gateway**, or a message landing in an **Amazon SQS** queue.
* **Automatic Scaling:** Lambda scales horizontally instantly. If 1,000 events trigger simultaneously, Lambda spins up 1,000 isolated, concurrent execution environments to handle them.

### Operational Characteristics

* **Maximum Execution Duration:** A single Lambda invocation has a hard timeout limit of **15 minutes**. Workloads that require longer runtimes must be broken down or hosted on EC2/Fargate.
* **Stateless Architecture:** Lambda execution contexts are ephemeral. Any data that needs to persist across executions must be written to an external data tier, such as Amazon S3, Amazon RDS, or Amazon DynamoDB.

### Billing & Pricing Parameters

Lambda charges based on two primary dimensions:

1. **Request Volume:** The total number of requests across all functions.
2. **Duration (Compute Time):** Calculated from the millisecond your code begins executing until it returns or terminates. The rate is determined by the amount of **memory** you allocate to the function (allocating more memory linearly scales the allocated CPU allocation).

---

## 5. Automated Platform Management: AWS Elastic Beanstalk

AWS Elastic Beanstalk is an easy-to-use Platform as a Service (PaaS) designed for deploying and scaling web applications and services developed with Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker.

### Core Philosophy: High Control + Low Management

Unlike other managed platforms that hide infrastructure details, Elastic Beanstalk gives you visibility and control. If you decide you want to manage the underlying infrastructure components yourself, you can take control of the EC2 instances, auto-scaling groups, or load balancers directly.

### Managed Provisions

Elastic Beanstalk automates the deployment workflow by managing:

* Provisioning underlying AWS resources (EC2, RDS databases, ElastiCache).
* Configuring and provisioning **Elastic Load Balancing (ELB)** and **Auto Scaling Groups (ASG)**.
* Application health monitoring and log collection.
* Applying OS patches and platform runtime updates.

### Pricing Structure

> ### 💰 Cost Rule: Elastic Beanstalk Pricing
> 
> 
> There is **no additional charge** for AWS Elastic Beanstalk itself. You pay exclusively for the underlying AWS infrastructure resources (such as EC2 instances, S3 buckets, Elastic Load Balancers) that are generated and consumed by your running application.

---

## 6. Shared Responsibility Model for Compute Services

Security responsibility scales dynamically based on the compute abstraction layer selected.

```
       ┌────────────────────────────────────────────────────────┐
       │                 CUSTOMER RESPONSIBILITY                │
       ├───────────────────┬───────────────────┬────────────────┤
       │    Amazon EC2     │    AWS Fargate    │   AWS Lambda   │
┌──────┼───────────────────┼───────────────────┼────────────────┤
│ Code │ Application Code  │ Application Code  │ Function Code  │
├──────┼───────────────────┼───────────────────┼────────────────┤
│ Data │ Customer Data     │ Customer Data     │ Customer Data  │
├──────┼───────────────────┼───────────────────┼────────────────┤
│ Config│ IAM & Sec Groups │ IAM & Sec Groups  │ IAM Execution  │
├──────┼───────────────────┼───────────────────┼────────────────┤
│ OS   │ Guest OS, Patches │ Managed by AWS    │ Managed by AWS │
└──────┴───────────────────┴───────────────────┴────────────────┘

```

### Infrastructure as a Service (e.g., Amazon EC2)

* **AWS Responsibility:** Physical security of data centers, host hardware infrastructure, network switching, and the virtualization hypervisor layer.
* **Customer Responsibility:** Provisioning guest operating system updates and security patches, firewall configuration (Security Groups), identity management (IAM), data encryption (EBS), application runtimes, and application code dependencies.

### Container Abstractions (e.g., AWS Fargate)

* **AWS Responsibility:** Physical hardware, hypervisor, host operating system patching, and scaling container runtime environments.
* **Customer Responsibility:** Container image configuration, application code, security groups isolating task execution, defining tasks, container configuration, and identity policies.

### Serverless Abstractions (e.g., AWS Lambda)

* **AWS Responsibility:** Physical hardware, OS, runtime software stacks (e.g., Python, Node.js underlying runtimes), auto-scaling, and system patches.
* **Customer Responsibility:** Maintaining the application code, function identity execution policies (IAM roles), and configuring specific function triggers.

---

## 7. Connecting Compute to the Well-Architected Framework

###  Security Pillar

* **Principle of Least Privilege:** Assign precise execution permissions to EC2 instances using **IAM Instance Profiles**, or to Lambda using **Lambda Execution Roles**, rather than embedding long-lived AWS Access Keys directly inside application code configuration files.
* **Network Isolation:** Deploy EC2 clusters inside private subnets within an Amazon VPC, allowing access from the public internet only via controlled entry points like an Elastic Load Balancer or an API Gateway.

###  Reliability Pillar

* **Decoupled Architectures:** Use compute nodes alongside **Amazon SQS** (Simple Queue Service) or **Amazon SNS** (Simple Notification Service). This prevents spikes in traffic from overwhelming compute engines and causing systemic downtime.
* **Multi-AZ Deployments:** Always distribute EC2 compute power or container instances across multiple Availability Zones to withstand data center disruptions.

###  Performance Efficiency Pillar

* **Right-Sizing Compute:** Use performance monitoring metrics from **Amazon CloudWatch** to identify over-provisioned or idle EC2 instances. Adjust instance families to align with actual resource utilization trends.
* **Serverless First:** Use AWS Lambda for short-lived, event-driven processes to minimize idle runtime costs and improve response times during sudden traffic spikes.

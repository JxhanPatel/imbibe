# Module 1 - Cloud Concepts Overview



## Introduction to Cloud Computing

Traditional IT infrastructure requires organizations to predict capacity, invest heavily in physical hardware, and manage complex data centers. Cloud computing fundamentally shifts this paradigm by converting capital expenses into variable expenses and treating infrastructure as software.

### Definition of Cloud Computing

According to official AWS and industry standards, **cloud computing** is the on-demand delivery of compute power, database storage, applications, and other IT resources through a cloud services platform via the internet with **pay-as-you-go pricing**.

### Mapping Traditional IT to AWS

To understand the cloud, it helps to map legacy physical infrastructure components directly to their AWS virtualized equivalents:

| Traditional IT Component | AWS Cloud Equivalent | Primary Function / Resource Type |
| --- | --- | --- |
| **Physical Servers** | Amazon EC2 (Elastic Compute Cloud) | Compute resources (CPU, RAM) |
| **Firewalls, ACLs, Routers** | Security Groups, Network ACLs, Amazon VPC | Network security and routing |
| **Network Switches / Cables** | Amazon VPC (Virtual Private Cloud) | Network infrastructure |
| **Hard Drives (Direct Attached)** | Amazon EBS (Elastic Block Store) | Block storage for instances |
| **Network Attached Storage (NAS)** | Amazon EFS (Elastic File System) | Shared file storage |
| **On-Premises Databases** | Amazon RDS, Amazon DynamoDB | Database management systems |



## Three Cloud Deployment Models

Organizations choose different deployment strategies based on security, compliance, legacy investments, and operational needs.

### 1. Cloud-Based Deployment (All-in on Cloud)

* **Definition:** A cloud-based application is fully deployed in the cloud, and all parts of the application run in the cloud.
* **Characteristics:** Applications are either built natively in the cloud or migrated from an existing infrastructure to take full advantage of cloud benefits.
* **Use Case:** A fast-growing startup building a new mobile application entirely on AWS using managed services to eliminate infrastructure overhead.

### 2. On-Premises Deployment (Private Cloud)

* **Definition:** Deploying resources on-premises using virtualization and resource management tools is sometimes called a **private cloud**.
* **Characteristics:** While it provides dedicated resources, it does not offer many of the true benefits of cloud computing (such as shifting CapEx to OpEx or massive elasticity) because it still requires physical hardware maintenance and procurement.
* **Use Case:** A highly regulated government agency that must keep sensitive data hosted on physical hardware located within their own restricted-access facility.

### 3. Hybrid Deployment

* **Definition:** A hybrid deployment is a way to connect infrastructure and applications between cloud-based resources and existing resources that are located on-premises.
* **Characteristics:** Enables an organization to extend and grow their infrastructure into the cloud while connecting back to internal legacy systems via secure connections like **AWS Direct Connect** or a VPN.
* **Use Case:** A large established bank that keeps its legacy core transaction-processing engine on-premises but uses AWS to host its customer-facing web and mobile applications for rapid scaling.



## The Six Advantages of Cloud Computing

AWS defines six distinct business and technical advantages that cloud infrastructure provides over traditional IT deployments:

### 1. Trade Capital Expense for Variable Expense

* **Technical Detail:** Instead of investing heavily in physical data centers and servers before knowing how they will be used (**Capital Expenditures or CapEx**), organizations pay only for the computing resources consumed (**Variable/Operational Expenditures or OpEx**).
* **Business Value:** Reduces financial risk and lowers the barrier to entry for new initiatives.

### 2. Benefit from Massive Economies of Scale

* **Technical Detail:** Because AWS aggregates usage from hundreds of thousands of customers, it achieves massive scale, which translates into lower pay-as-you-go prices.
* **Business Value:** AWS has historically passed these efficiencies on to customers in the form of dozens of voluntary price reductions.

### 3. Stop Guessing Capacity

* **Technical Detail:** Eliminates the problem of pre-provisioning hardware. In traditional IT, under-provisioning leads to application downtime, while over-provisioning results in wasted money on idle servers. With cloud computing, resources scale automatically based on real-time traffic.
* **Business Value:** Ensures high availability during peak traffic while optimizing infrastructure spend during low-traffic periods.

### 4. Increase Speed and Agility

* **Technical Detail:** In a cloud environment, new IT resources are only a click away. The time required to make those resources available to developers drops from weeks (hardware procurement) to minutes.
* **Business Value:** Dramatically reduces the time-to-market for new features, allowing organizations to experiment and innovate faster.

### 5. Stop Spending Money Running and Maintaining Data Centers

* **Technical Detail:** Organizations can focus less on the "undifferentiated heavy lifting" of infrastructure management—such as racking servers, stacking hardware, powering facilities, and managing cooling systems.
* **Business Value:** Allows engineering teams to focus exclusively on their core business applications and customer experience rather than infrastructure plumbing.

### 6. Go Global in Minutes

* **Technical Detail:** AWS allows applications to be deployed across multiple geographic locations around the world with just a few clicks.
* **Business Value:** Provides lower latency and a better experience for global users at a fraction of the cost of building international data centers.



## Web Services and the AWS Ecosystem

### What is a Web Service?

A **web service** is any piece of software that makes itself available over the internet or private networks and uses a standardized format (such as XML or JSON) for the request and response of an application programming interface (API) interaction.

### The Core AWS Concept

AWS is fundamentally built on web services. Every action performed in AWS—whether clicking a button in the AWS Management Console, running a command in the AWS CLI, or deploying an infrastructure script—sends a secure **HTTP/HTTPS API request** to an AWS service endpoint.




## Moving to the Cloud: Business Transformation & CAF

Migrating to the cloud requires an organization to look beyond technology and transform how the entire business operates. To guide this transition, AWS created the **AWS Cloud Adoption Framework (AWS CAF)**.

### The 6 Perspectives of AWS CAF

The AWS CAF organizes guidance into six distinct focus areas called **Perspectives**. Each perspective consists of specific capabilities that help structure a comprehensive cloud transformation plan.

#### Business Perspectives (Focus on Business Capabilities)

1. **Business Perspective:**
* *Focus:* Ensures that cloud investments align with business strategies and drive measurable business outcomes.
* *Roles:* Finance Managers, Strategic Planners, Business Analysts.


2. **People Perspective:**
* *Focus:* Supports development of an organizational evolution strategy, focusing on training, competency development, and change management.
* *Roles:* Human Resources, Personnel Managers, Training Directors.


3. **Governance Perspective:**
* *Focus:* Focuses on skills and processes to align IT strategy with business strategy, maximizing organizational benefits while minimizing risks.
* *Roles:* CIO, Project Managers, Enterprise Architects, Risk Managers.



#### Technical Perspectives (Focus on Technical Capabilities)

4. **Platform Perspective:**
* *Focus:* Architecting, delivering, and optimizing cloud infrastructure solutions. Helps define standard patterns and target state platforms.
* *Roles:* CTO, Cloud Architects, Systems Engineers.


5. **Security Perspective:**
* *Focus:* Ensures that the architecture meets security objectives, compliance requirements, regulatory standards, and risk management goals.
* *Roles:* CISO, Security Auditors, Security Analysts.


6. **Operations Perspective:**
* *Focus:* Defines how daily, weekly, and yearly operational health is conducted. Focuses on running, monitoring, and maintaining cloud applications.
* *Roles:* Operations Managers, Site Reliability Engineers (SREs), Service Desk Personnel.





## Fundamental AWS Cloud Billing Principles

Understanding how AWS charges for its services is vital for maintaining cost-efficient architectures.

### The Three Fundamental Drivers of Cost

While every service has unique billing dimensions, almost all AWS costs trace back to three fundamental metrics:

1. **Compute:** Charged per second or per minute (varies based on instance type, CPU, and RAM allocation).
2. **Storage:** Charged per Gigabyte (GB) per month for data retained on AWS storage media.
3. **Data Transfer OUT:** Data moving out of AWS data centers to the internet is generally metered and charged per GB.

> ### ⚠️ Architectural Rule: The Data Transfer Rule
> 
> 
> * **Data Transfer IN** from the internet to AWS resources is **always free**.
> * **Data Transfer OUT** from AWS resources to the internet is **chargeable**.
> * Data transfer *between* services within the same AWS Region is often free or discounted compared to internet data transfer out, though transfers across Availability Zones within a region incur nominal fees.
> 
> 

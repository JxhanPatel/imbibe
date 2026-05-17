# Module 4: AWS Cloud Security

<img width="800" height="400" alt="image" src="https://github.com/user-attachments/assets/1b930179-b6d8-4669-91f6-f1564f07b003" />

## 1. The AWS Shared Responsibility Model

The fundamental cornerstone of AWS security is the **Shared Responsibility Model**. Security and compliance are shared responsibilities between AWS and the customer. This model relieves the customer’s operational burden as AWS operates, manages, and controls the components from the host operating system and virtualization layer down to the physical security of the facilities.

### AWS Responsibility: "Security OF the Cloud"

AWS is entirely responsible for protecting the global infrastructure that runs all the services offered in the AWS Cloud. This infrastructure includes:

* **Physical Security:** Highly secure, nondescript data centers with controlled, need-based access, 24/7 security guards, two-factor authentication, video surveillance, and secure destruction of decommissioned storage drives.
* **Hardware Infrastructure:** The actual physical servers, storage devices, and appliances.
* **Software Infrastructure:** The virtualization layer, hypervisors, and host operating systems running the physical infrastructure.
* **Network Infrastructure:** Physical routers, switches, load balancers, cabling, and automated protection against network-level attacks (like DDoS mitigation at the edge).

### Customer Responsibility: "Security IN the Cloud"

The customer is responsible for everything they provision, configure, and store within AWS. This includes:

* **Customer Data:** Managing encryption (both at rest and in transit) and data integrity.
* **Platform, Applications, Identity & Access Management (IAM):** Configuring fine-grained user permissions, provisioning application code, and preventing unauthorized access.
* **Operating System:** Patching and updating the guest OS on compute instances (e.g., Amazon EC2).
* **Network Security Configuration:** Configuring security groups, Network Access Control Lists (NACLs), and routing tables.

> ### ⚠️ Tip
> 
> 
> Always determine who has the *configuration switch* or *access to the patch command*. If a task requires configuring an OS firewall or patching Windows/Linux on an EC2 instance, it is a **Customer** responsibility. If it requires updating the hypervisor software or decommissioning a physical hard drive, it is an **AWS** responsibility.

### Service Models and Security Shifts

The boundary line of responsibility shifts depending on the abstraction level of the service model being used:

| Service Model | Service Examples | AWS Responsibilities | Customer Responsibilities |
| --- | --- | --- | --- |
| **Infrastructure as a Service (IaaS)** | Amazon EC2, Amazon Amazon EBS, Amazon VPC | Physical facilities, hardware, hypervisor virtualization layer. | Guest OS patching, application runtime, data encryption, network firewall rules (Security Groups). |
| **Platform as a Service (PaaS)** | AWS Lambda, Amazon RDS, AWS Elastic Beanstalk | Physical facilities, underlying hardware, host & guest OS patching, database/runtime platform updates. | Application code configuration, API access management, data classification, and data security settings. |
| **Software as a Service (SaaS)** | AWS Trusted Advisor, AWS Shield, Amazon Chime | Complete software solution stack, application infrastructure, scaling, and platform availability. | Data input management, application-level user access control, and identity provisioning. |



## 2. AWS Identity and Access Management (IAM)

AWS Identity and Access Management (IAM) is a foundational web service that allows you to securely manage access to AWS resources. It provides centralized control to authenticate (verify who you are) and authorize (verify what you are allowed to do) users and systems.

### Core Components of IAM

* **IAM User:** An identity created in AWS representing a specific person or application interacting with AWS resources.
* **IAM Group:** A collection of IAM users. Permissions assigned to a group are automatically inherited by any user added to that group, simplifying permission management at scale.
* **IAM Role:** An identity with explicit permission policies that is **not** uniquely associated with one specific person. Instead, it is intended to be *assumed* temporarily by anyone (or any AWS service/application) who needs it.
* **IAM Policy:** A formal document (written in **JSON** syntax) that explicitly defines permissions—specifying what actions are allowed or denied on which resources.

### Architectural Comparison: Identity-Based vs. Resource-Based Policies

| Feature | Identity-Based Policies | Resource-Based Policies |
| --- | --- | --- |
| **Attachment Point** | Attached directly to an IAM User, Group, or Role. | Attached directly to an individual resource (e.g., S3 Bucket, KMS Key). |
| **Relationship** | Many-to-many; modifying a policy instantly affects all identities attached to it. | One-to-one with the resource; defines who outside or inside the account can access it. |
| **Structure Type** | Contains `Effect`, `Action`, `Resource`, and `Condition`. | Contains `Effect`, `Action`, `Resource`, `Condition`, **AND** an explicit `Principal`. |
| **Availability** | Supported globally across virtually all AWS actions. | Only supported by specific AWS services (e.g., Amazon S3, AWS KMS, Amazon SQS). |

### The Principle of Least Privilege

The guiding structural design for IAM is the **Principle of Least Privilege**. Users and services must only be granted the precise, minimal permissions required to complete their designated business functions, and absolutely nothing more. By default, all requests to AWS are implicitly **Denied**.

### Types of IAM Security Credentials

IAM users can access AWS environments via two distinct channels, each requiring specific authentication credentials:

1. **AWS Management Console Access:** Authenticates using a 12-digit Account ID (or account alias), an IAM Username, and an IAM Password. Highly secure environments mandate the enforcement of **Multi-Factor Authentication (MFA)**.
2. **Programmatic Access:** Authenticates using an **Access Key ID** and a **Secret Access Key**. This configuration bypasses the interactive console interface to provide cryptographic authentication for the AWS Command Line Interface (CLI) and AWS Software Development Kits (SDKs).

> ### 🛑 Critical Security Warning
> 
> 
> A Secret Access Key is only visible at the exact moment of creation. If lost, it cannot be recovered from the AWS Console; a new access key pair must be generated and rotated. Never commit access keys to public version control systems (like GitHub).


## 3. Securing a New AWS Account: The Critical Initial Steps

When an AWS account is first created, it begins with a single identity known as the **AWS Account Root User**. This identity has absolute, unalterable control over all resources and billing data in the account.

```
[Root User Created] 
       │
       ▼
[Enable Multi-Factor Authentication (MFA)]
       │
       ▼
[Create Admin Group & IAM User]
       │
       ▼
[Deactivate/Delete Root Access Keys]
       │
       ▼
[Lock Away Root Credentials & Use IAM Admin User]

```

### Essential Tasks Reserved ONLY for the Root User

Because of the safety risk, the root user should not be used for daily management tasks. However, explicit billing and structural configurations require root login:

* Changing account settings (e.g., legal company name, root email, contact information).
* Closing an AWS account.
* Changing or altering the AWS Support plan.
* Registering as a seller in the AWS Marketplace.

### Step-by-Step Initial Hardening Checklist

1. **Stop Using the Root User Immediately:** Log into the root user once to create an administrative IAM user and group. Apply the `AdministratorAccess` policy to the group, add your IAM user to it, and log out of the root account.
2. **Enable Multi-Factor Authentication (MFA):** Enforce hardware or virtual MFA (e.g., Google Authenticator, YubiKey) on the root user account and all high-privilege IAM user accounts.
3. **Activate AWS CloudTrail:** CloudTrail continuously tracks user activity and API requests across the entire AWS infrastructure. It maintains a searchable 90-day event history by default, but a customized **Trail** should be constructed to pipe these logs into a secure, encrypted Amazon S3 bucket for permanent audit storage.
4. **Enable Billing Alerts and Reports:** Configure the **AWS Cost and Usage Report** to deliver daily itemized cost data directly into an S3 bucket to flag cost anomalies early.


## 4. Securing Enterprise Infrastructure and Data at Scale

As multi-account strategies evolve, security must be automated, audited, and enforced centrally.

### AWS Organizations and Service Control Policies (SCPs)

**AWS Organizations** allows enterprises to centrally consolidate, manage, and scale multiple AWS accounts under a structured tree of **Organizational Units (OUs)**.

To govern permission boundaries at scale, administrators deploy **Service Control Policies (SCPs)**. SCPs establish the absolute maximum permissions available to any account within an OU.

> ### 🧠 Well-Architected Connection: Security Pillar
> 
> 
> An SCP functions as a coarse-grained guardrail. Even if an IAM user inside a child account has an explicit `AdministratorAccess` identity policy, if an SCP applied to that account specifies a `Deny` for a service (e.g., blocking Amazon DynamoDB access), the user **cannot** use that service. The effective permission is always the *intersection* of the SCP and the IAM policy.

### Data Security: Encryption Mechanisms

Data protection is categorized into two functional states:

1. **Data at Rest:** Data stored physically on non-volatile media (e.g., hard drives, solid-state drives, magnetic tapes). AWS secures this via **AWS Key Management Service (AWS KMS)**, which easily creates and controls cryptographic keys across services like Amazon S3, Amazon EBS, and Amazon RDS.
2. **Data in Transit:** Data moving over an active network connection. This traffic is secured using **Transport Layer Security (TLS)** connections. **AWS Certificate Manager (ACM)** completely manages, provisions, and handles the automated lifecycle renewal of public/private SSL/TLS certificates to enforce secure HTTPS channels.

### Amazon S3 Security Fine-Tuning

Amazon S3 buckets are secure by default. Advanced structural protection includes:

* **S3 Block Public Access:** A global, account-level switch that completely overrides any bucket policies or Access Control Lists (ACLs) to ensure data cannot accidental be made public.
* **Bucket Policies:** Resource-based JSON structures applied directly to a bucket to allow or deny specific bucket operations based on source IPs, TLS status, or specific AWS account origins.



## 5. Compliance, Auditing, and Managed Security Services

AWS maintains rigorous compliance frameworks to streamline external audits and prove operational security posture.

### Mapping Compliance via AWS Artifact and AWS Config

* **AWS Artifact:** An on-demand portal providing direct access to AWS’s internal security and compliance reports. It enables customers to download AWS’s independent auditor attestations, ISO certifications, SOC 1/2/3 reports, and Payment Card Industry (PCI) documentation to validate infrastructure compliance to business stakeholders.
* **AWS Config:** A fully managed service that continuously evaluates, audits, and monitors the historical configuration states of your AWS resources. It maps current configurations against desired, compliant baselines and triggers automated notifications or remediations when resources drift out of compliance.

### Core Managed Security Services

To maintain real-time protection against infrastructure threats, AWS offers native, automated security tooling:

* **Amazon Cognito:** Provides a highly scalable, hosted user directory system handling sign-up, sign-in, and granular access token controls for mobile and web applications. It supports federation with social identity networks (Google, Apple) and enterprise identity systems via SAML 2.0.
* **AWS Shield:** A managed Distributed Denial of Service (DDoS) protection service that safeguards applications running on AWS.
* *Shield Standard:* Automatically enabled for all AWS customers at **no additional cost**, protecting against common layer 3 and 4 network attacks.
* *Shield Advanced:* A paid tier offering real-time mitigation against sophisticated layer 7 application attacks, specialized routing monitoring, and 24/7 access to the AWS DDoS Response Team (DRT).




## 6. Traditional IT to AWS Security Mapping

| Traditional IT Infrastructure Component | Equivalent AWS Cloud Security Solution | Architectural Improvement/Value |
| --- | --- | --- |
| Physical Data Center Security Guards / Keycards | **AWS Infrastructure Security Operations** | Complete offloading of physical perimeter maintenance costs and compliance audits. |
| Active Directory / Corporate LDAP Directories | **AWS Directory Service / AWS IAM** | Centralized, granular cloud authorization using native JSON syntax and IAM role token exchange. |
| On-Premises Hardware Security Modules (HSMs) | **AWS Key Management Service (KMS) / AWS CloudHSM** | FIPS 140 compliance coupled with native API integration for effortless multi-service data encryption. |
| Compliance Audits & Physical Paper Reports | **AWS Artifact** | On-demand access to real-time independent global auditing documentation. |
| Hardware Appliances for Network Traffic Sniffing | **AWS CloudTrail / AWS Config** | Immutable, structured logging of every single API configuration change across global regions. |

# Module 5: Networking and Content Delivery

## Introduction to Networking and Content Delivery

In a traditional data center, networking requires physical hardware: routers, switches, patch panels, and miles of cabling. In AWS, networking is completely software-defined. Understanding how to construct a secure, scalable, and highly available network topology is the foundation of any cloud architecture.

This module covers how to build your isolated network space in AWS, route traffic, secure it, and deliver content globally with low latency.


## 1. Amazon Virtual Private Cloud (Amazon VPC)

An **Amazon VPC** is a logically isolated virtual network dedicated to your AWS account. It gives you complete control over your virtual networking environment, including selecting your own IP address range, creating subnets, and configuring route tables and network gateways.

### Traditional IT vs. AWS Networking

| Traditional IT Component | AWS Networking Equivalent | Description |
| --- | --- | --- |
| Physical Data Center Network | **Amazon VPC** | Isolated logical network wrapper for your resources. |
| LAN / Internal Switch | **Subnets** | Segments of a VPC's IP address range to group resources. |
| Router | **Route Tables / Local Routing** | Determines where network traffic is directed. |
| Network Security Team / DMZ | **Internet Gateway (IGW) & NAT Gateway** | Controls entry and exit points to the public internet. |
| Hardware Firewall | **Network Access Control Lists (NACLs) & Security Groups** | Multi-layered traffic filtering protocols. |

### IP Addressing in a VPC

When you create a VPC, you must assign it an IPv4 address range in the form of a **Classless Inter-Domain Routing (CIDR)** block (e.g., `10.0.0.0/16`).

* **Allowed Block Sizes:** The prefix length must be between `/16` (65,536 available addresses) and `/28` (16 available addresses).
* **Private IP Ranges:** AWS recommends using RFC 1918 private address spaces:
* `10.0.0.0 - 10.255.255.255` (`10.0.0.0/8`)
* `172.16.0.0 - 172.31.255.255` (`172.16.0.0/12`)
* `192.168.0.0 - 192.168.255.255` (`192.168.0.0/16`)



> ⚠️ **Architectural Gotcha:** You cannot change the primary CIDR block of a VPC after creation. If your VPC IP range overlaps with your corporate on-premises network or other connected VPCs, you will not be able to establish a VPN link or VPC Peering connection. Always plan your IP schema carefully.



## 2. Subnets and Routing

A **Subnet** is a range of IP addresses within your VPC. You launch AWS resources (like EC2 instances) into a specific subnet.

### Subnet Characteristics

* **Availability Zone Specific:** Each subnet must reside entirely within **one** Availability Zone (AZ) and cannot span multiple AZs. For high availability, you must deploy subnets across multiple AZs.
* **Reserved AWS IP Addresses:** For *every* subnet you create, AWS automatically reserves **5 IP addresses**. If your subnet CIDR is a `/24` (256 IPs), only 251 are usable.
* `x.x.x.0`: Network address.
* `x.x.x.1`: Reserved by AWS for the VPC router.
* `x.x.x.2`: Reserved by AWS for the DNS server (Amazon Provided DNS).
* `x.x.x.3`: Reserved by AWS for future use.
* `x.x.x.255`: Network broadcast address (AWS does not support broadcast routing, but reserves the IP).



### Public vs. Private Subnets

* **Public Subnet:** A subnet whose route table contains a direct route to an **Internet Gateway (IGW)**. Resources in a public subnet can have public IPs and can communicate directly with the public internet.
* **Private Subnet:** A subnet whose route table does *not* contain a route to an Internet Gateway. Resources inside a private subnet only have private IPs and are isolated from inbound internet traffic.

### Route Tables

A **Route Table** contains a set of rules (called routes) that determine where network traffic from your subnet or gateway is directed.

* **Main Route Table:** The default route table automatically assigned to a VPC upon creation.
* **Implicit Local Route:** Every route table contains a default rule allowing internal communication between all subnets within the VPC (e.g., Destination: `10.0.0.0/16`, Target: `local`). This cannot be deleted or modified.



## 3. VPC Gateways and Network Address Translation (NAT)

To connect your VPC to the outside world, you use specific software-defined gateways.

### Internet Gateway (IGW)

An **Internet Gateway** is a redundant, horizontally scaled, and highly available VPC component that allows communication between instances in your VPC and the internet.

* **Dual Purpose:** It provides a target in your VPC route tables for internet-bound traffic, and it performs Network Address Translation (NAT) for instances assigned public IPv4 addresses.
* **Binding:** You can attach exactly **one** IGW to a VPC at a time.

### NAT Gateway

A **Network Address Translation (NAT) Gateway** allows instances in a **private subnet** to connect to the internet or other AWS services, but prevents the internet from initiating a connection with those instances.

* **Deployment:** Must be deployed inside a **public subnet** and must be assigned an **Elastic IP (EIP)** address.
* **Routing:** Private subnets route their internet-bound traffic (`0.0.0.0/0`) to the NAT Gateway target.

| Feature | Internet Gateway (IGW) | NAT Gateway |
| --- | --- | --- |
| **Subnet Location** | Attached at the VPC level. | Placed inside a **Public Subnet**. |
| **Traffic Direction** | Inbound and Outbound. | Outbound Only (Inbound responses allowed). |
| **Managed by** | AWS (Fully managed, auto-scales). | AWS (Fully managed, scales up to 100 Gbps). |
| **Cost** | Free. | Charged per hour of usage + per GB of data processed. |

> 💡 **Tip:** To ensure high availability for outbound private traffic, deploy a NAT Gateway in **each** Availability Zone. If a single AZ goes down and your private subnets across the entire VPC rely on a single NAT Gateway in that failed AZ, all private subnets lose internet access.



## 4. VPC Security: Security Groups vs. Network ACLs

AWS provides a dual layer of firewalls to protect your virtual network.

### Security Groups (Instance-Level Security)

A **Security Group** acts as a virtual firewall for your **EC2 instances** to control inbound and outbound traffic.

* **Stateful Firewall:** If you send an outbound request from your instance, the inbound response traffic for that request is automatically allowed to flow back in, regardless of inbound security group rules.
* **Default State:** By default, all inbound traffic is blocked, and all outbound traffic is allowed. You can only create **allow** rules; you cannot create deny rules.
* **Evaluation:** All rules are evaluated simultaneously before traffic is permitted.

### Network Access Control Lists (NACLs - Subnet-Level Security)

A **Network ACL** is an optional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more **subnets**.

* **Stateless Firewall:** Return traffic must be explicitly allowed by an inbound/outbound rule pair. If you allow inbound traffic on port 80, you must also explicitly configure an outbound rule to allow the response traffic back out (usually to ephemeral ports `1024-65535`).
* **Rule Ordering:** Rules are processed in numerical order, starting with the lowest-numbered rule. As soon as a rule matches the traffic, it is applied, and no further rules are evaluated.
* **Explicit Deny:** Supports both **allow** and **deny** rules. This is ideal for blocking specific malicious IP addresses.

### Detailed Security Comparison

| Feature | Security Group | Network ACL (NACL) |
| --- | --- | --- |
| **Operating Level** | Instance level (Network Interface / ENI) | Subnet level |
| **Statefulness** | **Stateful:** Automatically allows return traffic. | **Stateless:** Must explicitly permit return traffic. |
| **Supported Rules** | Allow rules only. | Allow and Deny rules. |
| **Evaluation Order** | Evaluates all rules comprehensively. | Processes rules sequentially by number. |
| **Application** | Applied to an instance only if explicitly assigned. | Automatically applies to all instances in the bound subnet. |



## 5. Global Networking: Route 53 and CloudFront

When applications require global reach, low latency, and high availability, AWS leverages its global infrastructure via Route 53 and CloudFront.

### Amazon Route 53

**Amazon Route 53** is a highly available and scalable cloud Domain Name System (DNS) web service. It translates human-readable names like `[www.example.com](https://www.example.com)` into numeric IP addresses like `192.0.2.1`.

#### Core Functions:

1. **Domain Registration:** Buy and manage domain names directly within AWS.
2. **DNS Routing:** Route internet traffic to resources (EC2, ELB, S3 buckets).
3. **Health Checking:** Monitor the health of your resources. Route 53 can divert traffic away from an unhealthy endpoint to a healthy one.

#### Route 53 Routing Policies:

* **Simple Routing:** Directs traffic to a single static resource (e.g., one IP address).
* **Weighted Routing:** Distributes traffic across multiple resources based on user-defined proportions (e.g., 70% to production, 30% to a beta deployment).
* **Latency-based Routing:** Routes users to the AWS Region that provides the lowest network latency.
* **Failover Routing:** Configures active-passive failover. Sends traffic to a primary region, shifting to a secondary DR site if health checks fail.
* **Geolocation Routing:** Routes traffic based on the geographic location of your users (e.g., users in Europe are directed to a European localized server).

### Amazon CloudFront

**Amazon CloudFront** is a fast **Content Delivery Network (CDN)** service that securely delivers data, videos, applications, and APIs to customers globally with low latency and high transfer speeds.

#### How CloudFront Works:

CloudFront caches copies of your content at a network of global data centers called **Edge Locations**. When a user requests content, the request is routed to the nearest Edge Location, drastically reducing latency by removing the need to traverse back to the origin server.

#### Core Definitions:

* **Origin:** The source of truth for your content. Can be an Amazon S3 bucket, an Elastic Load Balancer (ELB), or an on-premises web server.
* **Distribution:** The configuration unit for CloudFront, tied to a unique DNS domain name (e.g., `d111111abcdef8.cloudfront.net`).
* **Cache Behavior:** Defines rules for how CloudFront handles content caching (e.g., time-to-live expiration windows, query string forwarding).



## 6. Hybrid Cloud Connectivity

Organizations often need to bridge their existing on-premises data centers with their AWS virtual network environments.

### AWS Site-to-Site VPN

* **Mechanism:** Establishes a secure, encrypted tunnel over the public internet between your on-premises network and your VPC.
* **Components:** Utilizes a **Customer Gateway** (physical hardware or software appliance on your side) and a **Virtual Private Gateway (VGW)** or **AWS Transit Gateway** on the AWS side.
* **Pros/Cons:** Fast to provision and cost-effective, but network performance is dependent on public internet congestion because traffic travels over the public internet.

### AWS Direct Connect (DX)

* **Mechanism:** Bypasses the public internet entirely by establishing a dedicated, private physical network connection from your premises directly into AWS infrastructure.
* **Pros/Cons:** Provides consistent network performance, increased throughput (1Gbps, 10Gbps, or 100Gbps ports), and reduced telecommunication costs for high-volume data transfers. However, it takes weeks or months to provision through telco providers.

| Feature | AWS Site-to-Site VPN | AWS Direct Connect |
| --- | --- | --- |
| **Path Medium** | Public Internet (Encrypted) | Dedicated Private Physical Line |
| **Encryption** | IPSec Encrypted by default | Not encrypted by default (Can layer VPN on top) |
| **Setup Time** | Minutes | Weeks to Months |
| **Performance** | Variable (dependent on ISP congestion) | Highly predictable, ultra-low latency |



## 7. Architectural Alignment: Well-Architected Framework

### Reliability Pillar

* **Design Pattern:** Deploy infrastructure into multiple subnets spanning across at least two distinct Availability Zones inside your VPC.
* **Implementation:** Couple multi-AZ subnets with an **Elastic Load Balancer (ELB)** to automatically distribute incoming traffic evenly across healthy targets, protecting applications from localized infrastructure failures.

### Performance Efficiency Pillar

* **Design Pattern:** Use **Amazon CloudFront** to cache static assets close to end-users at global Edge Locations.
* **Implementation:** Offloading high-volume asset delivery (images, stylesheets, videos) to Edge Locations minimizes the computational workload on backend server infrastructure and optimizes content load times.

### Cost Optimization Pillar

* **Design Pattern:** Minimize data transfer expenses across regional availability zones.
* **Implementation:** Keep internal traffic localized within the same Availability Zone when possible, as cross-AZ data transfer within a region incurs billing charges. Use **VPC Endpoints (AWS PrivateLink)** to communicate with services like S3 or DynamoDB privately without using internet gateways or NAT gateways, saving on data processing costs.



## 8. Real-World Use Case Scenario

### Scenario: High-Traffic E-Commerce Platform

An e-commerce retailer wants to migrate their legacy platform to AWS. The architecture needs to handle flash sales safely, secure credit card transactions, and keep page load times low for customers across the globe.

#### Architectural Solution Blueprint:

1. **Network Segregation (VPC):** Configure a VPC with public and private subnets.
2. **Public Subnet Deployment:** Place **Application Load Balancers (ALB)** and a pool of **NAT Gateways** in the public subnets across three different Availability Zones.
3. **Private Subnet Deployment:** Place the core auto-scaling application EC2 instances and the primary database clusters (Amazon RDS) safely inside the private subnets. They cannot be reached directly from the internet.
4. **Traffic Security Filters:**
* Configure a **Network ACL** on the public subnet allowing ports 80/443 but explicitly blocking a list of known malicious IP addresses attacking the site.
* Attach a **Security Group** to the application servers that *only* accepts traffic originating from the security group of the Application Load Balancer.


5. **Global Optimization:** Place an **Amazon CloudFront** distribution in front of the application. Static product catalog images are cached directly at global Edge Locations. **Amazon Route 53** uses Latency-Based Routing to direct users to the optimal deployment entry point.

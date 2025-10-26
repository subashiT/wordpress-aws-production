# Production-Ready WordPress on AWS

A **highly available, scalable, and secure WordPress deployment** on AWS designed to handle large-scale legitimate traffic while maintaining performance, security, and cost-efficiency.

## Architecture Diagram
(![Alt Text](./diagrams/Diagram.drawio.png))


## Architecture Overview

This deployment follows the **AWS Well-Architected Framework** principles
**Operational Excellence**, **Security**, **Reliability**, **Performance Efficiency**, and **Cost Optimization**.

### Architecture Components

#### Global Entry Points
- **Amazon Route 53** – DNS management and domain routing  
- **AWS Global Accelerator** – Provides static IPs and routes user traffic via AWS global network for optimal latency

#### Security & Protection
- **AWS WAF** – Protects against SQL injection, XSS, and other web exploits  
- **Security Groups** – Stateful firewalls for EC2, RDS, and ALB  
- **Network ACLs** – Stateless subnet-level filtering  

#### Application Layer
- **Application Load Balancer (ALB)** – Distributes traffic across EC2 instances, handles SSL termination  
- **Auto Scaling Group** – Automatically scales EC2 instances based on CPU usage  
- **Amazon EC2** – WordPress application servers (private subnets)

#### Storage & Database
- **Amazon EFS (Elastic File System)** – Shared storage for `wp-content` across instances  
- **Amazon RDS MySQL** – Managed database with Multi-AZ replication  

#### Network Architecture
| Component | CIDR | Description |
|------------|-------|-------------|
| **VPC** | 10.0.0.0/16 | Isolated virtual network |
| **Public Subnet** | 10.0.1.0/24 | ALB only |
| **Private App Subnet** | 10.0.2.0/24 | EC2 instances |
| **Private Storage Subnet** | 10.0.3.0/24 | EFS |
| **Private DB Subnet** | 10.0.4.0/24 | RDS |

---

## Traffic Flow

1. User requests domain via **Route 53**  
2. **Global Accelerator** routes to nearest AWS edge  
3. **AWS WAF** filters malicious traffic  
4. **ALB** distributes traffic to healthy EC2 instances  
5. **EC2** serves WordPress from shared **EFS** and **RDS**

---

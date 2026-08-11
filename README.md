# Web Application Deployment on AWS EC2

## Project Overview

Set up hosting for a web application on AWS by provisioning EC2 instances, configuring security groups and key pairs for secure SSH access, and setting up DNS routing using Route 53 for public accessibility.

## Flow

1. **Users** access the application over **HTTPS**.
2. Traffic hits the **Application Load Balancer (ALB)**, which is protected by its own Security Group.
3. The ALB forwards traffic on **port 8080** to **Tomcat instances** running inside an **Auto Scaling Group**.
4. Tomcat instances connect to backend services within a private Security Group:
   - **MySQL** — application database
   - **Memcache** — caching layer
   - **RabbitMQ** — message queue
5. **Amazon Route 53** manages DNS, including **private hosted zones** for internal service discovery:
   - `db01` → MySQL instance IP
   - `mc01` → Memcache instance IP
   - `rmq01` → RabbitMQ instance IP
6. An **S3 Bucket** stores build/deployment artifacts.

## AWS Services Used

| Service | Purpose |
|---|---|
| EC2 | Hosting Tomcat, MySQL, Memcache, and RabbitMQ instances |
| Security Groups | Controlling inbound/outbound traffic between tiers |
| Application Load Balancer (ALB) | Distributing HTTP traffic across Tomcat instances |
| Auto Scaling Group | Automatically scaling Tomcat instances based on demand |
| Route 53 | Public DNS + private hosted zones for internal service discovery |
| S3 | Storing build artifacts |

## Key Configuration Steps

1. **Security Groups & Key Pairs** — Created isolated security groups for the ALB, app tier, and backend tier; generated key pairs for SSH access.
2. **EC2 Instances** — Launched instances for Tomcat (app), MySQL, Memcache, and RabbitMQ.
3. **Route 53** — Configured public DNS for the application domain and private DNS zones for internal service resolution (db01, mc01, rmq01).
4. **Build & Deploy Artifacts** — Built the application and uploaded artifacts to S3, then deployed to Tomcat instances.
5. **Load Balancer & DNS** — Set up an Application Load Balancer with a target group pointing to the Tomcat Auto Scaling Group; linked it to Route 53.
6. **Auto Scaling Group** — Configured min/max/desired capacity so Tomcat instances scale automatically based on load.

## Notes

- Application resources (EC2 instances, ALB, Auto Scaling Group) were **cleaned up after project completion** to avoid ongoing AWS charges. All configuration and setup steps are documented here and in the screenshots for future reference.



# DevOps Inventory Management System (AWS Deployment)

This project focuses on the **Solutions Architecture** and **Cloud Deployment** of a full-stack inventory management dashboard. The goal of this repository is to demonstrate a production-grade AWS infrastructure setup, encompassing networking, secure database management, and scalable compute services.

## Architecture Overview

The application follows a traditional 3-tier architecture deployed on AWS:

* **Frontend Tier:** Next.js application hosted on **AWS Amplify** for global scaling and managed CI/CD.
* **Application Tier:** Node.js/Express backend running on an **Amazon EC2 (T2 Micro)** instance within a Public Subnet.
* **Database Tier:** **Amazon RDS (PostgreSQL)** instance located in a Private Subnet for maximum security.
* **Storage Tier:** **Amazon S3** for persistent storage of product images and assets.
* **Connectivity:** **Amazon API Gateway** acts as the secure bridge between the frontend and the backend EC2 instance, providing HTTPS termination.

## 🌐 Networking Details (VPC Configuration)

The infrastructure is built within a custom Virtual Private Cloud (VPC) to ensure complete isolation and control:

* **CIDR Block:** `10.0.0.0/16`
* **Public Subnet:** Hosts the EC2 backend. Associated with an **Internet Gateway** and a Public Route Table [[06:03:51](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=21831)].
* **Private Subnets:** Two subnets in different Availability Zones (Multi-AZ) to host the RDS database [[06:31:56](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=23516)].
* **Security Groups:**
* **Public SG (EC2):** Allows inbound HTTP/HTTPS and SSH for administration [[06:13:27](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=22407)].
* **Private SG (RDS):** Restricts inbound traffic strictly to the EC2 security group on Port 5432 [[06:37:15](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=23835)].



## 🚀 Deployment Steps

### 1. Database Setup (RDS)

* Deploy a PostgreSQL instance using the **Free Tier** template [[06:34:00](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=23640)].
* Configure a **DB Subnet Group** using the two private subnets.
* Initialize the schema using **Prisma** migrations from the EC2 instance [[06:41:53](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24113)].

### 2. Backend Compute (EC2)

* Launch an Amazon Linux 2023 instance [[06:10:17](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=22217)].
* Install environment dependencies: **Node.js (NVM)** and **Git** [[06:15:04](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=22504)].
* Utilize **PM2** (Process Manager) to ensure the Node.js server remains running after reboots or crashes [[06:26:28](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=23188)].

### 3. API Gateway Integration

* Create an **HTTP API** to route requests from the Amplify frontend to the EC2 public IP [[06:48:44](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24524)].
* Enables HTTPS communication for the frontend to avoid "Mixed Content" security blocks [[06:52:03](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24723)].

### 4. Static Assets (S3)

* Bucket created for product images with **Public Access** enabled specifically for the asset folder [[06:53:14](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24794)].
* Configured **Bucket Policies** to allow `s3:GetObject` for public image rendering [[06:55:12](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24912)].

### 5. Frontend & CI/CD (Amplify)

* Connected the GitHub repository to **AWS Amplify** [[06:46:06](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24366)].
* Configured build settings to target the `/client` directory in the monorepo [[06:46:44](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24404)].
* Environmental variables mapped to the API Gateway Invoke URL [[06:47:14](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=24434)].

## 🛠️ Tech Stack

* **Cloud:** AWS (EC2, RDS, S3, VPC, API Gateway, Amplify)
* **Backend:** Node.js, Express, Prisma ORM
* **Frontend:** Next.js, Tailwind CSS, Redux Toolkit Query
* **Database:** PostgreSQL
* **DevOps:** Git, PM2

## 🔒 Security Best Practices Implemented

* **Least Privilege:** Security groups are scoped to specific ports and source IDs [[06:37:15](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=23835)].
* **Data Isolation:** Database is not reachable from the public internet [[06:35:11](http://www.youtube.com/watch?v=ddKQ8sZo_v8&t=23711)].
* **Secure Secrets:** Database credentials managed via `.env` files (simulating AWS Secrets Manager behavior).
* **HTTPS:** Encrypted transit for all user-facing data via API Gateway.

---

*This README was generated based on the project architecture detailed in [EdRoh's project tutorial](https://www.google.com/search?q=https://youtu.be/ddKQ8sZo_v8).*
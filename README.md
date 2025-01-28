# 4-Tier Architecture Project Documentation

## Project Overview

This project demonstrates the implementation of a 4-tier architecture using AWS services. The design ensures scalability, security, and efficient communication between the tiers. The flow of the architecture is structured as follows:

1. **First Tier**: Jump Server (Public Subnet)
2. **Second Tier**: Web Tier (Web Server in a Private Subnet)
3. **Third Tier**: App Tier (Application Logic in a Private Subnet)
4. **Fourth Tier**: Database Tier (Amazon RDS in a Private Subnet)
5. **Additional Components**:
   - S3 Bucket for object storage.
   - Internet-facing Elastic Load Balancer in the public subnet.
   - Internal Load Balancer for App Tier.
   - Auto Scaling Group for high availability and scalability.

---

## Services Used

### 1. **Amazon EC2**

- **Purpose**: To host the Jump Server, Web Server, and App Server.
- **Configuration**:
  - Jump Server: Deployed in the public subnet to allow secure SSH access to other instances in private subnets.
  - Web Server: Hosted on Nginx to serve static and dynamic content.
  - App Server: Runs the application logic (e.g., PHP backend).
- **Security Groups**:
  - Jump Server allows SSH access from a specific IP range.
  - Web Server allows HTTP traffic from the Internet-facing Load Balancer.
  - App Server allows traffic only from the internal Load Balancer.

### 2. **Amazon RDS**

- **Purpose**: To manage the database tier securely and efficiently.
- **Configuration**:
  - Engine: MySQL
  - Deployment: Private subnet with no public access.
  - Security Group: Allows traffic only from the App Server.
- **Features**:
  - Automated backups
  - Multi-AZ deployment for high availability

### 3. **Amazon S3**

- **Purpose**: To store objects (e.g., images, files) uploaded through the application.
- **Features**:
  - Scalable and durable storage.

### 4. **Elastic Load Balancers (ELB)**

- **Purpose**: To distribute traffic and ensure high availability.
- **Configuration**:
  - **Internet-facing Load Balancer**:
    - Deployed in the public subnet.
    - Handles incoming requests using its DNS.
    - Routes traffic to Web Tier instances.
  - **Internal Load Balancer**:
    - Deployed in the private subnet.
    - Routes traffic from Web Tier to App Tier instances.
- **Features**:
  - Integrated with Auto Scaling to handle varying traffic loads.
  - Health checks ensure only healthy instances receive traffic.

### 5. **Auto Scaling Group**

- **Purpose**: To automatically scale the number of instances in both the Web Tier and App Tier based on traffic.
- **Configuration**:
  - Separate Auto Scaling Groups for Web Tier and App Tier.
  - Minimum, maximum, and desired instance counts defined for each tier.
  - Scaling policies triggered by CloudWatch alarms (e.g., CPU utilization).

### 6. **VPC (Virtual Private Cloud)**

- **Purpose**: To create a logically isolated network for the infrastructure.
- **Configuration**:
  - **Subnets**:
    - Public Subnet: Hosts the Jump Server and Internet-facing Load Balancer.
    - Private Subnets:
      - Web Tier Subnet
      - App Tier Subnet
      - Database Tier Subnet
  - **Route Tables**:
    - Public Subnet: Internet Gateway attached for public access.
    - Private Subnets: Use NAT Gateway for secure outbound traffic.

---

## Architecture Flow

1. Users send requests to the **Internet-facing Load Balancer** using its DNS name.
2. The **Internet-facing Load Balancer** routes traffic to Web Tier instances in the private subnet.
3. The **Web Tier** processes requests and forwards them to the Internal Load Balancer.
4. The **Internal Load Balancer** routes traffic to App Tier instances in the private subnet.
5. The **App Tier** processes application logic and interacts with the Database Tier for data storage and retrieval.
6. The **Database Tier (RDS)** handles the data layer in the private subnet.
7. The **S3 Bucket** is used by the application for storing and serving objects (e.g., images).
8. The **Auto Scaling Group** ensures both the Web Tier and App Tier scale dynamically based on demand.

---

## Steps to Build the Architecture

1. **Create a VPC and Subnets**

   - Define CIDR blocks for the VPC and subnets.
   - Create public and private subnets.
   - Attach an Internet Gateway to the VPC.
   - Configure route tables for public and private subnets.
   - Set up a NAT Gateway for private subnets.

2. **Set Up RDS**

   - Launch an RDS instance in a private subnet.
   - Configure security groups to allow traffic only from the App Tier.

3. **Launch EC2 Instances**

   - Create and configure the App Server and Web Server.
   - Assign appropriate security groups.

4. **Set Up Load Balancers and Auto Scaling**

   - Launch an Internal Load Balancer in the private subnet.
   - Launch an Internet-facing Load Balancer in the public subnet.
   - Create Auto Scaling Groups for both the Web Tier and App Tier.
   - Configure scaling policies based on traffic demand.

5. **Configure S3 Bucket**

   - Create an S3 bucket for storing uploaded objects.
   - Apply bucket policies and configure permissions.

6. **Test the Setup**

   - Verify communication between tiers.
   - Test the application’s functionality, including database access and file uploads to S3.
   - Simulate high traffic to test the Auto Scaling functionality.

---

## Conclusion

This 4-tier architecture provides a secure, scalable, and highly available solution for hosting web applications. By leveraging AWS services like EC2, RDS, S3, Elastic Load Balancers, and Auto Scaling, the design ensures fault tolerance, traffic management, and efficient resource utilization.


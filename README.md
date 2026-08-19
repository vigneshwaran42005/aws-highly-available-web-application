# AWS-Based Highly Available Web Application

## 📌 Project Overview

This project demonstrates the design and deployment of a highly available web application on AWS using a secure public and private network architecture.

The application uses an **Application Load Balancer** to distribute incoming traffic across two private Windows EC2 application servers running **IIS and Python Flask**. Student details submitted through the web application are securely stored in **Amazon Aurora PostgreSQL**.

The project demonstrates practical implementation of **AWS networking, load balancing, private infrastructure, application deployment, database integration, and security controls**.

---

## 🏗️ Architecture

### Application Flow

```text
User
  │
  ▼
Internet
  │
  ▼
Application Load Balancer
HTTP : 80
  │
  ├───────────────┐
  ▼               ▼
Private EC2-A   Private EC2-B
AZ-A            AZ-B
IIS + Flask     IIS + Flask
  │               │
  └───────┬───────┘
          ▼
  Aurora PostgreSQL
      Port 5432
          │
          ▼
   Student Details
```

---

## ☁️ AWS Services Used

| AWS Service                   | Purpose                                                         |
| ----------------------------- | --------------------------------------------------------------- |
| **Amazon VPC**                | Created the isolated cloud network                              |
| **Public Subnets**            | Hosted public-facing resources such as the ALB and Bastion Host |
| **Private Subnets**           | Hosted application EC2 instances                                |
| **Database Subnets**          | Provided private network placement for Aurora                   |
| **Amazon EC2**                | Hosted the Windows application servers                          |
| **Application Load Balancer** | Distributed traffic across application servers                  |
| **Target Groups**             | Registered and monitored EC2 application instances              |
| **Amazon Aurora PostgreSQL**  | Managed relational database                                     |
| **Internet Gateway**          | Provided Internet connectivity for public resources             |
| **NAT Gateway**               | Provided outbound Internet access for private resources         |
| **Security Groups**           | Controlled network access between application layers            |
| **Route Tables**              | Controlled traffic routing between subnets                      |
| **Bastion Host**              | Provided controlled access to private EC2 instances             |

---

## 🖥️ Application Layer

Two Windows EC2 instances were deployed in separate Availability Zones.

```text
EC2-A → Private Subnet → AZ-A
EC2-B → Private Subnet → AZ-B
```

Both servers were configured with:

* Windows Server
* IIS
* Python
* Flask

The application was exposed through the Application Load Balancer rather than directly exposing the private EC2 instances to the Internet.

---

## ⚖️ Load Balancing

An Internet-facing Application Load Balancer was configured across two public subnets.

```text
Internet
   ↓
ALB : 80
   ↓
Target Group
   ├── EC2-A → Healthy
   └── EC2-B → Healthy
```

Health checks were configured to verify application availability.

This provides redundancy at the application layer and allows traffic to be distributed between the two EC2 instances.

---

## 🗄️ Database Layer

Amazon Aurora PostgreSQL was used as the managed relational database.

**Database configuration:**

```text
Database Engine : Aurora PostgreSQL
Port            : 5432
Access          : Private
```

The database was configured without public access.

Student information is stored in the following table:

```text
student_details
```

Example:

```text
ID | Name    | Standard | Roll No
---|---------|----------|--------
1  | Vignesh | 12th     | 101
```

---

## 🔐 Security Architecture

Security Groups were configured to restrict communication between the application layers.

```text
Internet
   │
   ▼
ALB-SG
HTTP : 80
   │
   ▼
EC2-SG
HTTP : 80
   │
   ▼
Aurora-SG
PostgreSQL : 5432
```

The Aurora database was not directly exposed to the Internet.

The application servers communicate with the database using private network connectivity.

---

## 🌐 Bastion Host

A Bastion Host was deployed in the public subnet to provide controlled access to the private Windows EC2 instances.

```text
Administrator
     ↓
Bastion Host
     ↓
Private EC2-A / EC2-B
```

This avoids exposing the private application servers directly to the Internet for administrative access.

---

## 🧑‍💻 Application Features

The project includes a simple Student Details web application.

Users can enter:

* Student Name
* Standard
* Roll Number

### Data Flow

```text
Student Form
     ↓
Flask Backend
     ↓
PostgreSQL INSERT
     ↓
Aurora PostgreSQL
```

The application UI is designed primarily for data entry, while the submitted records are stored in the backend database.

---

## 🧪 Connectivity Testing

EC2-to-Aurora connectivity was validated using:

```powershell
Test-NetConnection <Aurora-Endpoint> -Port 5432
```

The connectivity test returned:

```text
TcpTestSucceeded : True
```

This confirmed successful network connectivity between the private application server and Aurora PostgreSQL.

---

## 🛠️ Technologies Used

### Cloud & Infrastructure

* AWS
* VPC
* EC2
* Application Load Balancer
* Target Groups
* Aurora PostgreSQL
* NAT Gateway
* Internet Gateway
* Route Tables
* Security Groups

### Application

* Python
* Flask
* HTML
* CSS
* IIS
* PostgreSQL

---

## 📁 Project Structure

```text
aws-highly-available-web-application/
│
├── app.py
├── check.py
├── requirements.txt
├── README.md
├── .gitignore
│
├── templates/
│   └── index.html
│
└── docs/
    └── architecture.png
```

---

## 🚀 How the Application Works

1. A user accesses the application through the Application Load Balancer.
2. The ALB forwards the request to a healthy private EC2 instance.
3. IIS and Flask process the request.
4. The Flask backend receives the student details.
5. Flask connects to Aurora PostgreSQL using private network connectivity.
6. Student details are inserted into the `student_details` table.
7. A successful response is returned to the user.

---

## 🔍 Key Learning Outcomes

Through this project, I gained hands-on experience with:

* AWS VPC and subnet design
* Public and private network architecture
* Multi-AZ deployment
* Application Load Balancing
* EC2 and Windows Server administration
* IIS configuration
* Python Flask application deployment
* Aurora PostgreSQL integration
* Security Group configuration
* Bastion Host access
* NAT Gateway and routing
* Private database connectivity
* End-to-end cloud application deployment

---

## 🎯 Project Outcome

Successfully deployed and validated a highly available AWS web application with:

```text
ALB
 ↓
Private EC2-A / EC2-B
 ↓
IIS + Flask
 ↓
Aurora PostgreSQL
```

The project demonstrates practical knowledge of **AWS Cloud Infrastructure, Networking, Security, Application Deployment, Load Balancing, and Database Integration**.

---

## 👨‍💻 Author

**Vignesh Waran**

AWS Cloud Engineer Aspirant | 2026 Graduate

---

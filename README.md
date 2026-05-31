<div align="center">

<img src="https://img.shields.io/badge/PrintDesk-Backend_API-1a1a1a?style=for-the-badge&logoColor=white" alt="PrintDesk API" />

# ⚙️ PrintDesk — Backend API

### Service Centre Management System — REST API
*Built with Spring Boot 3.x, PostgreSQL, and JWT Authentication*

[![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.x-6DB33F?style=flat-square&logo=springboot)](https://spring.io/projects/spring-boot)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-4169E1?style=flat-square&logo=postgresql)](https://www.postgresql.org)
[![Java](https://img.shields.io/badge/Java-21-ED8B00?style=flat-square&logo=openjdk)](https://openjdk.org)
[![JWT](https://img.shields.io/badge/JWT-Auth-000000?style=flat-square&logo=jsonwebtokens)](https://jwt.io)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

[Frontend Repo](https://github.com/AlgoArchitect3005/printdesk-ui) · [API Docs](#-api-reference) · [Report Bug](#) · [Request Feature](#)

</div>

---

## 📋 Table of Contents

- [About](#-about)
- [Features](#-features)
- [Tech Stack](#-tech-stack)
- [Getting Started](#-getting-started)
- [Project Structure](#-project-structure)
- [Environment Variables](#-environment-variables)
- [API Reference](#-api-reference)
- [Database Schema](#-database-schema)
- [Security](#-security)

---

## 🧾 About

**PrintDesk API** is the backend for the PrintDesk Service Centre Management System. It provides a secure REST API for managing job cards, customers, inventory, and invoices for a printer repair business.

> This repository contains the **Spring Boot backend**. The React frontend lives [here](https://github.com/AlgoArchitect3005/printdesk-ui).

---

## ✨ Features

- 🔐 **JWT Authentication** — Stateless, secure token-based auth
- 👥 **Role-Based Access Control** — `ROLE_ADMIN` and `ROLE_OPERATOR`
- 📋 **Job Card Management** — Full CRUD with status tracking
- 👤 **Customer Management** — Search by phone, full profile
- 🧰 **Inventory Management** — Stock tracking with low-stock alerts
- 🧾 **Invoice Generation** — Auto-calculate from parts + service charge
- 💳 **Payment Recording** — Track `UNPAID`, `PARTIAL`, `PAID`
- ⚡ **Custom Exception Handling** — Consistent error responses
- 🛡️ **Input Validation** — `@Valid` on all DTOs

---

## 🛠 Tech Stack

| Category | Technology |
|---|---|
| Framework | Spring Boot 3.x |
| Language | Java 21 |
| Database | PostgreSQL |
| ORM | Spring Data JPA + Hibernate |
| Security | Spring Security + JWT (jjwt 0.12.3) |
| Build Tool | Maven |
| Validation | Jakarta Bean Validation |
| Utilities | Lombok |

---

## 🚀 Getting Started

### Prerequisites

```bash
Java 21+
Maven 3.8+
PostgreSQL 14+
```

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/AlgoArchitect3005/printdesk-api-.git

# 2. Navigate to project directory
cd printdesk-api-

# 3. Create PostgreSQL database
createdb printdesk_db

# 4. Configure environment variables
cp .env.example .env
# Edit .env with your DB credentials and JWT secret

# 5. Run the application
mvn spring-boot:run
```

API will be running at `http://localhost:8080`

---

## 📁 Project Structure

```
src/main/java/com/servicecentre/api/
├── controller/
│   ├── AuthController.java
│   ├── CustomerController.java
│   ├── JobCardController.java
│   ├── InventoryController.java
│   └── InvoiceController.java
├── service/
│   ├── AuthService.java
│   ├── CustomerService.java
│   ├── JobCardService.java
│   ├── InventoryService.java
│   └── InvoiceService.java
├── repository/
│   ├── UserRepository.java
│   ├── CustomerRepository.java
│   ├── JobCardRepository.java
│   ├── InventoryRepository.java
│   ├── JobPartRepository.java
│   └── InvoiceRepository.java
├── model/
│   ├── User.java
│   ├── Customer.java
│   ├── JobCard.java
│   ├── Inventory.java
│   ├── JobPart.java
│   └── Invoice.java
├── dto/
├── security/
│   ├── JwtUtil.java
│   ├── JwtFilter.java
│   └── SecurityConfig.java
└── exception/
    ├── GlobalExceptionHandler.java
    └── ResourceNotFoundException.java
```

---

## 🔑 Environment Variables

Create a `.env` file or set in `application.properties`:

```properties
# Database
spring.datasource.url=jdbc:postgresql://localhost:5432/printdesk_db
spring.datasource.username=your_db_username
spring.datasource.password=your_db_password

# JPA
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=false

# JWT
jwt.secret=your_super_secret_key_minimum_32_characters
jwt.expiration=86400000
```

> ⚠️ **Never commit real credentials to GitHub.**

---

## 📡 API Reference

### Auth

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/auth/login` | Public | Login, returns JWT |
| `POST` | `/api/auth/register` | Public | Register admin |

### Customers

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/customers` | Admin | Get all customers |
| `GET` | `/api/customers/{id}` | All | Get customer by ID |
| `GET` | `/api/customers/phone/{phone}` | All | Search by phone |
| `POST` | `/api/customers` | All | Create customer |
| `PUT` | `/api/customers/{id}` | All | Update customer |
| `DELETE` | `/api/customers/{id}` | Admin | Delete customer |

### Job Cards

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/jobs` | All | Get all job cards |
| `GET` | `/api/jobs/{id}` | All | Get job by ID |
| `POST` | `/api/jobs/customer/{customerId}` | All | Create job card |
| `PATCH` | `/api/jobs/{id}/status` | All | Update job status |
| `POST` | `/api/jobs/{jobCardId}/parts` | All | Add part to job |
| `GET` | `/api/jobs/customer/{customerId}` | All | Jobs by customer |

### Inventory

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/inventory` | All | Get all items |
| `GET` | `/api/inventory/{id}` | All | Get item by ID |
| `GET` | `/api/inventory/low-stock` | Admin | Low stock items |
| `POST` | `/api/inventory` | Admin | Add inventory item |
| `PUT` | `/api/inventory/{id}` | Admin | Update item |
| `DELETE` | `/api/inventory/{id}` | Admin | Delete item |

### Invoices

| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/invoices/job/{jobId}/generate` | All | Generate invoice |
| `GET` | `/api/invoices/{id}` | All | Get invoice |
| `PATCH` | `/api/invoices/{id}/payment` | All | Record payment |

---

## 🗄️ Database Schema

```
User ──────────────────────────────────────┐
  id, username, password, role             │
                                           │
Customer                                   │
  id, name, phone, email, address          │
       │                                   │
       └──── JobCard ────────────────── assigned_to (User)
               id, jobCardNumber,
               printerBrand, printerModel,
               problemDescription, status,
               estimatedCost, createdAt
                    │
                    ├──── JobPart
                    │       id, quantity,
                    │       unitPriceAtTime
                    │            │
                    │            └──── Inventory
                    │                   id, itemName,
                    │                   quantity, unitPrice,
                    │                   reorderLevel
                    │
                    └──── Invoice
                            id, partsTotal,
                            serviceCharge,
                            grandTotal,
                            amountPaid,
                            paymentStatus
```

---

## 🔒 Security

- Passwords hashed with **BCrypt**
- All endpoints protected via **JWT filter**
- Role-based method-level security with `@PreAuthorize`
- CORS configured for frontend origin only
- Input validation on all request bodies

---

## 👤 Author

**Yash Gupta**
- GitHub: [@AlgoArchitect3005](https://github.com/AlgoArchitect3005)

---

<div align="center">

Made with ❤️ for PrintDesk

⭐ Star this repo if you found it helpful!

</div>

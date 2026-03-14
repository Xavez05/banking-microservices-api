# 🏦 Banking Microservices System

![Java](https://img.shields.io/badge/Java-21-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-3.5.6-6DB33F?style=for-the-badge&logo=springboot&logoColor=white)
![RabbitMQ](https://img.shields.io/badge/RabbitMQ-FF6600?style=for-the-badge&logo=rabbitmq&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apachemaven&logoColor=white)

A distributed banking system built with a **microservices architecture** using Spring Boot 3, Java 21, RabbitMQ for async communication, and Docker Compose for orchestration.

---

## 📐 Architecture Overview

```
                        ┌─────────────────────────────────┐
                        │         Docker Compose           │
                        │                                  │
         ┌──────────────┤   ┌────────────┐                │
         │  REST API    │   │  RabbitMQ  │                │
         ▼              │   │  (Broker)  │                │
┌─────────────────┐     │   └─────┬──────┘                │
│  Customers      │─────┼─────────┤  customer.created     │
│  Service :8081  │     │         ▼                        │
│                 │     │   ┌─────────────────┐            │
│  - CRUD clients │     │   │  Accounts       │            │
│  - Publish      │     │   │  Service :8082  │            │
│    events       │     │   │                 │            │
└────────┬────────┘     │   │  - Accounts     │            │
         │              │   │  - Transactions │            │
         │              │   │  - Reports      │            │
         ▼              │   └────────┬────────┘            │
┌─────────────────┐     │            │                     │
│  MySQL DB       │     │   ┌────────▼────────┐            │
│  (customers)    │     │   │  MySQL DB       │            │
└─────────────────┘     │   │  (accounts)     │            │
                        │   └─────────────────┘            │
                        └─────────────────────────────────┘
```

---

## 🧩 Services

### 👤 Customers Service — `localhost:8081`
Handles client lifecycle and publishes events to RabbitMQ when a new customer is created.

**Package structure:**
```
com.bank.customers
 ├── config
 ├── controller
 ├── events
 ├── model
 ├── repository
 ├── service
 └── CustomersApplication.java
```

### 💳 Accounts Service — `localhost:8082`
Manages bank accounts, processes transactions (deposits & withdrawals), validates balances, and generates financial reports by date range.

**Package structure:**
```
com.bank.accounts
 ├── config
 ├── controller
 ├── exception
 ├── listener
 ├── model
 ├── repository
 ├── service
 └── AccountsApplication.java
```

---

## 🔗 Async Communication — RabbitMQ

| Service | Action | Exchange | Routing Key |
|---|---|---|---|
| Customers | Publishes event on customer creation | `customers.exchange` | `customer.created` |
| Accounts | Listens and syncs customer data | `customers.exchange` | `customer.created` |

---

## 🚀 Getting Started

### Prerequisites
- Java 21
- Maven 3.9+
- Docker & Docker Compose

### Build
```bash
mvn clean package -DskipTests
```

### Run with Docker
```bash
docker-compose up --build
```

### Service URLs

| Service | URL |
|---|---|
| Customers API | http://localhost:8081 |
| Accounts API | http://localhost:8082 |
| RabbitMQ Dashboard | http://localhost:15672 |

> RabbitMQ credentials: `guest` / `guest`

---

## 📬 Postman Collection

A ready-to-use Postman collection is included in the root of the project:

```
postman_collection.json
```

Import it directly into Postman to test all available endpoints.

---

## 🧪 Running Tests

```bash
mvn test
```

Tests are written with **JUnit 5 + Mockito** and validate business logic without loading the full Spring context.

**Coverage includes:**
- Customer creation with RabbitMQ event publishing
- Transaction registration with balance validation
- Insufficient balance exception handling

---

## 🗄️ Database Schema

The SQL schema is included in the root of the project:

```
banking_schema.sql
```

Each microservice has its own dedicated MySQL database following the **Database per Service** pattern.

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Language | Java 21 (LTS) |
| Framework | Spring Boot 3.5.6 |
| Persistence | Spring Data JPA |
| Messaging | Spring AMQP · RabbitMQ |
| Database | MySQL / MariaDB |
| Containerization | Docker · Docker Compose |
| Testing | JUnit 5 · Mockito |
| Build Tool | Maven 3.9.9 |

---

## 👨‍💻 Author

**Antony Chávez** — Backend Developer  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/antonychavez)
[![GitHub](https://img.shields.io/badge/GitHub-Xavez05-181717?style=flat&logo=github&logoColor=white)](https://github.com/Xavez05)

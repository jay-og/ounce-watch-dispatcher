# Ounce-Watch Dispatcher

## Overview

The **Ounce-Watch Dispatcher** is a mission-critical microservice built with **NestJS** and **PostgreSQL**. It handles live pricing for **10 precious metals** (e.g., Gold, Silver, Platinum) across a **multi-tenant architecture**.

### The Challenge

Every **15 minutes**, a live market feed provides updated **spot prices**. The system must:
- Calculate **Tenant-Specific** prices based on individualised **spread rules** (markup).
- Identify **high-volatility risks** when prices experience significant swings (>2%).
- Maintain **data integrity** and optimal **system performance** throughout.

---

## Technical Requirements & Constraints

### Framework
- **NestJS** (TypeScript)

### Database
- **PostgreSQL**
- Candidate's choice of ORM: **TypeORM** or **Prisma**

### Multi-Tenancy
- Designed to support **thousands of unique clients** (e.g., Banks, Jewelers, Hedge Funds).

### Market Feed
- Updates every **15 minutes** with live spot pricing.

---

### Phase A: Architecture & Concurrency

#### 1. **Instance Collision**
If the system is scaled to **5 instances** of this NestJS application for High Availability:
- **Question:** How would you ensure that only **one instance** processes the **15-minute price update** to avoid race conditions or double-updates?

#### 2. **Horizontal Scaling**
As the number of tenants ($N$) grows to **100,000+**:
- **Question:** What measures would you implement to ensure the **processing loop** does not exceed the **15-minute interval** or exhaust server resources (like RAM)?

---

### Phase B: Database & Data Integrity

#### 1. **The ACID Test**
If the price update process fails halfway through (e.g., at tenant #5,000):
- **Question:** How do you ensure the **database** is not left in a "partial state," where some tenants have **old prices** and others have **new prices**?

#### 2. **Tenant Isolation**
In a shared **PostgreSQL** database:
- **Question:** What strategies can ensure **Tenant A's custom spread rules** are never leaked or exposed to **Tenant B's calculation logic**?

---

### Phase C: Big O & Logic

#### 1. **Optimization**
Given the problem's complexity of **O(N × M)**:
- **Question:** With **10 metals** and $N$ tenants already being processed, how would you scale the architecture to handle **10,000 metal assets** efficiently?

#### 2. **Market Volatility**
To detect a **>2% swing** in prices:
- **Question:** Would you calculate this during the **write-phase** or as a **background event**? What are the tradeoffs for each approach?

# 🤝 Nasiba Foundation — Full-Stack Welfare & Donor Platform

> **Full-stack humanitarian relief web application, dynamic campaign donation engine, and 4-tier administrative ERP built for Nasiba Foundation.**

[![Case Study](https://img.shields.io/badge/Engineering-Case_Study-007ACC?style=for-the-badge&logo=github&logoColor=white)](#-system-architecture)
[![Next.js](https://img.shields.io/badge/Next.js-App_Router-black?style=flat-square&logo=next.js&logoColor=white)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Prisma ORM](https://img.shields.io/badge/Prisma_ORM-Database_First-2D3748?style=flat-square&logo=prisma&logoColor=white)](https://www.prisma.io/)
[![JWT Security](https://img.shields.io/badge/Auth-Jose_JWT_+_Bcrypt-000000?style=flat-square)](https://github.com/karnVen)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)

---

## 🧭 Executive Summary & Social Impact

Nasiba Foundation is a grassroots non-profit organization dedicated to emergency humanitarian aid, community healthcare, and underprivileged education.

The organization suffered from manual operational bottlenecks: donations were recorded in paper ledgers, and staff attendance/expenses were tracked across disorganized chat groups. This full-stack application provides:
1. **Dynamic Welfare Campaigns:** Real-time campaign tracking (`/campaigns`) with celebratory canvas-confetti donation feedback.
2. **Community Education Programs:** Dynamic programmatic routing for long-term initiatives (`/programs/[slug]`).
3. **Internal Administrative ERP:** Secure role-based staff management covering attendance logging, leave approvals, and salary disbursement tracking.

---

## 🏛️ System Architecture

```mermaid
graph TD
    Citizen([Donor / Citizen]) --> PublicTier[Next.js App Router (Public Tier)]
    Staff([Foundation Staff / HR / Admin]) --> AdminTier[Secure Administrative ERP Tier]
    
    subgraph Public Portal
        PublicTier --> Campaigns[/campaigns - Welfare Drives]
        PublicTier --> Programs[/programs/[slug] - Education & Relief]
        PublicTier --> Donate[/donate - Interactive Giving Wizard]
        Donate --> Confetti[Canvas Confetti Celebration]
    end
    
    subgraph Administrative ERP
        AdminTier --> Auth[Stateless JWT Middleware (jose + bcryptjs)]
        Auth --> HR[Attendance & Leave Approval Engine]
        Auth --> Finance[Expense & Salary Status Tracker]
    end
    
    subgraph Unified Data Tier (Prisma ORM)
        Donate --> Prisma[(Prisma Client / SQLite & PostgreSQL)]
        HR --> Prisma
        Finance --> Prisma
    end
```

---

## ⚡ The Hard Engineering Challenges Conquered

### 1. Unified Public & Administrative Prisma Schema
- **The Challenge:** Building separate backends for donor fundraising and internal staff management would double maintenance overhead for a resource-constrained non-profit.
- **The Solution:** Engineered a cohesive, type-safe Prisma schema with typed enums (`Role`: ADMIN, HR, EMPLOYEE, MEMBER; `AttendanceStatus`: PRESENT, ABSENT, HALF_DAY; `SalaryStatus`: PAID, PENDING), powering both public campaigns and internal ERP operations from a single codebase.

### 2. Cross-Platform Binary Target Compatibility in Prisma
- **The Challenge:** Local development on Windows WSL collided with Linux server runtimes (RHEL, Debian) due to native OpenSSL binary mismatches in Prisma Client.
- **The Solution:** Configured multi-architecture binary targets in `schema.prisma` (`["native", "rhel-openssl-3.0.x", "debian-openssl-3.0.x"]`), guaranteeing zero-friction builds across all development and production environments.

### 3. Stateless JWT Authentication with Edge Security
- **The Challenge:** Session-based authentication requires stateful server memory that scales poorly on modern serverless runtimes.
- **The Solution:** Implemented lightweight stateless JWT authentication using `jose` with salted `bcryptjs` password hashing, securing administrative ERP routes via Next.js middleware.

---

## 📈 Developer Transformation & Growth

| Dimension | Before Nasiba Project | After Delivering Platform |
| :--- | :--- | :--- |
| **Application Architecture** | Static HTML landing pages | Full-stack Next.js App Router with unified database tier |
| **Data Persistence** | Manual files / spreadsheets | Production Prisma ORM with type-safe migrations and seeds |
| **Authentication** | Basic credentials | Stateless JWT tokens with role-based route middleware |
| **Business Scope** | Simple UI layouts | Integrated charity ERP managing attendance, payroll & donations |

---

## 🔒 Confidentiality & Intellectual Property Notice
*Donor tax records, personal identification numbers, and foundation financial disbursement ledgers remain strictly confidential. This repository serves as an open architectural case study.*

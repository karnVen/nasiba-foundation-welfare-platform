# ◈ Nasiba Foundation — Full-Stack Welfare Platform & Staff ERP

> **Production Platform Case Study**  
> A full-stack humanitarian relief web application, dynamic donation engine, and 4-tier administrative ERP engineered for **Nasiba Foundation** (registered charitable trust).

[![Case Study](https://img.shields.io/badge/Full--Stack-Case_Study-1e293b?style=for-the-badge&logo=gitbook&logoColor=white)](#-system-architecture--request-lifecycle)
[![Next.js](https://img.shields.io/badge/Next.js-App_Router-18181b?style=flat-square&logo=next.js&logoColor=white)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-27272a?style=flat-square&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Prisma ORM](https://img.shields.io/badge/Prisma_ORM-Database_First-334155?style=flat-square&logo=prisma&logoColor=white)](https://www.prisma.io/)
[![JWT Security](https://img.shields.io/badge/Auth-Jose_JWT_+_Bcrypt-1e293b?style=flat-square)](https://github.com/karnVen)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3f3f46?style=flat-square&logo=tailwind-css&logoColor=white)](https://tailwindcss.com/)

---

> [!IMPORTANT]
> **About This Repository**  
> This repository is a **technical case study and architectural showcase** of an active full-stack non-profit application.
> - **Why this structure?** To protect donor personal details, tax exemption PAN numbers, and staff payroll records, the production database and administrative authentication keys remain protected in the foundation's private repository.
> - **What you will find here:** Unified public and ERP architecture specifications, typed Prisma data models, stateless JWT authentication flows, and dynamic campaign management designs.

---

## ◈ Key Platform Capabilities & Modules

| Module | Purpose | Tech Implementation |
| :--- | :--- | :--- |
| **Welfare Campaigns** | Real-time campaign tracking & donation flow | Dynamic `/campaigns` routes with celebratory canvas-confetti feedback |
| **Community Programs** | Long-term educational academy & healthcare discovery | Dynamic programmatic routing via `/programs/[slug]` |
| **Staff Attendance ERP** | Daily attendance tracking for on-ground foundation workers | Type-safe status enums: PRESENT, ABSENT, HALF_DAY, ON_LEAVE |
| **HR & Leave Approvals** | Volunteer & employee leave request management | Three-state approval pipeline: PENDING, APPROVED, REJECTED |
| **Payroll & Expenses** | Administrative financial audit & expense disbursement tracking | Typed tracking: PAID, PENDING with timestamped logs |

---

## ◈ Tech Stack & Tooling

- **Core Framework:** Next.js (App Router, Server Components)
- **Language:** TypeScript (Full-Stack Strict Mode)
- **Database & ORM:** Prisma ORM (`@prisma/client`) with SQLite / PostgreSQL support
- **Authentication & Security:** Stateless JWT validation (`jose`) paired with salted `bcryptjs` password hashing
- **Styling & UI:** Tailwind CSS, Lucide React, Canvas Confetti

---

## ◈ System Architecture & Request Lifecycle

```mermaid
flowchart TD
    Donor([Donor / Citizen]) --> PublicTier[Next.js App Router - Public Web]
    Staff([Foundation Staff / HR / Admin]) --> AdminTier[Next.js App Router - Secure ERP]

    subgraph Public Welfare Portal
        PublicTier --> Campaigns[/campaigns - Welfare Drives]
        PublicTier --> Programs[/programs/[slug] - Education & Relief]
        PublicTier --> Donate[/donate - Interactive Giving Flow]
        Donate --> Confetti[Canvas Confetti Celebration]
    end

    subgraph Administrative ERP
        AdminTier --> AuthMiddleware[Stateless JWT Middleware - jose + bcryptjs]
        AuthMiddleware --> AttendanceEngine[Staff Attendance Tracking]
        AuthMiddleware --> LeaveEngine[HR Leave Approval Engine]
        AuthMiddleware --> ExpenseEngine[Financial Disbursements & Salary Status]
    end

    subgraph Data Tier - Prisma ORM
        Donate --> PrismaClient[(Prisma ORM Client)]
        AttendanceEngine --> PrismaClient
        LeaveEngine --> PrismaClient
        ExpenseEngine --> PrismaClient
    end
```

---

## ◈ Production Codebase Directory Layout

```text
nasiba-foundation/
├── app/
│   ├── (public)/                  # Citizen-facing routes
│   │   ├── page.tsx               # Foundation mission, active drives & impact metrics
│   │   ├── campaigns/             # Real-time welfare drives & funding goals
│   │   ├── programs/[slug]/       # Dedicated program landing pages (Education, Relief)
│   │   ├── events/                # Community welfare event registrations
│   │   └── donate/                # Multi-step donation wizard with celebratory feedback
│   ├── admin/                     # Administrative ERP portal
│   │   ├── attendance/            # Employee daily attendance logging
│   │   ├── leaves/                # Staff leave approval workflows
│   │   └── finances/              # Foundation operational expenses & salary registers
│   └── api/                       # Next.js API route handlers for auth and data mutations
├── prisma/
│   ├── schema.prisma              # Unified database schema with enums & relations
│   └── seed.ts                    # Automated database seed script for development
├── components/                    # UI elements, campaign cards, modal dialogs
└── lib/
    ├── auth.ts                    # JWT token creation, signature verification, and session cookies
    └── prisma.ts                  # Global Prisma Client singleton instance
```

---

## ◈ Hard Engineering Battles & Problem Diagnosis

### 1. Unified Public & Administrative Prisma Schema
* **Problem:** Running separate codebases for public fundraising and internal staff management would double maintenance costs for a non-profit organization.
* **Solution:** Engineered a unified, type-safe Prisma schema. Modeled distinct organizational roles (`ADMIN`, `HR`, `EMPLOYEE`, `MEMBER`) and operational enums (`AttendanceStatus`, `LeaveStatus`, `SalaryStatus`), powering the entire foundation from a single cohesive repository.

### 2. Cross-Platform Binary Target Compatibility in Prisma
* **Problem:** Local development on Windows WSL collided with Linux server runtimes (RHEL, Debian) due to native OpenSSL binary mismatches in Prisma Client.
* **Solution:** Configured multi-architecture binary targets in `schema.prisma` (`["native", "rhel-openssl-3.0.x", "debian-openssl-3.0.x"]`), ensuring zero-downtime builds across both local and production environments.

### 3. Stateless JWT Edge Authentication
* **Problem:** Traditional server-side sessions require persistent in-memory session stores (like Redis) that add unnecessary hosting costs.
* **Solution:** Implemented stateless, cryptographic JWT authentication using `jose` paired with `bcryptjs` password hashing, validated transparently at the edge via Next.js middleware.

---

## ◈ Professional Notice & Confidentiality Boundary
*This repository is maintained as a technical portfolio case study. Donor tax identities, personal contact records, and staff salary ledgers are strictly confidential under non-profit data protection policies.*

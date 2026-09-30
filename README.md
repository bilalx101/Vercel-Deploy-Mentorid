# 🚀 Multi-Tenant Enterprise SaaS Platform

A multi-tenant project management SaaS platform designed for strict organizational data isolation over a shared-schema PostgreSQL infrastructure.

Built for the **Full Stack Developer** technical assessment at **MENTROID**.

---

## 🌐 Live Deployment
* **Live Application:** [https://mentorid-project.vercel.app](https://mentorid-project.vercel.app)
* **GitHub Repository:** [https://github.com/bilalx101/Vercel-Deploy-Mentorid](https://github.com/bilalx101/Vercel-Deploy-Mentorid)

---

## 🔒 Multi-Tenancy Architecture (Core Isolation Logic)

The platform implements a **"Shared Database, Shared Schema"** multi-tenancy model with backend-enforced data partitioning:

1. **Cryptographic Identity Scoping:**
   * Requests never trust client-provided tenant identifiers.
   * On login, the authenticated user's `tenantId` and `role` are cryptographically signed into an `HttpOnly`, `SameSite` JWT session cookie.
2. **Query Isolation (Database Layer):**
   * Every Prisma query interacting with workspaces or initiatives strictly filters by `where: { tenantId: session.tenantId }`.
   * Cross-tenant data leakage or IDOR (Insecure Direct Object Reference) mutations are prevented at the database query level.
3. **Role-Based Access Control (RBAC):**
   * Workspace members can view, create, and modify projects.
   * Deletion is restricted to workspace `ADMIN` accounts with explicit HTTP 403 authorization checks on backend handlers.

---

## 🧪 Verification & Isolation Test Cases

To verify that tenant data is isolated:

### Test Case A: Alpha Corp
1. Navigate to `/register` and provision an organization:
   * **Company Name:** `Alpha Corp`
   * **Email:** `admin@alpha.com`
   * **Password:** `password123`
2. From the dashboard, create two projects (*Website Overhaul*, *Mobile App MVP*).
3. Confirm projects appear in the grid and open external resource links if provided.
4. Click **Logout**.

### Test Case B: Beta Corp (Strict Data Boundary)
1. Return to `/register` and create an independent workspace:
   * **Company Name:** `Beta Corp`
   * **Email:** `admin@beta.com`
   * **Password:** `password123`
2. **Observed Result:** Dashboard initializes with an empty state. Zero projects from Alpha Corp are rendered.
3. Add a dedicated initiative (*Beta Q4 Roadmap*).
4. Log out and re-authenticate as `admin@alpha.com`.
5. **Observed Result:** Only Alpha Corp's initial projects are returned; Beta Corp data remains invisible.

---

## 🛠️ Tech Stack & Infrastructure

* **Frontend:** Next.js (App Router), React, Tailwind CSS, Lucide Icons
* **Backend:** Next.js Route Handlers & Edge Middleware protection
* **Database:** PostgreSQL (Neon Serverless) managed via Prisma ORM
* **Authentication:** Custom JWT authentication with salted `bcrypt` password hashing and secure cookie delivery
* **Hosting:** Vercel (Edge network)

---

## 👤 Author
* **Developer:** Bilal
* **Email:** contactbilal500@gmail.com

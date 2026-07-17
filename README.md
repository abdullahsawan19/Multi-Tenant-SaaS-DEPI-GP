# 🚀 TeachBase

> **A Multi-Tenant SaaS Platform for Independent Teaching Academies**
> White-label academies, spun up in one click — one codebase, one Supabase backend, unlimited independent teachers.

[Live Demo](https://multi-tenant-saas-depi-gp.vercel.app/)

## 📌 Overview

Independent teachers are often stuck between building their own expensive infrastructure or surrendering their brand and student data to generic marketplaces. **TeachBase** solves this by providing white-label academy infrastructure. 

Every teacher gets a ready-made backend, a fully-branded storefront, and complete control over their pricing, content, and student relationships—without writing a single line of code.

## ✨ Key Features

### 🏢 Multi-Tenant Architecture
*   **One Codebase, Many Academies:** A single React application and one Supabase PostgreSQL database seamlessly power infinite, completely isolated academies .
*   **Data Isolation:** Every student, course, and enrollment is strictly scoped to a specific `tenant_id`, ensuring absolute data privacy .
*   **Dynamic White-Labeling:** The UI dynamically adapts to the tenant's brand (name, logo, theme) based on the academy slug.

### 👥 Role-Based Portals
The platform is divided into three isolated worlds :
*   **Super Admin Portal (`/super-admin`):** Global platform management, advanced analytics with **Recharts** (tracking revenue, tenant growth, and course statuses), and full CRUD for subscription plans and tenant suspension.
*   **Teacher Dashboard (`/dashboard`):** Complete academy control. Teachers can build complex curricula via the **Course Builder**, manage students, apply bulk discounts, and customize their storefront.
*   **Student Experience (`/:tenantId`):** A branded storefront for browsing courses and a robust, **Udemy-style Course Player** with auto-enrollment, video tracking (Cloudinary), and live progress bars.

### 🎨 Seamless Design System
*   Built with a custom **CSS Variable** token system.
*   Flawless, single-click **Dark/Light mode** that respects system preferences (`prefers-color-scheme`) and persists via local storage .

## 🛠 Tech Stack

*   **Frontend Framework:** React 19, Vite 
*   **Routing:** React Router v7 (Nested, Role-based, Lazy-Loaded) 
*   **State Management & Caching:** TanStack Query v5 
*   **Backend as a Service (BaaS):** Supabase (PostgreSQL, Auth, Storage) 
*   **UI & Styling:** Bootstrap 5, Custom CSS Variables, React Hot Toast 
*   **Data Visualization:** Recharts
*   **Media Management:** Cloudinary

## 🏗 Architecture & Data Flow

TeachBase strictly enforces a **Four-Layer Architecture** to ensure clean separation of concerns and highly maintainable code :

1.  **Pages & Layouts:** Shared chrome and route-specific UI (e.g., `CoursePlayer`, `AdminStats`).
2.  **Hooks (TanStack Query):** Custom hooks (e.g., `useCoursePlayer`, `useMyCourses`) manage all caching, loading, and error states .
3.  **Services Layer:** Pure asynchronous functions (e.g., `courseService.js`). **Rule: Components never talk to Supabase directly.** Only services import the Supabase client .
4.  **Database (Supabase):** Handles the query execution and Row-Level Security (RLS).

```text
UI Component ➡️ Custom Hook (TanStack) ➡️ Service Module ➡️ Supabase API
```
🗄️ Database Schema Highlights
The relational PostgreSQL schema utilizes UUIDs and strict Foreign Key constraints cascading from the tenants table to ensure data integrity:

-- tenants (Core academies)

-- users (Super Admins, Teachers, Students)

-- courses ➡️ sections ➡️ lessons (Curriculum hierarchy)

-- enrollments (Tracking student progress and access control)

-- plans & categories

🚀 Technical Challenges Overcome
-- Session Hijacking during Registration: Creating a student account while a teacher is logged in would overwrite the current active session. Solution: Engineered a secondary, isolated Supabase client exclusively for admin-level user creation .

-- Curriculum Ordering: Arrays couldn't reliably maintain drag-and-drop course structures. Solution: Implemented explicit section_order and lesson_order integer columns synced via batch-update mutations .

-- Performance Optimization: Migrated from useEffect data fetching to TanStack Query, enabling 5-minute stale caching, deduplication, and automatic refetches on window focus , drastically reducing database reads.

👨‍💻 Leadership & Team
Developed as a Graduation Project for the Digital Egypt Pioneers Initiative (DEPI).
Led by Abdullah Sawan (Team Leader), managing a team of 6 engineers across the full software development lifecycle, architecting the frontend infrastructure, and establishing the unified codebase standards.

Built with passion, React, and a lot of coffee. 

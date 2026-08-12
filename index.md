# 🎓 Hostel Pass System — Project Roadmap

> Java Full-Stack project (Spring Boot + React + MySQL) built as a hands-on learning project, based on the Java Guides "ReactJS + Spring Boot CRUD Full Stack Application" course.

---

## 1. Intro to the Project

**Hostel Pass System** is a full-stack web application that digitizes the hostel outpass process in a college — replacing the manual paper-based gate pass system.

**Core idea:**
- A **Student** applies for an outpass (reason, out date-time, expected return date-time).
- A **Warden/Admin** reviews pending requests and approves or rejects them.
- The system tracks pass **status** (`PENDING` → `APPROVED` / `REJECTED`) and maintains a full **history** of passes per student.

**Tech Stack:**
| Layer | Technology |
|---|---|
| Backend | Java, Spring Boot, Spring Data JPA (Hibernate) |
| Database | MySQL |
| Frontend | React (Hooks), Axios, Bootstrap |
| Auth (later phase) | Spring Security + JWT |
| Build Tools | Maven, npm |

**Goal:** Build this as a CRUD-based learning project first (Employee Management → adapted into Student/Pass entities), then layer on role-based approval logic and authentication.

---

## 2. Pre-Project Resources (Prerequisites)

Complete these **before** starting the main playlist. Pick Tamil or English per topic — whichever is easier to follow; mixing both is fine.

### 2.1 Spring Boot + REST API Basics
| Language | Resource |
|---|---|
| 🇬🇧 English | [Java Spring Framework and Spring Boot Course – Build Your First REST API](https://www.youtube.com/watch?v=xHql5yeGlnU) |
| 🇮🇳 Tamil | [Spring Boot Tutorial For Beginners In Tamil (4 HOURS)](https://www.youtube.com/watch?v=a90mHYYwnzU) |

### 2.2 JPA / Hibernate Basics
| Language | Resource |
|---|---|
| 🇬🇧 English | [Spring Data JPA Tutorial – Crash Course (2025)](https://www.youtube.com/watch?v=dKK2dZVLFug) |
| 🇮🇳 Tamil | Covered inside the Spring Boot Tamil course above (database/JPA section) |

### 2.3 React Basics
| Language | Resource |
|---|---|
| 🇬🇧 English | [React Crash Course 2024 — Traversy Media](https://www.youtube.com/watch?v=LDB4uaJ87e0) |
| 🇮🇳 Tamil | [React JS Tutorial for Beginners in Tamil 2024 — Full Course](https://www.youtube.com/watch?v=Uv7cKlZFXU8) |

### 2.4 Authentication (JWT + Spring Security) — do this LAST, closer to the login/approval module
| Language | Resource |
|---|---|
| 🇬🇧 English | [Spring Boot React JWT Authentication Example — Java Guides](https://www.javaguides.net/2024/05/spring-boot-react-jwt-authentication.html) |
| 🇮🇳 Tamil | No dedicated video — by this stage the English write-up + code will be easy to follow |

**Prerequisite Timeline:** ~5-6 days (Spring Boot + REST: 2-3 days, JPA: 1 day, React: 2 days). Authentication is deferred to Phase 3 below.

---

## 3. Main Course — Playlist to Follow

**ReactJS + Spring Boot CRUD Full Stack Application** — Java Guides (Ramesh Fadatare)
🔗 https://www.youtube.com/playlist?list=PLGRDMO4rOGcNLnW1L2vgsExTBg-VPoZHr

- 70 videos total, teaches CRUD basics by building an **Employee Management System**
- Reference source code: https://github.com/RameshMF/ReactJS-Spring-Boot-CRUD-Full-Stack-App
- Type every line along with the video — don't copy-paste.

---

## 4. Suggested Video Breakdown (by phase)

> Exact video numbers can shift if the playlist is reordered — use these phase names to locate your position, cross-check against the live playlist order.

| Phase | What it covers | Approx. video range |
|---|---|---|
| **Phase A — Backend Setup** | Spring Initializr, project structure, `application.properties`, MySQL connection | Videos 1–8 |
| **Phase B — Entity + Repository Layer** | `@Entity`, `@Id`, `JpaRepository`, Employee entity CRUD methods | Videos 9–20 |
| **Phase C — Service + Controller Layer** | `@Service`, `@RestController`, REST endpoints (GET/POST/PUT/DELETE), Postman testing | Videos 21–35 |
| **Phase D — React App Setup** | `create-react-app`, folder structure, Axios setup, Bootstrap install | Videos 36–45 |
| **Phase E — React Components (CRUD UI)** | List, Add, Edit, Delete components, routing, forms | Videos 46–60 |
| **Phase F — Integration + Polish** | Connecting frontend to backend fully, validations, final touches | Videos 61–70 |

**Watch order:** A → B → C (test APIs in Postman before touching React) → D → E → F.

---

## 5. Changes to Make From the Base Playlist (Customization Plan)

The playlist builds an **Employee Management System** — here's how to adapt it into the **Hostel Pass System**.

### 5.1 Entity changes
| Playlist has | Replace with |
|---|---|
| `Employee` (id, firstName, lastName, emailId) | `Student` (id, name, regNo, dept, hostelBlock, roomNo) |
| — (new) | `Pass` (id, studentId — FK, reason, outDateTime, expectedInDateTime, status) |
| — (new) | `Warden` (id, username, password — for approval login) |

### 5.2 Repository / Service / Controller changes
- Rename `EmployeeRepository` → `StudentRepository`, add new `PassRepository`
- Add a **relationship**: `Pass` has a `@ManyToOne` to `Student` (playlist's CRUD is single-entity, so this JOIN logic is your own addition — refer to the JPA prerequisite resource for `@ManyToOne`/`@JoinColumn`)
- New service methods beyond basic CRUD:
  - `applyForPass(studentId, passDetails)` → sets status `PENDING`
  - `approvePass(passId)` / `rejectPass(passId)` → status update only (Warden-only action)
  - `getPassHistory(studentId)` → list all passes for one student

### 5.3 New REST endpoints (not in playlist)
```
POST   /api/students/{id}/passes        → apply for a pass
GET    /api/passes/pending              → warden view: all pending passes
PUT    /api/passes/{id}/approve         → approve a pass
PUT    /api/passes/{id}/reject          → reject a pass
GET    /api/students/{id}/passes        → student's pass history
```

### 5.4 React changes
| Playlist has | Replace/add |
|---|---|
| `EmployeeComponent` (list/add/edit/delete) | `StudentPassForm` (apply for pass) |
| Single list view | Two views: **Student Dashboard** (my passes) and **Warden Dashboard** (pending approvals) |
| — (new) | Status badges (Pending/Approved/Rejected) with conditional styling |
| — (new) | Simple login page — role-based redirect (Student vs Warden) — built after Authentication resource (Section 2.4) |

### 5.5 Order of building the customization
1. Get base Employee CRUD fully working (don't skip this — it teaches the Controller → Service → Repository → Entity flow)
2. Rename/rebuild entity as `Student` — re-test all CRUD via Postman
3. Add `Pass` entity + relationship — test independently
4. Add Warden approval endpoints (this is not in the video — apply what you learned)
5. Rebuild React components for Student form + Warden approval table
6. Layer in JWT authentication + role-based routes last

---

## 6. Overall Timeline

| Week | Focus |
|---|---|
| Week 1 | Prerequisites (Section 2) |
| Week 2 | Main playlist — Phases A, B, C (backend) |
| Week 3 | Main playlist — Phases D, E, F (frontend) |
| Week 4 | Customization — Student/Pass entities, Warden approval logic |
| Week 5 | Authentication + final polish + deployment/README for GitHub |

---

*Maintained as a personal learning log while building the Hostel Pass System project.*

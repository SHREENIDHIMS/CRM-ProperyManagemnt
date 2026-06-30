# Property Management CRM
## Business Requirements Document (Planning Phase)

---

## Project Status

| Item | Status |
|------|--------|
| Product Type | Internal Single-Company CRM |
| Phase | Planning — not yet in development |
| Multi-Tenant / SaaS | No |
| Tech Stack | Not yet decided |
| Last Updated | Planning Phase |

> All diagrams and documents in this repository are technology-agnostic. No framework, database, or hosting decisions have been made. This document will be updated as planning progresses.

---

## Product Vision

A Property Management CRM that serves two groups with conflicting priorities simultaneously.

**Field Operations** — Property Managers, Leasing Agents, Maintenance Techs — need speed, mobility, and automation while working on-site.

**Management & Finance** — Super Admin, Controller, Compliance Officer, Auditor — need strict access control, role-based data isolation, and an immutable audit trail.

Every feature in this system must serve at least one of these two groups. Features that serve neither are out of scope.

---

## Organization Structure

Everything in this system depends on the organizational hierarchy. This is Module 0.

```
Company
│
├── Branches / Regions
│     ├── Departments
│     ├── Teams
│     └── Employees
│
├── Properties (assigned to branches)
│
└── External Parties (Vendors, Contractors, Owners, Tenants)
```

---

## Roles (16 total)

Roles are flat — not a strict inheritance ladder. Peers (Property Manager, Leasing Agent, Maintenance Tech) have different capability sets, not subsets of one another.

| # | Role | Group | Function |
|---|------|-------|----------|
| 1 | Super Admin | Internal | Company-wide configuration, audit access, sub-admin provisioning, global policy and threshold control |
| 2 | Sub-Admin | Internal | Branch/region management, local user provisioning, expense approval over threshold |
| 3 | Property Manager | Internal / Field | Manages assigned portfolios, tenant communication, lease and maintenance approval |
| 4 | Leasing Agent | Internal / Field | Lead pipeline, showings, application processing, listing syndication |
| 5 | Maintenance Supervisor | Internal / Field | Triages requests, assigns techs and contractors, preventive maintenance scheduling |
| 6 | Maintenance Tech | Internal / Field | Executes assigned work orders, photo uploads, status updates only |
| 7 | Compliance Officer | Internal | Fair Housing monitoring, screening audits, lease legal review, trust accounting rules |
| 8 | Controller / Finance Manager | Internal | Owner distribution approval, GL sign-off, late-fee rules, large vendor payouts |
| 9 | Bookkeeper | Internal | Rent transactions, bank reconciliation, trust ledger, vendor invoices, owner statements |
| 10 | HR Manager | Internal | Employee records, attendance, leave management, payroll, recruitment |
| 11 | Property Owner | External | Portfolio performance, capex approval, distribution statements, read-only access |
| 12 | Investor | External | Investment performance, distribution reports, no operational access |
| 13 | Auditor | External | Read-only financials and compliance logs, tax form export, no edit rights |
| 14 | Vendor Company | External | Job visibility, COI and invoice submission, 1099 receipt, contractor assignment |
| 15 | Contractor | External | Single-job visibility, completion photos, status updates only |
| 16 | Tenant | External | Maintenance requests, rent payment, lease documents, move-in/out acknowledgement |

> **Note:** Property Owner and Investor are split. They have different dashboards, different reports, and different permission scopes. Owner has operational visibility (capex, distributions). Investor has financial/performance visibility only.

---

## Module List (14 Modules)

### Module 01 — Organization
> The foundation layer. Every other module depends on this.

- Company profile and branch/region structure
- Department and team management
- Employee records and role assignments
- Branch-level settings and approval thresholds

---

### Module 02 — Dashboards
> Every role gets its own dashboard. One shared dashboard is not acceptable.

| Role | Dashboard Focus |
|------|----------------|
| Super Admin | Company-wide KPIs, compliance flags, audit activity |
| Sub-Admin | Branch occupancy, pending approvals, team activity |
| Property Manager | Portfolio status, open work orders, upcoming lease renewals |
| Leasing Agent | Active leads, scheduled showings, application pipeline |
| Maintenance Supervisor | Open work orders, SLA risk, tech availability |
| Maintenance Tech | Assigned jobs for the day, priority queue |
| Controller | Trust ledger health, pending distributions, expense approvals |
| Bookkeeper | Rent collection status, reconciliation queue, overdue payments |
| Property Owner | NOI, occupancy rate, distribution history |
| Investor | Portfolio ROI, distribution timeline |
| Tenant | Rent due, active requests, lease documents |

---

### Module 03 — CRM
> Separated from Leasing. CRM manages the relationship before a person becomes a tenant.

- Lead capture and pipeline management
- Prospect tracking and follow-up scheduling
- Site visit and showing coordination
- Booking and application handoff to Leasing
- Communication history per prospect

---

### Module 04 — Property Management
> The physical asset layer.

- Hierarchical mapping: Complex → Building → Unit → Bed
- Utility and meter tracking (serial numbers, shutoff locations)
- Key and fob management (digital log, current holder)
- Occupancy and vacancy tracking
- Document vault per property (insurance, deeds, warranties)
- Preventive maintenance scheduling (appliance lifecycle, HVAC intervals)

---

### Module 05 — Tenant Management
> Managing the relationship once a person becomes a tenant.

- Tenant profiles and communication history
- Renters insurance verification and tracking
- Guarantor and co-signer handling
- Eviction workflow (notice → filing → court date → judgment)
- Move-in and move-out acknowledgement
- Tenant portal access management

---

### Module 06 — Leasing
> Picks up from CRM after a booking/application is submitted.

- Application processing and status tracking
- Automated screening (credit, background, eviction checks)
- E-sign lease generation
- Rent escalation and renewal automation
- Lease expiry notice management
- Move-in inspection with geo-tagged photo proof

---

### Module 07 — Maintenance Operations
> End-to-end work order management from submission to closure.

- Tenant-submitted maintenance requests with photos
- Smart dispatch (proximity, trade, technician availability)
- SLA tracking and escalation tiers (emergency vs. routine)
- Preventive maintenance scheduling
- Vendor marketplace (RFP, COI tracking, 1099 issuance)
- Job completion approval and photo documentation
- Offline mobile mode for field techs

---

### Module 08 — Finance & Accounting
> The most compliance-sensitive module. Needs separate legal/compliance review before build.

- Automated rent collection (ACH, credit card, cash)
- Split-ledger trust accounting for security deposits
- Late-fee automation based on lease rules
- NSF and failed-payment handling and retry
- Owner distributions and statements
- Vendor invoice processing and expense approval
- General ledger and bank reconciliation
- Year-end 1099 generation for vendors and owners

---

### Module 09 — Documents
> Central document management across all modules.

- Property documents (insurance, deeds, inspection certs)
- Lease documents (agreements, addenda, renewals)
- Tenant documents (IDs, screening reports, move-in/out forms)
- Employee documents (contracts, certifications)
- Legal documents (eviction filings, compliance records)
- Vendor documents (COIs, invoices, contracts)
- Inspection reports and photo archives

---

### Module 10 — Calendar & Tasks
> Every role schedules work. This is the shared scheduling layer.

- Individual and team calendars per role
- Task assignment and tracking
- Reminders and deadline alerts
- Showing and site visit scheduling
- Inspection scheduling
- Maintenance job scheduling
- Recurring task templates

---

### Module 11 — Notifications
> Cross-cutting. Almost every module triggers notifications.

| Trigger | Recipient |
|---------|-----------|
| Rent due | Tenant |
| Rent overdue | Bookkeeper, Property Manager |
| Lease expiring | Tenant, Leasing Agent, Property Manager |
| Maintenance request submitted | Maintenance Supervisor |
| Work order assigned | Maintenance Tech / Contractor |
| Job completed | Maintenance Supervisor, Property Manager |
| Expense approval pending | Sub-Admin |
| Distribution approved | Owner |
| Compliance flag raised | Compliance Officer, Sub-Admin |
| Inspection scheduled | Tenant, Property Manager |

- Delivery channels: in-app, email, SMS, push (channel not yet decided)
- Notification preferences per role
- Notification log and read receipts

---

### Module 12 — Reports
> Every role generates or consumes reports. Reports are read-only views, not editable.

- Property reports (occupancy, vacancy, turnover)
- Financial reports (NOI, rent roll, delinquency aging)
- Maintenance reports (open orders, SLA performance, vendor scores)
- Lease reports (active leases, expiries, renewals)
- Employee reports (attendance, task completion)
- Compliance reports (screening audit, fair housing log)
- Audit reports (action history, access log)
- Export rights per role (PDF, CSV)

---

### Module 13 — HR & Administration
> Internal staff management.

- Employee profiles and role assignments
- Attendance and leave management
- Payroll records (not payroll processing — records only unless decided later)
- Recruitment pipeline
- Onboarding and offboarding checklists
- Team and department management

---

### Module 14 — Audit & Compliance
> Every privileged action in the system is logged here. Immutable.

- Company-wide audit log (append-only, no edit or delete)
- Before/after state capture on every record change
- Fair Housing compliance monitoring
- Trust accounting rule tracking (state-by-state)
- Screening criteria audit trail
- Compliance flag workflow (raise → review → resolve)
- Auditor export tools (read-only, no system access)

---

## Key Design Decisions

**Vendor ≠ Contractor**
Vendor Company holds the legal/insurance/financial relationship (COI, invoice, 1099). Contractor is the individual dispatched under that company. Separate entities, separate portal access.

**Compliance Officer ≠ Auditor**
Compliance Officer is proactive — monitors Fair Housing, audits screening criteria, reviews lease language. Auditor is reactive and read-only — reviews records after the fact, exports tax documents, no write access.

**Controller ≠ Bookkeeper**
Controller has approval authority (distributions, GL sign-off, large payouts). Bookkeeper does transactional work (recording, reconciliation, statement prep). Different permission scope, different dashboards.

**Property Owner ≠ Investor**
Owner has operational visibility — capex approval, distribution statements, PM communication. Investor has financial/performance visibility only — ROI, portfolio value, distribution history. Separate dashboards, separate reports, separate permission sets.

**CRM is separate from Leasing**
CRM manages prospects before they become applicants. Leasing picks up when an application is submitted. The handoff point is the booking/application submission.

**Dashboards are role-specific**
Every role has its own dashboard built around their daily tasks. There is no global one-size dashboard.

**Audit logs are immutable**
Every privileged action (create, update, delete, approve) generates an append-only log entry with before/after state. No role — including Super Admin — can edit or delete audit entries.

---

## Planning Artifacts Status

| Artifact | Status |
|----------|--------|
| Product Vision | ✅ Complete |
| Role Definitions (16 roles) | ✅ Complete |
| UML Class Diagram (roles + permissions) | ✅ Complete |
| ER / Entity Overview Diagrams (master + 5 modules) | ✅ Complete |
| Individual Role Diagrams (14 files) | ✅ Complete |
| Application Flow Diagram | ✅ Complete |
| Super Admin UI/UX Layout | ✅ Complete |
| Module List (14 modules) | ✅ Complete |
| Business Rules Document | 🔲 Not started |
| User Journey Maps (per role) | 🔲 Not started |
| Screen Inventory (all application pages) | 🔲 Not started |
| Role Permission Matrix (role × module × action) | 🔲 Not started |
| Dashboard Specifications (per role) | 🔲 Not started |
| Report Catalogue | 🔲 Not started |
| Notification Catalogue | 🔲 Not started |
| Approval Matrix | 🔲 Not started |
| Glossary | 🔲 Not started |

---

## Open Items

- Technology stack — deferred until planning is finalized
- Trust accounting compliance rules per state — needs legal/compliance review before Module 08 is built
- Payroll processing scope — HR module currently covers records only; payroll processing is undecided
- Notification delivery channels — in-app/email/SMS/push decision pending
- Sequence diagrams for key transactions — not yet generated

---

## Glossary (Starter)

| Term | Definition |
|------|------------|
| Unit | A single rentable space within a building |
| Bed | A sub-unit within a shared or multi-bedroom unit |
| Complex | A group of buildings managed as a single portfolio |
| Lease | A legally binding agreement between the company and one or more tenants covering a specific unit |
| Work Order | A logged request for maintenance or repair, submitted by a tenant or staff member |
| Trust Ledger | A split-ledger accounting record holding tenant security deposits, kept strictly separate from operating funds |
| Distribution | A payment made to a Property Owner from rental income after expenses |
| COI | Certificate of Insurance — required from every Vendor Company before work is assigned |
| 1099 | A year-end tax form issued to vendors and owners for payments made during the tax year |
| SLA | Service Level Agreement — the maximum time allowed to respond to or resolve a maintenance request by priority level |
| NOI | Net Operating Income — total rental income minus operating expenses, before debt service |

---

*This document represents the planning state of the Property Management CRM project. It will be updated at each planning milestone before development begins.*

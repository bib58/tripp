## Product Requirements Document (PRD) — Phase 0 MVP
**Architecture:** Monolith (single deployable app)

---

## 1. Objectives (Phase 0 / MVP)

1. Backend and database setup.
2. Server-side auth: password hashing, sessions, and server-enforced RBAC.
3. One connected data flow: Truck In → Procurement → Inventory → Production → Sales → Payments → Accounting; so a single transaction updates every downstream number automatically
4. Weighbridge (Truck In / Truck Out) with auto net-weight and moisture deduction.
5. Party accounting(ledger) per farmer/dealer with CSV export.
6. Day-End Closing screen aggregating the whole day in one view.

### Things for further Phases
- GST E-Way Bill / E-Invoice IRN generation (needs govt API onboarding — separate phase).
- Barcode scanning and GPS/live vehicle tracking (needs hardware/device testing).
- Multi-branch data isolation
- Official WhatsApp Business API and sending whatsapp of the bills generated.

---

## 2. Users / Roles

| Role | Access |
|---|---|
| `owner_admin` | Everything |
| `accountant` | Procurement, Sales, Finance, Khata, Reports |
| `godown_staff` | Gate Pass, Procurement, Inventory |
| `production_staff` | Production, Inventory |
| `sales_staff` | Sales, Inventory (read) |
| `driver` | Fleet, own gate passes |

---

## Functional Requirements by Module
### 1. Weighbridge (Truck In / Truck Out)
- **Truck In:** operator enters/scans vehicle number, selects farmer/supplier and paddy variety, records gross weight → status becomes Inside, initial gate-pass token printed.
- **Truck Out:** operator selects truck from active list, records tare weight, system computes `Net Weight = Gross − Tare`.
- If moisture > 14%, system auto-applies the moisture deduction formula to net paddy qty.
- One click generates the final weighment slip and closes the gate pass.
- Gate-pass number is a DB-generated sequence (not random) with a unique constraint.

### 2. Procurement → Inventory (auto-linked)
- Completing a Truck Out for an inbound truck creates a Procurement record.
- That Procurement record **automatically** increments Raw Paddy stock in Inventory via an inventory transaction row (never edited directly).

### 3. Production / Milling → Inventory (auto-linked)
- Logging a Milling Batch automatically:
  - Deducts Raw Paddy stock by the qty consumed.
  - Adds Head Rice, Broken Rice, Bran and Husk to stock in the same transaction.

### 4. Sales & Invoicing
- Creating a Sales Invoice automatically deducts sold rice bags from Inventory and creates a Party entry (dealer's outstanding increases).
- Invoice numbers are DB-generated with a unique constraint (never random).

### 5. Payments & Party accounts
- Recording a payment updates cash/bank balance and reduces the party's outstanding amount.
- Account screen: search any farmer/dealer → goods delivered/purchased, payments made/received, running outstanding balance.
- Export to CSV/Excel.

### 6. WhatsApp & Printing
- "Share on WhatsApp" button on weighment slips and sales bills opens with a pre-filled message: mill name, slip/invoice number, vehicle number, party name, net paddy, rate, total.

### 7. Day-End Closing
- Single screen shows: total paddy received, total trucks, total paddy qty, total rice dispatched (bags + MT), total cash/bank collected, total operational expenses, net cash available — computed live from the day's transactions, not re-entered.

---

## Suggested Tech Stack (Monolith)

| Layer | Choice | Purpose |
|---|---|---|
| **Framework** | Next.js (App Router) | Frontend + backend |
| **Database** | PostgreSQL(Neon for Phase 0 and move to AWS RDS later) | ACID transactions, FK integrity and unique constraints |
| **ORM** | Prisma |
| **Auth** | Role-based access |
| **File storage** | Local disk for phase-0 | Move to AWS S3 later for file storage |
| **WhatsApp share** | `wa.me` deep links | No business API account needed for Phase 0 |
| **Containerization** | Docker | Single reproducible deployable artifact |
| **Hosting** | vercel for phase 0 use CI-CD and AWS EC2 later |

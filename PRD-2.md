# Rice Mill Management System
### Product Requirements Document — MVP (Phase 0)

---

## Table of Contents

1. [The Problem We're Solving](#1-the-problem-were-solving)
2. [Who Is This For?](#2-who-is-this-for)
3. [The Big Picture — How the Mill Works Today and Before vs After](#3-the-big-picture)
4. [User Roles & What They Can Do](#4-user-roles--what-they-can-do)
5. [End-to-End User Journey](#5-end-to-end-user-journey)
6. [MVP Features — What's In & What's Out](#6-mvp-features--whats-in--whats-out)
7. [Key Screens at a Glance](#7-key-screens-at-a-glance)

---

## 1. The Problem We're Solving

A typical rice mill runs dozens of operations every day — trucks arriving, paddy being weighed and milled, rice being sold, payments being collected, farmer accounts being updated. **Today, all of this is tracked manually** — on paper registers, WhatsApp messages, and scattered Excel sheets.


| Pain Point | Impact |
|---|---|
| Manual weight entries | Calculation mistakes, disputes with farmers |
| No auto-inventory | Owner doesn't know real stock without counting bags |
| Disconnected cash book | Payments recorded separately from sales |
| Paper ledgers for farmers/dealers | Hard to check outstanding balances quickly |
| No daily summary | Owner has to call 4 different people to know the day's total |

**This app solves all of the above in one place.** One transaction at the gate automatically updates the stock, the ledger, and the daily summary — with no double-entry needed.

---

## 2. Who Is This For?

The primary user is a **rice mill in India** that:
- Processes paddy procured from local farmers
- Sells milled rice to dealers/wholesalers
- Currently uses manual registers or basic Excel

---

## 3. The Big Picture

### How the Mill Operates — The Core Flow

```
FARMER brings PADDY
        ↓
TRUCK arrives at GATE
        ↓
WEIGHBRIDGE records GROSS WEIGHT
        ↓
Paddy is UNLOADED
        ↓
TRUCK leaves → TARE WEIGHT recorded → NET PADDY calculated
        ↓
RAW PADDY added to INVENTORY (automatically)
        ↓
MILLING BATCH run → Raw Paddy consumed → Rice + Bran + Husk produced (automatically)
        ↓
SALES INVOICE created → Rice deducted from stock (automatically)
        ↓
PAYMENT received → Dealer's outstanding reduced (automatically)
        ↓
DAY-END SUMMARY → Everything in one view
```

> one entry. The app handles all the downstream updates automatically.

#### Before vs. After

```
BEFORE (Manual)                        AFTER (This App)
──────────────────────────────────     ──────────────────────────────────────
Weigh truck → write in register        Weigh truck → app calculates net paddy
Update stock register manually         Stock updates itself automatically
Update farmer's account manually       Farmer ledger updates itself
Update cash book manually              Cash book updates itself
Call accountant for daily total        Open Day-End screen → everything there
WhatsApp slip typed by hand            Click "Share" → pre-filled message ready
```

---

## 4. User Roles & What They Can Do

Each person who uses the app gets a login with a role. The role decides what screens and buttons they can see.

| Role | Authorized work |
|---|---|
| **Owner / Admin** | Sees and controls everything. The final authority. |
| **Accountant** | Handles money — procurement bills, sales invoices, party payments, and reports. |
| **Godown Staff** | Manages the gate — logs trucks in and out, records weights. |
| **Production Staff** | Logs milling batches. Sees what raw paddy is available, records what came out. |
| **Sales Staff** | Creates sales invoices. Can view inventory but cannot change it. |
| **Driver** | Sees their own gate passes. Can update fleet/vehicle info. |

---

## 5. End-to-End User Journey

This section walks through the full life of one paddy delivery — from the moment a truck arrives to when the owner reviews the day.

---

### Journey 1 — Truck Arrives (Godown Staff)

```
Staff opens app on tablet/PC at the gate
          │
          ▼
Clicks "New Truck In"
          │
          ▼
Enters or scans vehicle number
Selects farmer name from list
Selects paddy variety (e.g., Sona Masuri, IR-36)
          │
          ▼
Records GROSS WEIGHT (full truck)
          │
          ▼
App prints / shows GATE ENTRY TOKEN
Truck status → "Inside / Unloading"
          │
          ▼
Paddy is unloaded from truck
          │
          ▼
Staff clicks "Truck Out" from active trucks list
Records TARE WEIGHT (empty truck)
          │
          ▼
App auto-calculates:
   Net Paddy = Gross Weight − Tare Weight
   If moisture > 14% → deduction applied automatically
          │
          ▼
One click → WEIGHMENT SLIP generated
Gate pass closed ✓
          │
          ▼
"Share on WhatsApp" button → pre-filled message sent to farmer
```

> **What happens automatically behind the scenes:**
> Raw Paddy stock in inventory increases by the net paddy quantity. Farmer's procurement record is created. No manual entry needed.

---

### Journey 2 — Milling a Batch (Production Staff)

```
Production staff opens "New Milling Batch"
          │
          ▼
Selects paddy variety and quantity to process
(Can see current raw paddy stock — can't mill more than available)
          │
          ▼
Enters milling output ratios
(e.g., Head Rice 65%, Broken 8%, Bran 20%, Husk 7%)
          │
          ▼
Clicks "Start Batch"
          │
          ▼
App automatically:
   Deducts raw paddy from stock
   ➕ Adds Head Rice to stock
   ➕ Adds Broken Rice to stock
   ➕ Adds Rice Bran to stock
   ➕ Adds Husk to stock
          │
          ▼
Batch recorded with timestamp ✓
```

---

### Journey 3 — Selling Rice (Sales Staff / Accountant)

```
Sales staff clicks "New Sales Invoice"
          │
          ▼
Selects dealer from list
Selects rice type (Head Rice / Broken / Bran / Husk)
Enters quantity and rate
          │
          ▼
App shows:
   Total amount
   GST (if applicable)
   Dealer's current outstanding balance
          │
          ▼
Staff clicks "Confirm Invoice"
          │
          ▼
App automatically:
   Deducts rice bags from inventory
   Creates dealer's outstanding entry in their ledger
          │
          ▼
Invoice PDF generated
"Share on WhatsApp" or Print ✓
```

---

### Journey 4 — Recording a Payment (Accountant)

```
Accountant opens "Payments" screen
          │
          ▼
Searches farmer (for paddy purchase payment)
          OR
Searches dealer (for sales payment received)
          │
          ▼
Enters amount and payment mode (Cash / Bank Transfer / Cheque)
Optionally attaches payment proof photo
          │
          ▼
Clicks "Record Payment"
          │
          ▼
App automatically:
   Updates cash or bank balance
   Reduces party's outstanding amount
   Adds entry to party's ledger
          │
          ▼
Payment recorded ✓
```

---

### Journey 5 — Checking a Party's Account (Accountant / Owner)

```
Opens "Party Accounts"
          │
          ▼
Searches by name: farmer or dealer
          │
          ▼
Sees full running ledger:
   ─ Date  | Transaction           | Debit    | Credit   | Balance ─
   08/28   | Paddy Received 120 bags | ₹60,000 |          | ₹60,000 ↑
   08/29   | Payment Received        |          | ₹30,000  | ₹30,000 ↓
   09/01   | Paddy Received 80 bags  | ₹40,000 |          | ₹70,000 ↑
          │
          ▼
Clicks "Export to Excel / CSV" → file downloaded ✓
```

---

### Journey 6 — Day-End Review (Owner)

```
Owner opens "Day-End Summary" at end of day (e.g., 7 PM)
          │
          ▼
Single screen shows everything for today:

┌────────────────────────────────────────────────────┐
│               DAY-END SUMMARY — 4 Sep 2026         │
├────────────────────────┬───────────────────────────┤
│ Total Trucks Arrived   │  12 trucks                │
│ Total Paddy Received   │  480 Quintals             │
│ Total Rice Dispatched  │  320 Bags / 16 MT         │
│ Cash Collected         │  ₹1,20,000                │
│ Bank Transfers         │  ₹3,80,000                │
│ Operational Expenses   │  ₹22,000                  │
│ Net Cash in Mill       │  ₹98,000                  │
└────────────────────────┴───────────────────────────┘

All computed live — owner doesn't enter anything ✓
```

---

## 6. MVP Features — What's In & What's Out

### ✅ IN SCOPE (Phase 0 / MVP)

| # | Feature | Why It's In |
|---|---|---|
| 1 | **Login with roles** | Security baseline — each person sees only what they should |
| 2 | **Weighbridge (Truck In / Out)** | Core daily operation. Every procurement starts here |
| 3 | **Auto Inventory** | Single source of truth for stock — no manual tally |
| 4 | **Milling Batch Log** | Tracks what paddy was consumed and what rice was produced |
| 5 | **Sales & Invoicing** | Core revenue function |
| 6 | **Payments & Party Ledger** | Know who owes what — farmers AND dealers |
| 7 | **Day-End Summary** | Owner's daily single-screen view |
| 8 | **WhatsApp Share** | Immediate slip/bill sharing without extra tools |
| 9 | **CSV / Excel Export** | Accountant's backup and GST prep |

---

### ❌ OUT OF SCOPE (Future Phases)

| Feature | Reason Deferred |
|---|---|
| GST E-Way Bill / E-Invoice (IRN) | Needs Government API onboarding — separate process |
| Barcode scanning at gate | Hardware testing required |
| GPS / Live vehicle tracking | Needs device integration and testing |
| Multi-branch data isolation | Requires more complex architecture |
| Official WhatsApp Business API | Needs WhatsApp Business account approval |

---

## 7. Key Screens at a Glance

```
┌──────────────────────────────────────────────────────────────────┐
│  APP NAVIGATION (Sidebar)                                        │
├──────────────────────────────────────────────────────────────────┤
│  🏠 Dashboard          → Live overview of today's activity       │
│  🚛 Weighbridge        → Truck In / Truck Out / Active Trucks    │
│  📦 Inventory          → Current stock of all items              │
│  🏭 Production         → Log milling batches                     │
│  🛒 Sales              → Create invoices, view invoice history   │
│  💰 Payments           → Record and track payments               │
│  📒 Party Accounts     → Farmer & dealer ledgers                 │
│  📊 Day-End Summary    → One-screen daily close                  │
│  ⚙️  Settings           → User management, mill profile          │
└──────────────────────────────────────────────────────────────────┘
```

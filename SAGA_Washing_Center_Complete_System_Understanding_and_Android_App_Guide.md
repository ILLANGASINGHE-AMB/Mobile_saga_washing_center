# SAGA Washing Center — Complete System Understanding & Android Mobile App Guide

## 1. What this system actually is

This is essentially a **Laundry/Washing Center Business Management + POS/ERP system**.

Its purpose is to manage the complete operational cycle:

```text
CUSTOMERS
    ↓
ORDERS
    ↓
LAUNDRY ITEMS / SERVICES
    ↓
PICKUP / DELIVERY
    ↓
INVOICING
    ↓
PAYMENTS / DEDUCTIONS
    ↓
REVENUE
    ↓
EXPENSES
    ↓
CHEMICAL STOCK
    ↓
TRANSPORT / DRIVERS
    ↓
REPORTING
    ↓
DATA ANALYTICS
    ↓
AI BUSINESS ANALYSIS
```

It is considerably more than a simple POS.

---

# 2. Main System Modules

The application contains these major areas:

| Module | Purpose |
|---|---|
| Dashboard | Overall business overview |
| Orders | Create and manage laundry orders |
| Customers | Manage hotel/customer accounts |
| Drivers | Manage drivers |
| Transport | Manage trips and customer visits |
| Pay Now | Process order/invoice payments |
| Invoices | Invoice lifecycle and billing |
| Deductions | Track customer deductions |
| Items | Laundry item/service catalogue |
| Expenses | General expenses + chemical inventory |
| Data Analytics | Business intelligence dashboard |
| Reports | Operational/financial reports |
| Recent Actions | Audit/activity history |
| Settings | Users, company, billing, database, AI etc. |

There is also:

- Quotation generation
- Invoice printing
- QR-based delivery confirmation
- Customer-specific pricing
- Database backup/restore
- Import/export
- Gemini AI integration
- Dark/light mode
- Adjustable text size
- Global search
- Keyboard shortcuts

---

# 3. User Roles

There are three actual application roles.

## Admin

Admin has essentially unrestricted access.

```text
Dashboard
Orders
Customers
Drivers
Transport
Pay Now
Invoices
Deductions
Items
Expenses
Data Analytics
Reports
Recent Actions
Settings
```

Admin can generally:

- Add
- Edit
- Delete
- Export
- Import
- Configure
- Manage users
- Backup/restore
- Reset data
- Access reports
- Access analytics

---

## Staff User

Staff has operational access but restricted administrative capabilities.

```text
Dashboard
Orders
Customers
Drivers
Transport
Pay Now
Deductions
Items
Expenses
Settings
```

Restrictions include:

- No deleting orders
- No deleting customers
- No deleting drivers
- No invoice management
- No reports
- No recent-actions access
- Limited settings
- No quotation/catalogue functionality where restricted

---

# 4. Driver Role

This is especially important for the Android application.

The Driver role is deliberately much smaller:

```text
Transport
Customers
Orders
Settings
```

### Driver can:

**Transport**
- Create trips
- Edit trips
- Complete trips
- Select customers
- Record trip information

**Customers**
- Add customers
- Edit customers
- Cannot delete customers

**Orders**
- View orders
- Print orders/bills
- Cannot create orders
- Cannot edit orders
- Cannot delete orders

**Settings**
- Text size
- Light/dark mode

Everything else is hidden.

So the driver is fundamentally an **operational field worker**, not a financial/admin user.

---

# 5. Authentication & Role Control

The application starts with login.

The system determines the user's role and then dynamically restricts navigation and functions.

Conceptually:

```text
LOGIN
  ↓
Validate Username/Password
  ↓
Identify Role
  ↓
Load Allowed Pages
  ↓
Restrict Sidebar
  ↓
Restrict Buttons/Actions
  ↓
Open Appropriate Dashboard
```

Drivers are sent directly to:

```text
Transport
```

rather than the normal business dashboard.

This is a very good architectural decision for the future driver mobile app.

---

# 6. Dashboard

The main dashboard provides a high-level business overview.

It includes things such as:

### Business statistics

- Orders
- Revenue
- Customers
- Payments
- Outstanding amounts
- Order status

### Charts

The application contains:

- Daily order/status charts
- Order status distribution
- Recent orders
- Unpaid invoices
- Customer information

The dashboard is intended for **management monitoring**, rather than detailed analysis.

---

# 7. Customer Management

Customers are primarily hotel/business customers.

The customer record is used throughout the system.

A customer can be associated with:

```text
Customer
   ↓
Orders
   ↓
Order Items
   ↓
Invoices
   ↓
Payments
```

Customer functionality includes:

- Add customer
- Edit customer
- Delete customer — Admin only
- Search
- Pagination
- View customer orders
- Customer sales summary
- Customer-specific item prices

The last feature is particularly important.

---

# 8. Customer-Specific Pricing

The system supports **custom pricing per customer**.

For example:

```text
Normal Price

Bed Sheet (L) = Rs. 300
```

But:

```text
Hotel ABC
Bed Sheet (L) = Rs. 270
```

When Hotel ABC is selected while creating an order, the system attempts to use the customer's customized pricing.

Otherwise:

```text
Customer-specific price
        ↓
if unavailable
        ↓
Default item price
```

This is an important business rule.

---

# 9. Orders

Orders are one of the core modules.

An order represents a customer's laundry transaction.

The general structure is:

```text
Customer
   ↓
Order
   ├── Order ID / Batch ID
   ├── Pickup
   ├── Delivery
   ├── Items
   ├── Quantities
   ├── Prices
   ├── Subtotal
   ├── Discount
   ├── Delivery charge
   ├── Advance payment
   └── Status
```

---

# 10. Order Items

Each order can contain multiple laundry items.

For example:

```text
Order #001

Bed Sheet L       × 20
Bath Towel        × 30
Pillow Case       × 40
Chef Coat         × 10
```

Each item has pricing and quantity information.

The system calculates item subtotal based on quantity and price.

---

# 11. Order Types / Operational Flow

The system supports different order-related workflows including:

- Normal orders
- Pickup requests
- Credit bills
- Payment-related order states
- Delivery confirmation

The order lifecycle feeds directly into invoicing.

---

# 12. Order Status

The application mainly normalizes order payment state into:

```text
Paid
Unpaid
```

There are also operational distinctions such as credit-related orders.

The important point is that **order status is tightly connected to the payment/invoice system**.

---

# 13. Pickup & Delivery

The system has explicit pickup/delivery information.

There is functionality for:

- Pickup request
- Pickup date
- Delivery date
- Delivery information
- QR delivery confirmation

This means the application is not purely financial; it also manages physical laundry movement.

---

# 14. QR Delivery Confirmation

There is a QR-based workflow.

The database layer contains functionality for:

```text
getOrderByToken()
markOrderReceivedByQR()
```

So the system can associate a QR/token with an order and mark it as received/confirmed.

This is useful for:

- Delivery confirmation
- Customer receiving
- Proof of delivery
- Reducing manual confirmation

---

# 15. Invoice Management

The invoice module is relatively sophisticated.

An invoice can contain:

```text
Invoice
 ├── Invoice Number
 ├── Order
 ├── Customer
 ├── Items
 ├── Total
 ├── Advance Payment
 ├── Balance
 ├── Delivery Charge
 ├── Discount
 ├── Extra Payment
 ├── Deduction
 ├── Payment Status
 └── Payment History
```

There are also standard and credit invoice types.

---

# 16. Invoice Generation

When an order is ready, the system can generate an invoice.

Conceptually:

```text
ORDER
  ↓
Calculate Order Total
  ↓
Generate Invoice Number
  ↓
Create Invoice
  ↓
Calculate Advance
  ↓
Calculate Balance
  ↓
Determine Paid/Unpaid
  ↓
Print / View Invoice
```

Invoices can be viewed and printed.

---

# 17. Consolidated Invoices

An important feature is support for **batch/consolidated invoices**.

The invoice code supports concepts such as:

```text
batch_order_ids
batch_invoice_details
```

This means multiple orders can be represented within a consolidated invoice.

For a hotel customer, this is particularly useful.

Example:

```text
Hotel ABC

Order 001 → Rs. 25,000
Order 002 → Rs. 18,000
Order 003 → Rs. 32,000

                    ↓

Consolidated Invoice
                    ↓
                  Rs.75,000
```

---

# 18. Payments

Payments are stored separately from invoices.

The system can record:

- Payment amount
- Payment method
- Payment date
- Notes
- Multiple payments against an invoice

This allows partial payment.

Example:

```text
Invoice = Rs.100,000

Payment 1 = Rs.30,000
Payment 2 = Rs.40,000
Payment 3 = Rs.30,000

Balance = Rs.0
Status = Paid
```

---

# 19. Partial Payment

The system explicitly supports partial payments.

Therefore:

```text
Invoice Total
      ↓
Amount already paid
      ↓
Remaining balance
```

The invoice status is updated according to payment progress.

---

# 20. Deduction Payments

The system supports:

```text
Standard Payment
```

and:

```text
Pay with Deductions
```

A customer may deduct an amount from the invoice.

For example:

```text
Invoice       Rs.100,000
Deduction     Rs.  5,000
Actual paid   Rs. 95,000
```

The deduction is recorded separately.

Admin can later reverse/delete deduction records.

---

# 21. Items / Service Catalogue

The Items module is essentially the laundry service/product catalogue.

It contains items such as:

- Bed Sheet
- Duvet Cover
- Pillow Case
- Bath Towel
- Bath Robe
- Pool Towel
- Curtain
- Table Cloth
- Shirt
- Trouser
- Chef Coat
- Apron
- etc.

Each item can have information such as:

```text
Item ID
Item Code
Item Name
Price
Status
```

The quotation documentation contains a large predefined item list.

---

# 22. Item Management

Admin/staff can:

- Add items
- Edit items
- Delete items depending on role
- Search
- Export
- Import

Admin can also:

- Print catalogue
- Generate quotations

---

# 23. Quotation System

The application contains a proper quotation workflow.

```text
Items
 ↓
Select Customer
 ↓
Load Customer-specific prices
 ↓
Select items
 ↓
Set quantities/prices
 ↓
Add notes/terms
 ↓
Generate Quote ID
 ↓
Save quotation
 ↓
Preview
 ↓
Print / Export
```

Quotation IDs are automatically generated and are not supposed to be manually edited.

---

# 24. Quotation History

Generated quotations are saved.

The system supports:

- View quotation
- Print quotation
- Delete quotation

So quotations aren't just temporary PDFs.

They have a historical record.

---

# 25. Expenses

The Expenses module has two major areas:

```text
Expenses
├── General Operational Expenses
└── Chemicals
```

---

# 26. General Expenses

Examples include:

- Electricity
- Diesel
- Stationery
- Utilities
- Other operational expenses

Each expense contains concepts such as:

```text
Expense ID
Expense Name
Expense Date
Amount
Months Covered
Monthly Averaged Amount
Payment Method
Notes
```

The `months_covered` and `monthly_averaged_amount` fields are designed to distribute multi-month expenses.

---

# 27. Chemical Management

The chemical system is essentially an inventory ledger.

There is a **Chemical Master**:

```text
CHM-0001
Supermat
kg
Active
```

and a **Chemical Ledger**.

The ledger supports:

```text
IN
OUT
BAL
```

Conceptually:

```text
Opening Balance
       +
Stock IN
       -
Stock OUT
       =
Current Balance
```

---

# 28. Chemical Stock Logic

The intended formula is:

```text
Balance =
Previous Balance + IN - OUT
```

Example:

```text
Opening = 10 kg
IN      = 5 kg
OUT     = 3 kg

Balance = 10 + 5 - 3
        = 12 kg
```

This is an important operational feature because chemical consumption is a significant cost driver in a laundry business.

---

# 29. Transport & Trip Management

This is the module most relevant to the Android Driver application.

The trip structure is:

```text
Trip
├── Trip ID
├── Driver
├── Start Date
├── Start Time
├── Starting KM
├── Selected Customers
├── Visit Order
├── Notes
├── End Date
├── End Time
├── Final KM
├── Distance KM
└── Status
```

---

# 30. Driver Trip Workflow

The actual business workflow is:

```text
LOGIN
 ↓
TRANSPORT
 ↓
CREATE NEW TRIP
 ↓
Generate Trip ID
 ↓
Record Start Date
 ↓
Record Start Time
 ↓
Record Starting KM
 ↓
Select Customers
 ↓
Arrange Customer Visit Order
 ↓
Save
 ↓
Trip = IN PROGRESS
 ↓
Travel / Visit Customers
 ↓
END TRIP
 ↓
Record End Date
 ↓
Record End Time
 ↓
Record Final KM
 ↓
Calculate Distance
 ↓
Validate
 ↓
Save
 ↓
Trip = COMPLETED
```

Distance:

```text
Distance = Final KM - Starting KM
```

---

# 31. Customer Visit Sequence

The system doesn't merely associate customers with a trip.

It stores the visit sequence using concepts such as:

```text
customer_id
hotel_name
visit_order
```

Therefore:

```text
Trip TR-0001

1 → Hotel A
2 → Hotel C
3 → Hotel B
4 → Hotel D
```

The sequence itself is meaningful.

This is useful for:

- Driver routing
- Pickup planning
- Delivery planning
- Operational reporting

---

# 32. Data Analytics

The application already contains a substantial **Data Analytics** module.

It was designed to evolve into a business intelligence dashboard.

It supports analysis of:

### Revenue

```text
Gross Revenue
```

### Expenses

```text
General Expenses
+
Chemical Purchases
```

### Profit

```text
Net Operating Profit
=
Gross Revenue - Total Expenses
```

### Profit Margin

```text
Profit Margin
=
(Net Profit / Gross Revenue) × 100
```

### Cost Ratio

```text
Cost Ratio
=
(Total Expenses / Gross Revenue) × 100
```

### AOV

```text
Average Order Value
=
Gross Revenue / Number of Orders
```

---

# 33. Analytics Time Views

The analytics implementation supports:

```text
Daily
Weekly
Monthly
Yearly
```

This is done through time-bucket aggregation.

Therefore management can investigate:

```text
Day
 ↓
Week
 ↓
Month
 ↓
Year
```

---

# 34. Analytics Filters

The current analytics engine supports filters including:

- Date range
- Customer
- Payment status
- Item

This means management can ask questions such as:

> How much revenue did Hotel ABC generate this year?

or:

> How much did paid orders contribute last month?

or:

> How much revenue came from a particular laundry item?

---

# 35. Analytics Insights

The existing analytics engine attempts to identify:

### Peak trading day

Example:

```text
Friday
→ 32% of revenue
```

### Best customer

```text
Hotel ABC
→ highest revenue contributor
```

### Financial health

It checks:

```text
Expense Ratio
Profit Margin
```

and produces contextual recommendations.

---

# 36. Analytics Charts

Current analytics includes:

### Revenue vs Expenses

```text
Revenue
Expenses
```

over time.

### Revenue by Day of Week

```text
Monday
Tuesday
Wednesday
...
Sunday
```

### Top Items

Top items by revenue.

### Expense Categories

Doughnut/pie-style visualization of expense distribution.

---

# 37. Analytics Tables

There are detailed tables for:

### Customers

```text
Customer
Orders
Revenue
Average Order
```

### Items

```text
Item Code
Item
Quantity Sold
Revenue
```

This allows management to move from visual overview to detailed records.

---

# 38. Analytics Export

Analytics can export:

```text
CSV
Excel
Print
```

The Excel export can contain separate sheets such as:

```text
Financial Summary
Top Customers
Item Performance
```

---

# 39. Reports

The Reports module is separate from Data Analytics.

This distinction is good.

### Reports answer:

> What happened?

### Analytics answers:

> Why is it happening and what should we do?

The current reports include things such as:

- Daily orders
- Monthly revenue
- Customer billing
- Driver reports
- Full reports
- Customer summaries
- Monthly bills

Reports can generally be:

```text
CSV
Excel
Print
```

---

# 40. AI / Gemini Integration

The system also has an AI layer.

It integrates with Gemini and builds database context for AI analysis.

The AI functionality includes:

- Business analysis
- Forecasting
- Efficiency analysis
- Interactive chat
- Screen-aware context
- AI drawer
- AI reports

The architecture is moving toward:

```text
Raw Business Data
        ↓
Analytics Engine
        ↓
AI Context
        ↓
Gemini
        ↓
Business Recommendations
```

---

# 41. AI Forecasting

There is an explicit AI forecast function.

The AI receives aggregated business context and is asked to produce professional analysis.

The intention is for the owner to ask things like:

> What are my sales trends?

> Which customers are declining?

> What should I expect next month?

> Where are my biggest costs?

> Which items should I focus on?

---

# 42. Settings

Settings is another substantial module.

It includes:

### Company information

Used for invoices and documents.

### Billing settings

Controls billing/invoice defaults.

### Logo

Upload/remove company logo.

### Users

Admin can:

- Add users
- Edit users
- Delete users

### Text size

```text
Small
Medium
Large
XL
```

### Theme

```text
Light
Dark
```

### Database

Admin functionality includes:

```text
Export
Import
Cloud Upload
Cloud Import
Reset
```

### Gemini

AI configuration is also managed here.

---

# 43. Database Architecture

The backend is Supabase.

The DB abstraction layer exposes operations for:

```text
Customers
Drivers
Orders
Order Items
Invoices
Payments
Items
Users
Deductions
Quotations
Actions
Chemicals
Chemical Ledger
General Expenses
Trips
Settings
```

This is essentially the application's data-access layer.

---

# 44. High-Level Database Relationship

The core business data relationship is approximately:

```text
CUSTOMER
   │
   ├───────────────┐
   ↓               ↓
 ORDERS        CUSTOM PRICES
   │
   ↓
ORDER ITEMS
   │
   ↓
 INVOICE
   │
   ├──────→ PAYMENTS
   │
   └──────→ DEDUCTIONS
```

And:

```text
ITEM CATALOG
     ↓
ORDER ITEMS
     ↓
REVENUE ANALYTICS
```

Expenses:

```text
CHEMICAL MASTER
       ↓
CHEMICAL LEDGER
       ↓
CHEMICAL EXPENSES
```

and:

```text
GENERAL EXPENSES
       ↓
TOTAL OPERATING COST
```

Transport:

```text
DRIVER
  ↓
TRIP
  ↓
CUSTOMERS
  ↓
VISIT ORDER
```

---

# 45. Business Intelligence Model

The system's data naturally forms these major dimensions:

```text
CUSTOMER
ITEM
DRIVER
DATE
ORDER
PAYMENT
EXPENSE
CHEMICAL
TRIP
```

This is a good foundation for a proper BI system.

---

# 46. What the System Can Ultimately Answer

With the current architecture, management can investigate questions such as:

### Sales

> What were my sales this month?

> Which month had the highest revenue?

> Which day generates the most business?

> Are sales increasing or declining?

### Customers

> Which hotel gives us the most revenue?

> Which customers order most frequently?

> Which customers are declining?

> Which customers have unpaid balances?

### Items

> Which laundry item sells the most?

> Which item generates the most revenue?

> Which items are becoming less popular?

### Expenses

> What is our biggest expense?

> How much are chemicals costing us?

> Are chemical expenses increasing?

> Which expense category is growing fastest?

### Profitability

> What is monthly profit?

> What is yearly profit?

> What is our operating margin?

> Are costs growing faster than revenue?

### Operations

> Which days have the highest workload?

> How much does each driver travel?

> Which customers are frequently visited?

> How many trips are completed?

---

# 47. Important Technical Observation

The application is already much more advanced than a basic POS, but several parts are still implemented as a client-heavy application.

For example:

```text
Frontend
   ↓
Fetch large datasets
   ↓
JavaScript aggregation
   ↓
Chart rendering
```

rather than:

```text
Frontend
   ↓
Database views / RPC / optimized queries
   ↓
Aggregated results
   ↓
Frontend
```

For a small/medium washing center this can work initially, but if the number of orders becomes very large, the analytics layer should eventually move more aggregation into PostgreSQL/Supabase.

---

# 48. Important Data/Analytics Issues Identified

These areas should not be blindly preserved when extending the system.

## 1. Profit isn't necessarily true accounting profit

The current formula is effectively:

```text
Gross Revenue
-
General Expenses
-
Chemical Purchases
=
Net Operating Profit
```

This is useful as an **operating estimate**, but it isn't necessarily true accounting net profit.

It may not comprehensively account for:

- Salaries
- Depreciation
- Asset costs
- Tax
- Financing
- Outstanding liabilities
- Inventory valuation
- Other accounting adjustments

Therefore, a more accurate label would be:

**Operating Profit Estimate**

unless the business confirms that all operating costs are captured.

---

## 2. Expense timing needs careful treatment

The system uses:

```text
monthly_averaged_amount
```

for some general expenses.

This is useful, but analytics needs to distinguish:

```text
Cash Expense
vs
Recognized Operating Expense
```

Otherwise monthly profit can become misleading.

---

## 3. Order date vs pickup date vs delivery date

The system contains several relevant dates:

```text
created_at
pickup_date
delivery_date
expense_date
trip start date
trip end date
```

Analytics needs a clearly defined **business date**.

For example:

```text
Sales analytics → order/pickup date
Cash analytics → payment date
Expense analytics → expense date
Transport analytics → trip date
```

This distinction becomes important when building the next version.

---

## 4. Weekly calculations need improvement

The existing weekly bucket logic uses a custom week-number calculation.

For professional BI, use a consistent definition such as:

```text
ISO Week
Monday → Sunday
```

Otherwise year boundaries can produce misleading week comparisons.

---

## 5. Customer concentration should be measured

The current system identifies the top customer.

For stronger business decisions, additionally calculate:

```text
Top 1 customer %
Top 3 customers %
Top 5 customers %
```

This tells the owner how dependent the business is on a few hotels.

---

## 6. Item revenue isn't enough

The system emphasizes:

```text
Quantity
Revenue
```

But a more valuable metric is:

```text
Revenue
-
Direct Processing Cost
=
Item Contribution
```

Eventually, chemical usage, labor and processing costs should be allocated to items/services so management can identify **high-revenue but low-margin items**.

---

# 49. Overall Understanding of the System

The application can be understood as five interconnected business layers:

```text
                 SAGA WASHING CENTER
                         │
        ┌────────────────┼────────────────┐
        ↓                ↓                ↓
     SALES           OPERATIONS        FINANCE
        │                │                │
     Orders            Trips           Invoices
     Items             Drivers         Payments
     Customers         Visits          Deductions
        │                │                │
        └────────────────┼────────────────┘
                         ↓
                     EXPENSES
                         │
                  ┌──────┴──────┐
                  ↓             ↓
               General       Chemicals
               Expenses
                  │             │
                  └──────┬──────┘
                         ↓
                    ANALYTICS
                         │
                  ┌──────┴──────┐
                  ↓             ↓
               Reports         AI
```

The key insight is:

**The system already contains the foundations for a complete business management platform.**

It is not necessary to redesign the entire backend to create the driver app or improve the analytics system. The better approach is to build **focused interfaces on top of the existing business data and workflows**.

---

# 50. Android Mobile Application — Overall Strategy

The Android application should **not** try to squeeze the desktop system into a phone.

The core principle should be:

> **Office manages the business. Mobile manages the work happening outside the office.**

The desktop application remains the **command center**, while the Android application becomes the **field-work interface**.

---

# 51. Recommended Mobile App Structure

I recommend **5 main areas**:

```text
┌──────────────────────────────────────┐
│              SAGA APP                │
│                                      │
│  Home                                │
│  Transport                           │
│  Orders                              │
│  Customers                           │
│  More                                │
│                                      │
└──────────────────────────────────────┘
```

### Bottom Navigation

| Tab | Purpose | Access |
|---|---|---|
| 🏠 **Home** | Mobile operational dashboard | Yes |
| 🚚 **Transport** | Trips, customers visited, KM | Yes — PRIMARY |
| 📦 **Orders** | View/manage field-related orders | Yes |
| 👥 **Customers** | Customer information and visits | Yes |
| ☰ **More** | Other mobile-accessible functions/settings | Yes |

Five tabs is already close to the upper limit for comfortable mobile navigation. Do not put every desktop module in the bottom navigation.

---

# 52. Why Home Should Exist

The desktop system has a Dashboard, but the mobile Home should **not be a copy of the desktop Dashboard**.

The driver needs:

```text
Good morning 👋

Today's Activity

┌──────────────────────────┐
│ 🚚 Active Trip           │
│ TR-00042                 │
│ 4 Customers              │
│ 2 Visited                │
│                          │
│       Continue Trip →    │
└──────────────────────────┘

Today's Trips       Orders
     2                 8

Upcoming Visits
──────────────────────
🏨 Hotel ABC
10:30 AM

🏨 Hotel XYZ
11:15 AM
```

### Home should show:

- Active trip
- Today's trips
- Today's customer visits
- Pending field orders
- Recent orders
- Quick actions
- Important notifications
- Sync status

---

# 53. Transport — The Most Important Mobile Module

This should be the **primary function of the Android application**.

The existing desktop workflow already gives us an excellent foundation.

Mobile Transport should contain:

```text
Transport

[ Active Trip ]

[ Start New Trip ]

Today's Trips

Completed Trips
```

---

# 54. Start Trip

The mobile Start Trip screen should be extremely simple.

```text
Start New Trip

Trip ID
TR-00043

Date
10 Aug 2026

Start Time
09:32 AM

Starting KM
[  12450  ]

Customers
[ Select Customers ]

        [ Start Trip ]
```

The driver should not have to deal with unnecessary fields.

---

# 55. Customer Selection for Trip

After starting a trip:

```text
Select Customers

🔍 Search customer...

☐ Hotel ABC
☐ Hotel Grand
☐ Hotel XYZ
☐ Ocean Resort

        [ Continue ]
```

Then:

```text
Trip Route

1   Hotel ABC          ☰
2   Hotel Grand        ☰
3   Hotel XYZ          ☰
4   Ocean Resort       ☰

Drag to reorder
```

This directly maps to the existing `visit_order` concept.

---

# 56. Active Trip Screen

This should be the **most important screen in the whole mobile app**.

```text
ACTIVE TRIP

TR-00043

Started
09:32 AM

Starting KM
12,450 KM

────────────────────

CUSTOMER VISITS

✓ Hotel ABC
  Completed

● Hotel Grand
  Current

○ Hotel XYZ
  Upcoming

○ Ocean Resort
  Upcoming

────────────────────

Distance
—

[ Open Current Customer ]

[ End Trip ]
```

The driver should always be able to return to this screen.

---

# 57. Customer Visit Screen

When the driver reaches a customer:

```text
Hotel Grand

📍 Customer

Contact
071 XXX XXXX

Address
...

────────────────────

Today's Orders

Order #1042
Pickup Required

Order #1043
Delivery Required

────────────────────

Actions

[ Pickup ]

[ Delivery ]

[ Add Note ]

[ View Orders ]
```

This is where the mobile app becomes genuinely useful rather than being merely a mobile version of the desktop system.

---

# 58. Ending a Trip

The existing system already has:

- End date
- End time
- Final KM
- Distance calculation

Mobile should simplify this:

```text
End Trip

Trip
TR-00043

Start KM
12,450

Final KM
[ 12,487 ]

Distance

37 KM

End Time
10:48 AM

Notes
[________________]

       [ Complete Trip ]
```

Then:

```text
Trip Completed ✓

TR-00043

37 KM travelled
4 customers visited

[ Done ]
```

---

# 59. Orders Tab

The Orders tab should **not become the full desktop Order Management screen**.

The current Driver role has view/print access to Orders, not full order creation/edit/delete.

So mobile Orders should primarily be:

```text
Orders

🔍 Search orders...

Today
──────────────────

#1042
Hotel ABC
Pickup
Pending

#1043
Hotel Grand
Delivery
Ready

#1044
Hotel XYZ
Pickup
Completed
```

Use **cards**, not tables.

---

# 60. Order Details

```text
Order #1042

Hotel ABC

Status
● Ready for Pickup

────────────────

Items

Bed Sheet       20
Bath Towel      15
Pillow Case     30

────────────────

Pickup
10 Aug 2026

Delivery
11 Aug 2026

────────────────

[ Print Order ]

[ View Customer ]
```

No complex invoice/payment editing should appear here.

---

# 61. Customers Tab

This is another important mobile feature.

```text
Customers

🔍 Search customers...

┌──────────────────────┐
│ Hotel ABC            │
│ Colombo              │
│ 3 orders today       │
│                      │
│ View →               │
└──────────────────────┘

┌──────────────────────┐
│ Hotel Grand          │
│ Colombo              │
│ 1 pending order      │
└──────────────────────┘
```

---

# 62. Customer Details

```text
Hotel ABC

📍 Address
...

📞 Contact
...

────────────────

Today's Orders
3

Pending Orders
1

────────────────

Recent Orders

#1042
#1039
#1032

────────────────

[ Edit Customer ]
[ View Orders ]
[ Add Order? ]
```

For the existing Driver role, **do not automatically expose Add Order**.

The current role permissions say the driver can view orders but cannot create/edit/delete them.

Preserve that permission model unless you deliberately change the business rules.

---

# 63. Adding Customers

The existing driver role allows:

> Add customer  
> Edit customer  
> Cannot delete customer

Therefore mobile can have:

```text
Customers

                [+]
```

and:

```text
Add Customer

Customer Name
[________________]

Contact
[________________]

Address
[________________]

Email
[________________]

        [ Save Customer ]
```

This is a good mobile function because a driver may encounter a new customer while operating in the field.

---

# 64. More Tab

This is where the remaining mobile-accessible functions can go.

```text
More

────────────────────

📦 Items
View Item Catalogue

💰 Payments
View Payment Information

🧾 Deductions
View Deduction Information

👨‍✈️ Drivers
View Driver Information

💵 Expenses
Field Expense Entry*

🕘 Recent Activity
Recent Actions

────────────────────

⚙️ Settings

👤 My Profile

🚪 Logout
```

Do not automatically expose every item from the desktop system. Expose only things that have a genuine field-use case.

---

# 65. What Should NOT Be on Mobile

Keep these **office-only**:

## ❌ Data Analytics

Keep it on desktop.

Analytics contains:

- Revenue analysis
- Expense analysis
- Profit
- Customer performance
- Item performance
- Charts
- Forecasting
- AI business analysis

This is management functionality.

---

## ❌ Invoice Management

Keep full invoice management on desktop.

Mobile should not allow:

- Creating invoices
- Editing invoices
- Deleting invoices
- Consolidating invoices
- Managing invoice numbering
- Financial reconciliation

The driver can see relevant order/billing information where necessary.

---

## ❌ Full Reports

Office only.

Examples:

- Monthly revenue
- Customer billing
- Financial reports
- Driver performance reports
- Full business reports

---

## ❌ User Management

Office/Admin only.

---

## ❌ Database Management

Definitely desktop only:

```text
Backup
Restore
Import
Export
Reset Database
Cloud Database Operations
```

---

## ❌ AI Business Analytics

Keep Gemini business-analysis functionality on desktop.

The driver does not need:

> "Analyze our monthly profitability."

---

# 66. What About Expenses?

This needs a distinction.

The desktop Expenses module is mainly an **office/financial management function**.

But there is one possible mobile function:

### Field Expense Entry

For example, if a driver pays for:

- Parking
- Toll
- Minor vehicle expense
- Other trip-related expense

you could eventually have:

```text
More
   ↓
Expenses
   ↓
Add Field Expense
```

Only implement this if the business actually needs drivers to record expenses.

For V1, it can remain office-only.

---

# 67. What About Items?

Items should be **read-only** on mobile.

The driver might need to know:

```text
Bed Sheet
Bath Towel
Pillow Case
```

but shouldn't manage:

- Item prices
- Item deletion
- Catalogue structure
- Customer pricing

Therefore:

```text
More
 ↓
Items
 ↓
Read-only catalogue
```

---

# 68. What About Payments?

Make this **read-only initially**.

The driver may need to see:

```text
Order #1042

Total       Rs. 25,000
Paid        Rs. 20,000
Balance     Rs.  5,000
```

But don't give the driver financial editing capability unless the business specifically requires collection/payment entry.

---

# 69. Recommended Mobile Permissions

Define mobile permissions explicitly.

| Function | Driver |
|---|---|
| Home | View |
| Transport | Create/Edit/Complete |
| Customers | Create/Edit/View |
| Orders | View |
| Order Print | Yes |
| Items | View |
| Payments | View |
| Deductions | View |
| Drivers | View |
| Expenses | No initially |
| Invoices | No |
| Analytics | No |
| Reports | No |
| AI Analytics | No |
| User Management | No |
| Database | No |
| Settings | Limited |

Preserve the existing desktop security model.

---

# 70. Recommended Navigation

The recommended bottom navigation is:

```text
             SAGA MOBILE

┌────────────────────────────────────┐
│                                    │
│              CONTENT               │
│                                    │
│                                    │
│                                    │
├────────────────────────────────────┤
│ Home │ Transport │ Orders │ Customers │ More │
└────────────────────────────────────┘
```

Visually:

**Home** — 🏠  
**Transport** — 🚚  
**Orders** — 📦  
**Customers** — 👥  
**More** — ☰

---

# 71. Why Transport Should Be the Center of the App

The user is mobile because they are **travelling**.

So the app should be organized around the driver's real-world workflow:

```text
WHERE AM I GOING?
       ↓
WHO AM I VISITING?
       ↓
WHAT DO I NEED TO DO?
       ↓
WHAT ORDERS ARE INVOLVED?
       ↓
DID I COMPLETE THE VISIT?
       ↓
WHAT IS MY NEXT CUSTOMER?
       ↓
END TRIP
```

That's much more useful than a desktop-style:

```text
Dashboard
Orders
Invoices
Reports
Analytics
Settings
...
```

---

# 72. Mobile Home Should Be Context-Aware

Instead of a generic dashboard, Home should dynamically show what matters **right now**.

### Before a trip

```text
Good Morning 👋

You have 2 trips today.

┌───────────────────────┐
│ Today's Trip          │
│ 4 Customers           │
│ Not Started           │
│                       │
│ [ Start Trip ]        │
└───────────────────────┘
```

### During a trip

```text
Good Morning 👋

Active Trip
TR-00043

2 / 4 Customers

[ Continue Trip ]

Next:
Hotel Grand
```

### After trip

```text
Today's Activity

✓ Trip completed

37 KM
4 customers

Orders
8 handled

[ View Summary ]
```

This is much better UX.

---

# 73. Mobile UI Design Language

Do not reproduce the desktop styling.

Use:

### Cards

Instead of tables.

### Large touch targets

Aim for approximately:

```text
44–48 px
```

for important controls.

### Bottom sheets

For:

- Customer selection
- Item selection
- Filters
- Quick actions

### Full-screen forms

For:

- Start Trip
- End Trip
- Add Customer

### Sticky primary buttons

Example:

```text
┌──────────────────────────┐
│                          │
│     Form content         │
│                          │
│                          │
├──────────────────────────┤
│ [      Save Customer   ] │
└──────────────────────────┘
```

---

# 74. Don't Use Desktop UI Patterns

Avoid:

- ❌ Sidebar
- ❌ Huge data tables
- ❌ Tiny buttons
- ❌ Dense forms
- ❌ Desktop modal dialogs
- ❌ Multiple simultaneous windows
- ❌ Excessive filters
- ❌ 20 buttons on one screen

Instead:

- ✅ Cards
- ✅ Search
- ✅ Tabs
- ✅ Bottom sheets
- ✅ Large buttons
- ✅ Step-by-step workflows
- ✅ Clear status indicators
- ✅ One primary action per screen

---

# 75. The Driver's Main Journey

The most important UX journey should be:

```text
LOGIN
 ↓
HOME
 ↓
TODAY'S TRIP
 ↓
START TRIP
 ↓
STARTING KM
 ↓
SELECT CUSTOMERS
 ↓
ARRANGE VISIT ORDER
 ↓
ACTIVE TRIP
 ↓
CUSTOMER 1
 ↓
CUSTOMER ACTION
 ↓
CUSTOMER 2
 ↓
CUSTOMER ACTION
 ↓
CUSTOMER 3
 ↓
CUSTOMER ACTION
 ↓
CUSTOMER 4
 ↓
END TRIP
 ↓
FINAL KM
 ↓
TRIP SUMMARY
 ↓
COMPLETE
```

The driver should be able to complete the entire process with **minimal typing**.

---

# 76. Offline Support — Very Important

Because this is a driver application, don't assume constant internet connectivity.

The mobile application should be designed as:

```text
Android App
     ↓
Local Cache
     ↓
Sync Engine
     ↓
Supabase
```

For example:

```text
Driver starts trip
       ↓
Saved locally
       ↓
Internet unavailable
       ↓
Driver continues working
       ↓
Internet returns
       ↓
Sync
       ↓
Supabase updated
```

At minimum, cache:

- Driver profile
- Customers
- Relevant orders
- Active trip
- Trip customers
- Trip status
- Recent data

And show:

```text
🟢 Synced
🟠 Syncing...
🔴 Offline
```

---

# 77. Conflict Handling

You also need to think about synchronization.

Example:

```text
Desktop changes customer
        ↓
Mobile has old customer
```

Therefore the mobile app needs:

```text
updated_at
```

timestamps and synchronization rules.

For example:

```text
Mobile record updated_at
vs
Server record updated_at
```

The server should generally be the source of truth.

---

# 78. Suggested Technical Architecture

Since the existing system uses Supabase, structure the Android app as:

```text
┌─────────────────────────────┐
│       Android UI            │
│                             │
│ Jetpack Compose             │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│       ViewModel             │
│                             │
│ UI State / Business Logic   │
└──────────────┬──────────────┘
               ↓
┌─────────────────────────────┐
│       Repository            │
│                             │
│ CustomerRepository          │
│ OrderRepository             │
│ TripRepository              │
└──────────────┬──────────────┘
               ↓
       ┌───────┴────────┐
       ↓                ↓
   Local DB          Supabase
       │                │
       └────── Sync ────┘
```

For a new Android application, strongly favor **Kotlin + Jetpack Compose** rather than reproducing the web UI inside a WebView.

---

# 79. Suggested Android Project Structure

```text
app/
│
├── data/
│   ├── local/
│   ├── remote/
│   ├── models/
│   └── repository/
│
├── domain/
│   ├── model/
│   └── usecase/
│
├── ui/
│   ├── navigation/
│   ├── theme/
│   │
│   ├── home/
│   │
│   ├── transport/
│   │   ├── TransportScreen
│   │   ├── StartTripScreen
│   │   ├── ActiveTripScreen
│   │   ├── CustomerVisitScreen
│   │   └── EndTripScreen
│   │
│   ├── orders/
│   │   ├── OrdersScreen
│   │   └── OrderDetailsScreen
│   │
│   ├── customers/
│   │   ├── CustomersScreen
│   │   ├── CustomerDetailsScreen
│   │   └── AddCustomerScreen
│   │
│   └── more/
│       ├── MoreScreen
│       ├── ItemsScreen
│       ├── PaymentsScreen
│       └── SettingsScreen
│
└── sync/
    ├── SyncManager
    └── SyncWorker
```

---

# 80. Backend Strategy

**Do not create a completely separate database for the Android application.**

Use:

```text
                    Supabase
                       │
          ┌────────────┴────────────┐
          ↓                         ↓
     Desktop Web                Android
     Application                Application
```

Both applications use the same business data.

This gives:

```text
Office creates order
       ↓
Supabase
       ↓
Driver sees order
       ↓
Driver completes pickup
       ↓
Supabase
       ↓
Office sees update
```

This is the architecture you want.

---

# 81. Secure Backend Access

Do not let Android directly do everything simply because a button is hidden.

The Android app should have a restricted data-access layer.

For example:

```text
Driver
 ↓
Can read:
Customers
Orders
Items
Trips

Can write:
Trips
Customer changes

Cannot write:
Invoices
Analytics
Financial reports
User management
```

This should be enforced at the **Supabase Row Level Security/database level**, not merely by hiding buttons.

That is critical.

---

# 82. Recommended Mobile V1

Do not build everything simultaneously.

Build V1 around the driver's actual workflow.

### Phase 1

```text
Login
Home
Transport
Customers
Orders
Settings
```

Specifically:

### Login

- Authentication
- Role detection
- Session persistence

### Home

- Today's trip
- Active trip
- Today's orders
- Sync status

### Transport

- New trip
- Start trip
- Customer selection
- Visit order
- Active trip
- Customer visit
- End trip
- Trip history

### Customers

- Search
- View
- Add
- Edit

### Orders

- Search
- View
- Order details
- Print

### Settings

- Theme
- Text size
- Profile
- Logout

---

# 83. V2

After V1 is stable:

```text
Payments
Deductions
Items
Notifications
Field Expenses
Enhanced customer operations
Offline synchronization
```

---

# 84. V3

Then consider:

```text
GPS
Navigation
Route optimization
Geofencing
Push notifications
Proof of delivery
Photo capture
Digital signatures
Advanced driver analytics
```

These are useful, but do not put them into the first version unless the business actually needs them.

---

# 85. Final Recommended Mobile Information Architecture

```text
SAGA DRIVER / MOBILE APP
│
├── 🏠 HOME
│   ├── Today's Activity
│   ├── Active Trip
│   ├── Today's Orders
│   ├── Upcoming Visits
│   └── Sync Status
│
├── 🚚 TRANSPORT
│   ├── Active Trip
│   ├── Start New Trip
│   ├── Customer Selection
│   ├── Visit Sequence
│   ├── Customer Visit
│   ├── End Trip
│   └── Trip History
│
├── 📦 ORDERS
│   ├── All Relevant Orders
│   ├── Search
│   ├── Filters
│   └── Order Details
│
├── 👥 CUSTOMERS
│   ├── Customer List
│   ├── Search
│   ├── Customer Details
│   ├── Add Customer
│   ├── Edit Customer
│   └── Customer Orders
│
└── ☰ MORE
    ├── Items
    ├── Payments
    ├── Deductions
    ├── Drivers
    ├── Settings
    │   ├── Profile
    │   ├── Theme
    │   ├── Text Size
    │   └── Logout
    │
    └── About
```

---

# 86. Office vs Mobile

| Function | Office Web | Android |
|---|:---:|:---:|
| Dashboard | ✅ | Simplified |
| Transport | ✅ | **✅ Full** |
| Customers | ✅ | **✅ Full** |
| Orders | ✅ Full | **✅ Field View** |
| Items | ✅ Full | Read-only |
| Payments | ✅ Full | Read-only initially |
| Deductions | ✅ Full | Read-only initially |
| Drivers | ✅ Full | Read-only |
| Expenses | ✅ Full | Later |
| Invoices | **✅ Full** | ❌ |
| Quotations | **✅ Full** | ❌ |
| Analytics | **✅ Full** | ❌ |
| Reports | **✅ Full** | ❌ |
| AI Business Analysis | **✅ Full** | ❌ |
| User Management | **✅** | ❌ |
| Database | **✅** | ❌ |
| Backup/Restore | **✅** | ❌ |
| Settings | **✅ Full** | Limited |
| Trip Operations | ✅ | **✅ Full** |

---

# 87. Final Architecture Recommendation

Do not think of this as:

> **"Let's make the desktop application responsive."**

Think of it as:

> **"Let's build a dedicated field-operations application that connects to the same SAGA business system."**

The desktop application remains the **command center**.

The Android application becomes the **field-work interface**.

```text
                    SAGA SYSTEM
                         │
              ┌──────────┴──────────┐
              ↓                     ↓
        OFFICE WEB              ANDROID
        COMMAND CENTER          FIELD APP
              │                     │
      ┌───────┼───────┐       ┌─────┼─────┐
      ↓       ↓       ↓       ↓     ↓     ↓
   Finance Analytics Reports  Trips Customers Orders
      │       │       │       │     │     │
      └───────┴───────┴───────┴─────┴─────┘
                         ↓
                      SUPABASE
```

## Final Principle

**Office manages the business.  
Mobile manages the work happening outside the office.**

The Android app should therefore be **fast, simple, touch-friendly, field-oriented, permission-restricted, and capable of continuing to work when connectivity is poor.**

For SAGA, **Transport should be the central feature**, with Home acting as a live operational summary, while Customers and Orders support the transport workflow rather than becoming separate complex management systems.

That is the architecture recommended for SAGA.

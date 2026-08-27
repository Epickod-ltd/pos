# EpicPOS v1.1.3 — First Public Release

**Released:** 27 August 2026
**Platform:** Windows 10 / 11 (64-bit)
**Installer:** `EpicPOS_Setup_v1.1.3.exe` (signed)

---

This is the first public release of **EpicPOS** — a multi-tenant Point of Sale system with real double-entry accounting built in. Not a till with a report screen bolted on: every sale, return, purchase and payment posts a balanced journal entry, so your books are correct the moment the drawer closes.

## What's in the box

### Point of Sale
- Fast checkout with barcode scanning, discounts and multiple payment methods
- Estimates and quotations, convertible to sales in one click
- Sales and purchase returns with full journal treatment
- Cash drawer sessions with opening/closing balances and variance tracking
- Shift management and employee handoff
- End-of-day reconciliation reports
- **Quick Actions** landing screen — the common jobs, one keystroke away

### Inventory
- Products with SKUs, categories, brands and colour/variant support
- Batch tracking, including batches on zero-stock products
- Stock adjustments with a full audit trail
- Inter-branch transfers with an approval workflow
- Purchase orders with Receive Order
- Barcode generation and printing

### Accounting (GAAP / IFRS aligned)
- Double-entry bookkeeping on every transaction — nothing posts unbalanced
- Hierarchical chart of accounts (Assets, Liabilities, Equity, Revenue, Expenses)
- Manual journal entries and vouchers
- Cashbook and daybook registers
- Trial Balance, Balance Sheet and Profit & Loss
- Sales, Purchase and General ledgers
- Amounts stored in integer cents — no floating-point drift, ever

### Multi-tenant by design
- Company → Branch → User hierarchy with Firestore-level data isolation
- Four roles: Super Admin, Company Admin, Branch Manager, Seller
- Per-company feature flags and subscription plans

### Everything else
- Multi-currency with exchange rates
- Receivables, payables, and receive/make payments
- Installment plans for customer financing
- CRM for customers and suppliers, with credit terms
- HRMS — employees, departments, attendance, leave, payroll, performance
- Real-time sales analytics dashboard
- Loyalty points, configurable discounts and promotions
- Complete audit log
- PDF export across reports and documents

### Built to stay current
- In-app update checks with an app-wide banner when a new version is available
- Installer signatures verified before anything is applied
- App version resolved at runtime, so what the About screen says is what you're running

## Installing

1. Download `EpicPOS_Setup_v1.1.3.exe`.
2. Run it. The Visual C++ 2015–2022 x64 runtime is bundled and installed if missing.
3. Sign in with the credentials issued for your company.

No manual configuration is needed — the installer sets up shortcuts, file associations and the update channel.

## Requirements

| | |
|---|---|
| OS | Windows 10 (1809+) or Windows 11, 64-bit |
| RAM | 4 GB minimum, 8 GB recommended |
| Disk | 500 MB free |
| Network | Internet connection for sync (the app works offline and reconciles when reconnected) |

## Known limitations

- Windows desktop only for this release.
- Data sync requires an initial online sign-in before offline use.
- Reporting periods follow the company's configured fiscal calendar; changing it mid-year requires a re-run of period reports.

## Support

Questions, bugs or licensing: **https://epickod.com/support**

---

*Thank you for being here for the first one. — The Epickod team*

---

## Short changelog (for the in-app update dialog)

```
EpicPOS 1.1.3 — first public release.

• Full POS: checkout, returns, estimates, cash drawer, shifts, end-of-day
• Double-entry accounting on every transaction, with Trial Balance, P&L and Balance Sheet
• Inventory with batches, adjustments, inter-branch transfers and purchase orders
• Multi-tenant company/branch/user model with role-based access
• Multi-currency, payments, installments, CRM, HRMS, loyalty and audit log
• New Quick Actions landing screen
• Signed installer with verified in-app updates
```

**Full Changelog**: https://github.com/Epickod-ltd/pos/commits/v1.1.3

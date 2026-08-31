# EpicPOS v1.1.7

**Released:** 31 August 2026
**Platform:** Windows 10 / 11 (64-bit)
**Installer:** `EpicPOS_Setup_v1.1.7.exe` (signed)
**Upgrades from:** v1.1.6

---

Two things in this release, both about not making you hunt. **Ctrl+K** reaches any screen, product, customer, supplier or action from wherever you are, so navigation stops being a matter of remembering which menu something lives under. And the **cart line** was rebuilt: it used to show a name, a price and a quantity, which told a cashier nothing about what they were actually selling. It now carries the brand, the category, the stock left, the batch and its expiry, and what the customer is saving — without getting taller.

## Global search — Ctrl+K

One box for everything in the system. Press **Ctrl+K** anywhere (**⌘K** on Mac), or click the search icon in the title bar.

- **Searches across five kinds of thing at once** — actions, screens, products, customers and suppliers — with results grouped by kind rather than mixed into one list.
- **Products match on more than their name**: SKU, serial number, brand and category all hit, so a part number or a brand finds the item just as well.
- **Typed prefixes** for anyone who would rather not reach for the mouse: `>` for actions, `/` for screens, `#` for products, `@` for people. Or use the scope tabs.
- **Recents lead the empty state.** The last six things you opened are there the moment the palette appears, which makes hopping between two screens a double keystroke. They persist across restarts.
- **Arrow keys to move, Enter to open, Esc to close.** The palette never requires the mouse.
- **Permission-aware.** Screens and actions you cannot reach are not offered, so the palette never sends anyone to a locked door.

## The cart line, rebuilt

The sale terminal's cart line is now two tiers: the transaction on top, the context on a recessed rail beneath it. Same height as before, considerably more on it.

- **Brand and category** on every line, so two similarly named products are no longer a guess.
- **Stock remaining** — and it counts down as you change the quantity, because the number a cashier needs is what is left *after* this line sells, not the untouched catalogue figure. Green while there is room, amber under five, red on the last unit, and a red line with an *over stock* warning if the quantity exceeds what is on hand.
- **Batch and expiry, spelled out.** The lot number is its own neutral chip; the expiry sits beside it with the full date *and* the time remaining — `Exp 15 Aug 2028 · 1y 11m left` — colour-coded expired / today / ≤30 days / ≤90 days / clear. Nobody counts days at the counter.
- **MRP and savings.** When a product's market price is above what you are charging, the MRP shows struck through next to the unit price and the line's total saving is stated on the rail.
- **Margin behind the eye.** Tapping the cost toggle now reveals both the cost price and the line's margin — percent and money — coloured green, amber or red. Cost alone did not answer the question anyone was asking it.
- **Serial number** on every line, so a line is identifiable without opening anything.
- **Discounts read at a glance**: the percentage sits inline as a chip and the total shows the pre-discount figure struck through beneath it.
- Prices, quantities and totals use tabular figures, so the numbers stop shifting sideways as quantities change.
- The three cart densities have all been re-tuned around the new layout.

## Fixed

- **Adding a batch-tracked product from the search bar opened a second, batch-less line.** Pressing Enter in the search box was the only add route that skipped batch selection — every other route (quick add, the product grid, the search dialog) picks the nearest-expiry batch first. So scanning or searching a medicine that was already in the cart under a batch created a duplicate line with no batch attached, and stock came off the wrong place. Search and Enter now take the same path as everything else. The same fix restores the IMEI picker on that route, which was also being skipped.

## Upgrading

Run `EpicPOS_Setup_v1.1.7.exe`. Settings, data and credentials carry over; no migration step is needed, and no new Firestore indexes are required.

## Notes

- Carts already holding a duplicated batch-less line from the bug above keep both lines — they are two separate entries. Clear the line and re-add it.


---

# EpicPOS v1.1.5

**Released:** 30 August 2026
**Platform:** Windows 10 / 11 (64-bit)
**Installer:** `EpicPOS_Setup_v1.1.5.exe` (signed)
**Upgrades from:** v1.1.3

---

The headline of this release is **credit management**. Selling on credit used to mean an invoice with a balance and a hope; it now means payment terms, a due date, a customer ledger, an aging bucket and a statement you can hand to the customer. Alongside that: returns have been rebuilt so they actually reach your books, and pharmacies get patient records and expiry tracking.

## Credit & Receivables

Sales and purchases on account are now a first-class workflow rather than an unpaid balance.

- **Payment terms master** — define terms once (Net 15, Net 30, end-of-month, custom), set a default per customer and per supplier, and override on any individual invoice.
- **Due dates calculated from terms** on every credit sale and purchase, so what is overdue is a fact rather than an opinion.
- **Credit setup validation** — selling on credit to a customer with no terms and no limit configured now stops and tells you what is missing, instead of silently booking a receivable nobody is tracking.
- **Credit limit enforcement** at the point of sale, checked against the customer's existing outstanding balance plus the new sale.
- **Customer and supplier aging** — outstanding balances bucketed by how overdue they are, calculated live from the ledger rather than from a stored snapshot that drifts.
- **Party statements** — a printable account statement for any customer or supplier, opening balance through to closing, exportable to PDF.
- **Receivables & Payables** screen rebuilt around the above.

## Payments

- **Receive and make payments** against specific invoices, with the allocation recorded — you can see which payment settled which invoice.
- **Partial and multiple payments** on a single invoice, each posting its own journal entry.
- **Payments register** reworked with a clearer table, direction (received vs paid) shown explicitly, and filtering.

## Cash & Daily Operations

- **Daily Closing** — an end-of-day cash count and sign-off, with variance against expected cash.
- **Cashbook** rebuilt to work across multiple cash and bank accounts rather than assuming a single till.
- **Day Book** reworked to show every transaction of the day in one register, cash and non-cash alike.

## Returns — rebuilt

Returns raised from the **Sale Return** screen were being recorded in a private collection of their own. They never became a transaction, which meant they never reached sale history, the dashboard, the sales reports or the cashbook: the stock came back and the money went out, and nothing downstream knew.

- Every return — from the terminal's Returns mode or from the Sale Return screen — now goes through **one pipeline**, writing a proper return transaction alongside its reversal journal entry.
- Returns now appear in **sale history**, on the **dashboard**, in **sales reports** and in the **cashbook**, and the original invoice is badged *Returned* or *Part Returned*.
- **Refunds are prorated correctly.** Line totals exclude tax and sit before invoice-level discount, so refunding the raw line total gave back the wrong money on any taxed or discounted sale. The refund is now that item's share of what the invoice actually charged.
- **A refund now decreases cash.** Terminal returns had been posting a sale-shaped journal entry, so a refund *increased* the cash balance. They now post the reversal.
- **Partial returns are tracked per line.** Returning the same item twice was possible because nothing recorded what had already gone back. Each line now caps at what is left, and shows how much was already returned.
- **Store credit** refunds post to the customer's account instead of the drawer.

## Purchase Returns

- Return quantities are capped by **what you can actually send back**: whatever the order still has outstanding, limited by stock on hand. Buy 10 and sell 2, and only 8 can go back — but if a later delivery has stock up to 18, the full 10 is returnable again.
- The cap is enforced again at submit against live stock, so a sale on another till while the return form is open can no longer drive inventory negative.
- Returned quantities are now recorded against the purchase order, so the same units cannot be returned twice.
- Stock changes and the order update now commit together — a failure part-way through used to leave some products decremented and the rest untouched.

## Pharmacy

- **Business type** setting. Set a company to *Pharmacy* to switch on trade-specific screens; *General Retail* leaves everything as it was.
- **Patients register** — patient records with history, and the ability to record which patient a medicine was dispensed to at the point of sale.
- **Expiry Aging report** — every batch-tracked product with an expiry date, soonest first, colour-coded by remaining shelf life: expired, ≤30 days, ≤60 days, ≤90 days, and clear. Summary cards show the count and the money sitting in each band, click one to filter the table, and the whole thing exports to PDF, Excel, CSV or print.

## Elsewhere

- **Live menu search** in the sidebar and mobile drawer — start typing to find any screen.
- **MRP shown in the cart**, so the printed price is visible next to what you are charging.
- Product detail action buttons no longer overflow their labels in a narrow pane.
- Fixed a widget-tree fault that could corrupt a screen mid-refresh. Screens backed by a live data stream flip between loading and content routinely, and doing so inside the 250 ms transition tripped a duplicate-key error and a cascade of failures behind it.
- Sales and returns that fail to sync are no longer silently dropped from the queue — they retry, and land in the recoverable failed list if they still cannot save.

## Upgrading

Run `EpicPOS_Setup_v1.1.5.exe`. Settings, data and credentials carry over; no migration step is needed.

**For self-hosted deployments:** this release adds a Firestore index for the Expiry Aging report. Deploy it with `firebase deploy --only firestore:indexes` before or after upgrading — the report falls back to a slower path and tells you if the index is missing, so nothing breaks either way.

## Notes

- Purchase orders created before this release have no record of past returns, so anything returned previously counts as un-returned against the order. The stock-on-hand cap still applies.
- Sales invoiced before this release carry no payment term, and appear in aging based on their invoice date.


---

<details>
<summary>Earlier releases</summary>

# EpicPOS v1.1.2 — First Public Release

**Released:** 27 August 2026
**Platform:** Windows 10 / 11 (64-bit)
**Installer:** `EpicPOS_Setup_v1.1.2.exe` (signed)

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

1. Download `EpicPOS_Setup_v1.1.2.exe`.
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
EpicPOS 1.1.2 — first public release.

• Full POS: checkout, returns, estimates, cash drawer, shifts, end-of-day
• Double-entry accounting on every transaction, with Trial Balance, P&L and Balance Sheet
• Inventory with batches, adjustments, inter-branch transfers and purchase orders
• Multi-tenant company/branch/user model with role-based access
• Multi-currency, payments, installments, CRM, HRMS, loyalty and audit log
• New Quick Actions landing screen
• Signed installer with verified in-app updates
```

</details>

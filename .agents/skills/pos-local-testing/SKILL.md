---
name: pos-local-ui-testing
description: Run a first-install Laravel POS cash-sale regression through the browser.
---

# Local POS UI testing

App commands run in the nested `point-of-sales-main/` directory. Follow the
environment blueprint for MySQL, migrations, storage linking and `composer run dev`.
Use localhost:8000; Vite runs on :5173.

## Devin Secrets Needed
None for a fresh local installation: create a test admin through `/setup`.
For an existing installation, use an authorized test account; do not reset data
or run the destructive demo seeder merely to obtain credentials.

## Workflow prerequisites
- There are no default users after the normal seed. Complete the first-install
  wizard and retain the test admin credentials securely.
- Login has a honeypot and minimum submit timer; avoid immediate automated submit.
- If email verification is required with `MAIL_MAILER=log`, use the UI resend
  action and the genuine verification URL in `storage/logs/laravel.log`.
  Do not mark verification directly in the database.
- Categories and products require image uploads. Product image rendering requires
  `php artisan storage:link`. Set test product tax to zero for exact cash totals.
- Initial stock is assigned to PUSAT, a central warehouse. To sell at a sales
  outlet, use Stock Transfers: create draft from PUSAT to the sales warehouse,
  send, then receive. Confirm completed status.
- Select the sales outlet in the header before opening a cashier shift;
  the outlet selector is disabled while the shift is active.
- Open a shift with known opening cash, then navigate to Transactions.
  Dismiss the first-use guided tour if it covers controls.
- POS requires a customer. Quick-create requires name, phone and address and
  automatically selects the new customer. Use synthetic local test details.
- The POS cart/payment panel scrolls independently. Cash amount fields may be
  below the fold even while the sticky total and checkout button are visible.

## Concrete cash-sale oracle
For stock 10, cost 6000, price 10000, tax 0: sell one item for cash 20000.
Expect paid invoice total 10000/change 10000, one persisted history row after
reload, sales report revenue 10000/profit 4000, and remaining product stock 9.
Customer phone formatting/preservation should be checked after a full reload,
not only in the optimistic quick-create selection.

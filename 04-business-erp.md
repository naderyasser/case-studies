# Multi-tenant Business ERP — HR · Inventory · Accounting · POS

**Kind:** one platform serving many companies, each on its own subdomain with its own branding
**Built on:** Frappe / ERPNext, with a Next.js 16 + React 19 frontend

## Systems
| System | What it covers |
|---|---|
| HR | employees, attendance, leaves, payroll, shifts, biometric devices, automatic attendance |
| Inventory | warehouses, products, transfers, returns, audit log, reconciliation |
| Accounting | chart of accounts, general ledger, AR/AP aging, P&L, balance sheet, bank reconciliation, VAT |
| Cashier (POS) | checkout, returns, held transactions, reports |
| Purchases · Tasks | purchase orders, task tracking |
| Sales reps | orders, payments, route plans, stock requests (PWA) |
| Admin | users, roles, system settings, domains |

A REGA-compliant real-estate marketplace is built on the same core.

## How it works
- **Tenant per subdomain:** exact domain → wildcard subdomain → default; branding injected at runtime.
- **Role hierarchy with module-level and page-level protection**, and roles assigned automatically
  when an employee is created, based on their designation.

`Frappe` `ERPNext` `Next.js` `React` `TypeScript` `Multi-tenant`

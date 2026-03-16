# Discharge Bill Report

A Jinja HTML template that generates a printable A4 discharge bill for a patient's account.

## What It Renders

### Patient & Encounter Info
Displays the patient's name, age, gender, address, official identifier, admission/discharge date-time, and bed location at the top of the bill.

### Charge Items (Billed,Billable,Paid)
Lists all charge items associated with the account, **grouped by their charge item resource category**. Each row shows:
- Date, invoice number, item title, status, unit rate (Base component), quantity, and line total.
- A category-level subtotal is shown per group.
- A **Net Total** is computed across all categories.

### Returned Charge Items
A separate section listing charge items that were returned, using the same category-grouped layout and calculating a **Net Returned Total**.

### Payment Reconciliations
Lists all payment transactions linked to the account, **grouped by reconciliation type** (e.g., Advance, Final). Each row shows the date, invoice number, payment method, and amount, with a **Net Payment Total** at the bottom.

### Account Summary
A summary row showing:
| Billed (Gross) | Total Paid | Amount Due | Total Billable |
|---|---|---|---|

## Template Variables

| Variable | Description |
|---|---|
| `account` | The patient account object (patient info, encounter, charge items, payments, totals) |
| `logo_img` | URL of the facility logo image |

## Filters / Functions Used
- `current_date()` — renders today's date in the header.
- `\|datetime` — formats a datetime string.
- `\|date` — formats a date string.
- `\|float` — casts a value to float for display.
- `\|int` — casts quantity to integer.
- `\|groupby` — groups payment reconciliations by `reconciliation_type`.
- `\|selectattr` — filters identifiers to find the official one.

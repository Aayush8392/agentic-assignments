# Assignment 1 – Zapier: Automated Payment Reminder

A no-code automation built with **Zapier**, **Google Sheets** and **Gmail** that spots unpaid invoices in a spreadsheet and emails reminders without anyone checking by hand.

## Links

- Zapier agent: https://agents.zapier.com/copy/65c3ba40-b434-45e2-9a65-0150659df063 (free account, so the link may expire)
- Google Sheet (SCIT Invoice test data): https://docs.google.com/spreadsheets/d/1C8QezSuCf7yhpllobUne75CGsOU9IA417mUSJmodt6U/edit?usp=sharing

## What it does

On a daily schedule the Zap reads all invoice rows and keeps only the ones with `Status = Pending` that also have a recipient email. For each of them it sends a personalised reminder and then updates the row so the reminder is on record.

```text
Schedule by Zapier
        |
Google Sheets – Get Many Spreadsheet Rows
        |
Filter – Status = Pending
        |
Filter – Email exists
        |
Gmail – Send payment reminder
        |
Google Sheets – Update spreadsheet row
```

Rows that are Paid, Completed, or Pending without an email are skipped. If nothing is pending, the run ends without sending anything.

## Spreadsheet columns

`Sr`, `Invoice ID`, `Type`, `Name`, `Amount`, `Account No`, `Bank Name`, `Status`, `Transaction ID`, `Reminder Count`, `Escalated`, `Email`

## Reminder email

Subject: `Payment Reminder - Invoice [Invoice ID] Due`

The body greets the customer by name and lists the invoice ID, amount, account number, bank, payment type and transaction ID, asks them to pay promptly, and tells them to ignore the message if they have already paid.

## Reminder stages

| Stage | Condition | Outcome |
|---|---|---|
| First reminder | Pending and email present | Email sent, `Reminder Count` goes 0 → 1 |
| Second reminder | Pending, `Reminder Count` = 1, transaction ID present, not escalated | Stricter HTML email, `Reminder Count` → 2 |
| Payment confirmation | Customer replies that payment is done | `Status` updated to completed |

`Status` stays Pending after a reminder; only a confirmed payment changes it.

## Tools

Zapier, Google Sheets, Gmail, Google Drive, CSV / Excel.

## Files

| File | Purpose |
|---|---|
| `Prompt.md` | The instructions given to Zapier's agent for the first and second reminder workflows (recipient addresses replaced with placeholders) |
| `screenshots/` | Zap, spreadsheet and email screenshots *(to be added)* |
| `Zapier_Automated_Payment_Reminder_Project_Report.pdf` | Project report |

## Safeguards

- Emails go only to the explicitly authorised test addresses.
- `Reminder Count` is updated only after a successful send.
- Only cell values are changed, never formatting.

Trigger: This workflow is triggered on a scheduled basis (e.g., every day at 9:00 AM) to check for pending payment reminders.

1. Connect to Google Drive and locate the specified Excel sheet containing the payment data :action[Google%20Drive%3A%20Find%20a%20File]{data="%7B%22id%22%3A%22file_v2%22%2C%22label%22%3A%22Google%20Drive%3A%20Find%20a%20File%22%2C%22selectedApi%22%3A%22GoogleDriveCLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"} with columns: Sr, Invoice ID, Type, Name, Amount, Account No, Bank Name, and Status.
2. Read and retrieve all rows from the Excel sheet :action[Google%20Sheets%3A%20Get%20Many%20Spreadsheet%20Rows%20(Advanced)]{data="%7B%22id%22%3A%22get_many_rows%22%2C%22label%22%3A%22Google%20Sheets%3A%20Get%20Many%20Spreadsheet%20Rows%20(Advanced)%22%2C%22selectedApi%22%3A%22GoogleSheetsV2CLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"}

:action[Files%20By%20Zapier%3A%20Line%20Items%20From%20CSV]{data="%7B%22id%22%3A%22e8f4ad80-2ce1-4a35-95d8-a9595c1b7dcb%22%2C%22label%22%3A%22Files%20By%20Zapier%3A%20Line%20Items%20From%20CSV%22%2C%22selectedApi%22%3A%22FilesByZapierCLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"} 
3. Filter the rows where the 'Status' column value is exactly 'Pending'.
4. For each row with a 'Pending' status, extract the following details: Sr, Invoice ID, Type, Name, Amount, Account No, Bank Name.
5. Compose a professional payment reminder email draft for each pending record :action[Gmail%3A%20Create%20Draft]{data="%7B%22id%22%3A%22draft_v2%22%2C%22label%22%3A%22Gmail%3A%20Create%20Draft%22%2C%22selectedApi%22%3A%22GoogleMailV2CLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"} using the following sample template:
- Subject: 'Payment Reminder - Invoice [Invoice ID] Due'
- Body: 'Dear [Name], This is a friendly reminder that your payment for Invoice ID: [Invoice ID] of Amount: [Amount] is currently pending. Please find your banking details below for reference: Account No: [Account No], Bank Name: [Bank Name], Payment Type: [Type].Transaction ID:[Type] Kindly ensure the payment is completed at the earliest to avoid any inconvenience. If you have already made the payment, please disregard this message. Thank you for your cooperation. Regards, 'Payments Team'
6. Send the composed reminder email to the email address associated with each 'Pending' record. email id - test@example.com

send only to those whom i have mentioned the email ids.

:action[Google%20Sheets%3A%20Update%20Spreadsheet%20Row]{data="%7B%22id%22%3A%22f62b3942-4c38-4c59-95c5-bf04a78086df%22%2C%22label%22%3A%22Google%20Sheets%3A%20Update%20Spreadsheet%20Row%22%2C%22selectedApi%22%3A%22GoogleSheetsV2CLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"}

From - test@example.com
7. Log each sent email with the corresponding Invoice ID, Name, Amount, and timestamp for record-keeping.

Conditional Logic:
- If the Status is 'Pending', send the reminder email.
- If the Status is anything other than 'Pending' (e.g., Paid, Completed), skip that row and move to the next.
- If no pending records are found, end the workflow without sending any emails.

Expected Outcome: Every day, all individuals with a 'Pending' payment status in the Google Drive Excel sheet will automatically receive a personalized payment reminder email containing their invoice details and banking information, ensuring timely follow-up on outstanding payments.

After sending the mail automatically check if any reply mail came from them and update the sheet accordingly.

If the user has replied that payment was successful in the mail, :action[Gmail%3A%20Find%20Email]{data="%7B%22id%22%3A%223c71927f-cb65-4ef6-9511-87930e348cb0%22%2C%22label%22%3A%22Gmail%3A%20Find%20Email%22%2C%22selectedApi%22%3A%22GoogleMailV2CLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"} update the google sheet (SCIT Invoice) and update the Status cell. Proceed with just updating the data values without any formatting changes only on the specified cell of that column.

:action[Google%20Sheets%3A%20Update%20Spreadsheet%20Row]{data="%7B%22id%22%3A%22f62b3942-4c38-4c59-95c5-bf04a78086df%22%2C%22label%22%3A%22Google%20Sheets%3A%20Update%20Spreadsheet%20Row%22%2C%22selectedApi%22%3A%22GoogleSheetsV2CLIAPI%22%2C%22copilotInserted%22%3Afalse%2C%22isLoading%22%3Afalse%7D"}



Trigger:

This workflow should execute only after the V0 workflow has already sent the first reminder email and updated the Google Sheet.

The workflow runs once every day at 9:00 AM.

Workflow:

1. Connect to Google Drive and open the Excel file named "SCIT Invoice".

2. Read all rows containing:

- Sr

- Invoice ID

- Type

- Name

- Amount

- Account No

- Bank Name

- Status

- Transaction ID

- Reminder Count

- Escalated

3. Process only rows satisfying ALL of the following:

• Status = Pending

• Reminder Count = 1 (meaning the first reminder has already been sent)

• Transaction ID is not empty

• Escalated is blank or No

4. For every matching row, compose a professional HTML email.

Subject:

SECOND REMINDER – Payment Pending – Invoice [Invoice ID]

Body:

<p style="color:red;font-size:22px;">

<b>SECOND PAYMENT REMINDER</b>

</p>

Dear [Name],

This is a second reminder regarding your pending payment.

Our records indicate that the payment for the following invoice is still <span style="color:red;"><b>PENDING</b></span> even after receiving our previous reminder.

<b>Invoice Details</b>

Invoice ID:

<b>[Invoice ID]</b>

Amount:

<b>₹[Amount]</b>

Transaction ID:

<span style="color:blue;"><b>[Transaction ID]</b></span>

Payment Type:

<b>[Type]</b>

<b>Bank Details</b>

Account Number:

<b>[Account No]</b>

Bank Name:

<b>[Bank Name]</b>

<p style="color:red;">

<b>Your payment has remained pending for more than 24 hours after our first reminder.</b>

</p>

Kindly complete your payment immediately.

<b style="color:red;">

Failure to complete the payment without delay may result in escalation to the concerned authority.

</b>

If payment has already been completed, kindly reply to this email with your payment confirmation.

Regards,

Finance Team

5. Send the email.

6. Update the Google Sheet:

Reminder Count = 2

7. Log:

Invoice ID

Name

Amount

Timestamp

Conditional Logic:

IF

Status = Pending

AND Reminder Count = 1

AND Transaction ID exists

THEN

Send the Second Reminder

Update Reminder Count = 2

Otherwise skip the record.

Expected Outcome:

Only invoices that have already received the first reminder (V0) and have not been marked Completed by V1 will receive the second reminder. After the email is sent, the Reminder Count is updated to 2.

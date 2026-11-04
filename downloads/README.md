# Downloads — EQ Maintenance Manager v3.1

Open a file below, then click **Download raw file** (the down-arrow icon). No pull request is needed.

- [Portable Windows app ZIP](EQ_Maintenance_Manager_V3_1_Portable_Windows_x64.zip)
- [Excel data import template](EQ_Data_Import_Template.xlsx)
- [Illustrated PDF user guide](EQ_Maintenance_Windows_User_Guide.pdf)

## Approval email

Microsoft 365 approval notifications are implemented with Approve and Reject buttons opening an authenticated review page. Configure approver email addresses in User Settings and mail settings under Administration → Email Notifications. Email remains disabled until IT configures HTTPS, approved Microsoft Graph sender permissions and the server-side application secret. No real inbox delivery has been tested against your tenant.

See [Microsoft 365 email setup](EMAIL_NOTIFICATION_SETUP.md). Opening a link does not change status; the assigned user must sign in and confirm.

## Excel upload

The updated app includes **Administration → User Settings → Excel Data Import**, also available from the application header for Administrators.

1. Download the Excel template and fill the worksheets you need. Keep worksheet names and headings unchanged.
2. Choose the completed `.xlsx` workbook in the app and click **Validate workbook**.
3. Correct all reported errors, review the warnings, check the confirmation and click **Import validated records**.

The workbook contains 18 sheets covering users/projects, drivers, equipment, maintenance and fleet records and historical documents. Imports are add-only and all-or-nothing. Duplicate identifiers do not overwrite existing data. Historical approval entries do not execute live workflows, and historical receipts do not repost stock. Imported user accounts remain Inactive until an administrator sets a password and activates them.

## Updating the app

Close the application and shared service before replacing program files. Preserve and back up the host's Data folder. Extract all ZIP files together; do not move only the EXE. All clients must use the same shared service for common multi-user data.

The Windows ZIP was assembled with the official checksum-verified runtime. Desktop workflows were tested using the matching Linux runtime; native Windows execution and 100-user load testing remain rollout checks.

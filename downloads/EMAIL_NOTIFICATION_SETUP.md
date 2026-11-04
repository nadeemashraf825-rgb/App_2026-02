# Microsoft 365 approval email and Approve / Reject buttons

## What is implemented

When a live document is submitted, the shared service queues one email for the assigned current approver in the same SQLite transaction as the submission. When that stage is approved, it queues the next stage. Duplicate saves do not create duplicate stage emails. Historical Excel approvals do not generate notifications.

Each email contains project/document details and **Approve** and **Reject** buttons. Buttons open an HTTPS review page; opening the link makes no decision. The assigned approver signs in with Project, Category, User ID and password, reviews the document, checks confirmation and submits a decision. Rejection requires a comment. Wrong-user/project, self-approval, expired/stale links and repeat decisions are rejected. The same server approval validation applies as in the desktop app. Final approval of a Repair Request creates exactly one Job Card.

## Administrator setup

1. In **User Settings → User master**, enter an email address for each approver. Names/accounts must match the configured approval authority. Email addresses can also be imported using the Users Excel worksheet.
2. Have IT configure HTTPS on the shared service and a company DNS address reachable by the approvers (on the network or VPN). Email review pages use that address. The normal local HTTP-only pilot cannot enable email notifications.
3. Have the Microsoft 365 administrator create an Entra ID application, authorize Microsoft Graph **application Mail.Send** with administrator consent, and restrict mailbox access to the approved sender using the organization's Exchange application access/RBAC controls.
4. Configure the application secret securely on the SHARED SERVER as environment variable **EQ_MAIL_CLIENT_SECRET** before starting its service process. Do not put it in Excel, client-config.json, the app settings UI, source code or chat. Use the organization's secret-management process and rotate it before expiry.
5. Open **Administration → Email Notifications** or the button in **User Settings**. Enter Tenant ID, Application/Client ID, sender mailbox and the service's HTTPS origin. These fields are configuration, not secret values.
6. Save, then enable notifications after the sender permissions, HTTPS and secret are ready. The app checks the prerequisites. With email disabled, messages stay queued and no external mail is sent.
7. The standalone shared service processes pending messages every 30 seconds. **Process pending queue** can also be used by an administrator. Check the delivery queue for recipient/address issues and processing results.

## Delivery states

- **Queued:** awaiting processing; email may still be disabled.
- **Missing recipient:** add/correct the approver's email in User Settings.
- **Sent:** Microsoft Graph accepted the message; this is not a guarantee it reached the recipient's inbox. Check sender/tenant mail tracing if necessary.
- **Used:** that stage was reviewed in the application or through its email link.
- **Expired:** the stage changed or the seven-day link lifetime ended; use the current application approval inbox.
- **Unconfirmed:** a send attempt failed or was interrupted. The service does not automatically retry ambiguous sends, to avoid duplicate emails. Investigate mail tracing before attempting an operational resend.

The service stores link hashes and encrypted token material; no passwords or email action tokens appear in its public delivery list. Keep **mail-token.key** with the SQLite database in secure host backups. Losing the key invalidates existing notification links.

## Validation limits

Graph transport and email decisions were tested using mock mail delivery. No real Microsoft 365 message has been sent from this build environment. A live tenant/sender acceptance test is still required after IT configuration. The 100-user target has not been load tested.

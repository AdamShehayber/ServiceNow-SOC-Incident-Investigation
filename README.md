# ServiceNow SOC Incident Investigation

## Project Overview

This project documents my hands-on SOC analyst investigation in a simulated ServiceNow environment. I investigated a suspicious vendor bank account change request, reviewed email and security evidence, assessed the potential financial impact, and documented my findings in a ServiceNow incident ticket.

**Incident:** CLD-0237 — Suspicious Vendor Bank Detail Change  
**Platform:** ServiceNow (SOC Simulation)  
**Classification:** True Positive — Suspected Business Email Compromise (BEC)  
**Recommended Severity:** High  
**Potential Financial Exposure:** £84,200

## Alert

A finance employee reported an email titled "Updated remittance details for October invoicing." The sender claimed to represent Bexley Payroll Integrations and requested that an outstanding £84,200 invoice be paid to a new bank account.

## Hypothesis

An attacker may be impersonating a legitimate vendor to redirect a payment to a fraudulent account. The attacker may have obtained genuine contract information through a previous compromise.

## Evidence Checked

- Compared the reported email against legitimate vendor correspondence.
- Identified a lookalike sender domain, `bexleypay-finance.example`, instead of `bexleypay.example`.
- Identified that the suspicious domain was only 11 days old.
- Reviewed SPF, DKIM, and DMARC authentication results, which passed but did not establish vendor legitimacy.
- Reviewed the vendor's security notification and available sign-in, helpdesk, and antivirus records.
- Found no confirmed payment transfer or direct account compromise in the reviewed evidence.

## Verdict and Severity

**True Positive — Suspected Business Email Compromise / Payment Redirection Attempt.**

**High severity recommended** because the request targeted a substantial payment. No completed fraudulent transfer was confirmed.

## Recommended Actions

- Escalate the suspicious request to finance and incident response.
- Advise finance not to change payment details until verified.
- Independently confirm bank details with the legitimate vendor.
- Review and potentially block the suspicious sender domain.
- Search for additional related messages and determine whether any payment was made.

## Resolution Notes

Documented the investigation in ServiceNow and recommended escalation, independent vendor verification, and confirmation of payment status. The incident should remain open or pending escalation until containment and payment verification are completed.

## Investigation Evidence

![CLD-0237 ServiceNow Incident Report](CLD-0237-ServiceNow-Incident-Report.png)

## Skills Demonstrated

ServiceNow | SOC Alert Triage | Phishing Analysis | Business Email Compromise | Email Header Analysis | Incident Documentation | Security Log Review

*This project was completed in a simulated SOC training environment.*

## Investigation Evidence

The following screenshots document my hands-on investigation of incident CLD-0237 in a simulated ServiceNow SOC environment. I reviewed the reported alert, compared legitimate and suspicious vendor emails, examined the vendor security notification, and analyzed proxy and sign-in logs to support my verdict.

### Evidence Screenshots

Screenshots are available in the [screenshots folder](./screenshots/).


## Investigation Evidence Screenshots

The following screenshots document the evidence I reviewed during my investigation of CLD-0237 in a simulated SOC environment.

### 1. Legitimate Vendor Email
![Legitimate Bexley correspondence](screenshots/Screenshot%202026-10-19%20144237.png)
I reviewed the legitimate email from Bexley Payroll Integrations to establish a baseline for normal vendor communication. I checked the sender domain, signed statement of work, invoice reference, and revised project schedule. This helped me identify which details in the reported email were genuine and which needed further verification.

### 2. Suspicious Payment Redirection Email
![Suspicious bank account change request](screenshots/Screenshot%202026-10-19%20144259.png)
I examined the reported email requesting that an £84,200 invoice be paid to a new bank account. I identified a lookalike sender domain, bexleypay-finance.example, instead of the legitimate bexleypay.example. Although SPF, DKIM, and DMARC passed, these checks did not prove that the sender was authorized to change the vendor's banking instructions.

### 3. Vendor Security Notification
![Bexley security advisory and indicators](screenshots/Screenshot%202026-10-19%20144342.png)
I reviewed Bexley's security notification describing a confirmed compromise in its own environment. The notification provided suspicious IP addresses, domains, and file hashes for investigation. I used this intelligence to understand the possible threat and identify indicators to check against Cloudora's records.
### 4. Proxy Log Investigation
![Proxy traffic reviewed](screenshots/Screenshot%202026-10-19%20144415.png)
I reviewed outbound proxy traffic for suspicious destinations and unusual activity, including connections involving bexley-sync.example. My goal was to determine whether the known indicators appeared in Cloudora's network activity. The proxy records required correlation with other evidence before any connection to the reported payment-change email could be established.
### 5. Vendor Threat Intelligence
![Vendor compromise indicators](screenshots/Screenshot%202026-10-19%20144455.png)
I examined the indicators supplied in the vendor notification, including infrastructure associated with credential harvesting, authentication attempts, and staging activity. This helped me distinguish known activity in the vendor's compromised environment from activity actually confirmed in Cloudora's environment.
### 6. Week 1 Sign-In Log Review
![Week 1 sign-in records](screenshots/Screenshot%202026-10-19%20144513.png)
I reviewed Week 1 authentication records to establish a baseline and identify unusual sign-ins, unfamiliar IP addresses, failed attempts, and MFA activity. I compared relevant records against the vendor's indicators while avoiding assumptions based on unrelated security events.
### 7. Week 2 Sign-In Log Review
![Week 2 sign-in records](screenshots/Screenshot%202026-10-19%20144534.png)
I reviewed Week 2 sign-in records to check for additional suspicious authentication activity and possible overlap with the reported incident. The records included other security events, so I evaluated whether any evidence directly supported compromise related to CLD-0237. I did not confirm a direct account compromise from the records reviewed.

Investigation Outcome

I classified CLD-0237 as a True Positive — Suspected Business Email Compromise (BEC) / Payment Redirection Attempt, with High recommended severity due to the potential £84,200 exposure.

The available evidence did not confirm that a payment was completed or that Cloudora accounts were compromised. I recommended escalation, independent vendor verification, and confirmation of payment status.

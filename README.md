# Email-Phishing-Investigation-
Investigated an Amazon-themed order-confirmation email using safe, defensive SOC Level 1 techniques.
SOC-PHISH-001 | Email Phishing Investigation (SOC Level 1)
Status: Manual triage documented. Email assessed as likely phishing (high analyst confidence). No confirmed recipient compromise or independently verified PDF malware behavior. Public evidence is sanitized.

Objective
Investigate an Amazon-themed order-confirmation email using safe, defensive SOC Level 1 techniques. The case demonstrates how to correlate mail routing and authentication, social-engineering content, attachment metadata, and provider security controls, while preserving uncertainty.
Tools used
- Mozilla Thunderbird: opened a message and reviewed rendered content and MIME/source headers.
- Google Admin Toolbox Messageheader Analyzer: summarized authentication and delivery route.
- Gmail: provided Spam-folder context and a blocked-attachment warning.
- Ubuntu: investigation workstation. The proposed hashing and Python analyzer were not yet executed.
Case summary
- Case: SOC-PHISH-001
- Subject: Your Recent Transaction Details – Amazon
- Sender: a personal outlook.com mailbox, unrelated to Amazon's branding (full identity redacted)
- Content: purported USD 407.98 purchase confirmation, encouraging a customer-service callback
- Authentication shown by analyzer: SPF none; DMARC fail; ARC pass; DKIM not established
- Attachment metadata: Amazon Order Confirmation.pdf, declared PDF, 138,608 bytes, base64 transfer encoding
- Gmail attachment verdict: Virus found; download disabled
- Analyst assessment: likely Amazon impersonation and callback phishing. No verified phone call, file execution, data loss or infection.
Investigation workflow
1. Collect and protect available email evidence (do not open suspicious links/attachments).
2. Examine sender identity, full headers, authentication and routing.
3. Review message body for impersonation, urgency, payment lures and callback instructions.
4. Record attachment MIME properties separately from independently verified file attributes.
5. Record mail-provider security detections, but do not infer a malware family or infection.
6. Preserve findings, confidence, limitations and proportional recommended actions.
Evidence map
Evidence	Source	What it proves
E-01	Thunderbird message view (private source)	Sender appearance, subject and recipient context
E-02	Redacted Messageheader screenshot	SPF none, ARC pass, DMARC fail
E-03	Redacted Thunderbird body screenshot	Amazon-themed invoice and callback pretext
E-04	Redacted Gmail warning screenshot	Provider flagged attachment and disabled download
E-05	Copied MIME header extract	Sender-supplied PDF name, MIME type, size, timestamps, encoding
E-06 / E-07	Not collected	Original-email hashes, verified payload behavior, user impact


Findings and boundaries
The sender/brand mismatch, unexpected invoice and contact request strongly support a deceptive email assessment. Gmail's block supports attachment risk, but the file was not independently reverse engineered. SPF none is not SPF fail, and ARC pass does not make the message trustworthy. Shared cloud mail infrastructure is not automatically hostile.
Response recommendation
Keep the email quarantined or in Spam, avoid calling the supplied number, and report the email through approved channels. In an enterprise setting, an authorized analyst should check whether other users received similar messages and whether anyone interacted. Escalate the attachment to appropriately isolated specialist analysis only if necessary. Do not bypass Gmail's blocked download.

Proposed next step: Python email triage
Develop and test an offline .eml parser that calculates file hashes, extracts reported authentication results and sender domains, inventories MIME parts, extracts URLs without visiting them, and exports CSV/JSON observations. This automation has not been implemented or validated in this case.
Safe-publication guidance
Do not publish the original email, blocked PDF, full mailbox addresses, message identifiers, or live phone number. Retain complete evidence only in appropriately protected notes. The document is a training case study, not an assertion of real-world compromise.
References
- MITRE ATT&CK Phishing (T1566)
- RFC 9989 — DMARC
- RFC 8617 — ARC
- Gmail Help — Antivirus scanning attachments
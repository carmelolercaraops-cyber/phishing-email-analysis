# SOC Incident Report — Binance Phishing Email

## Incident Overview

### Time of Activity

**August 22, 2022**

Email received at approximately **21:39 UTC**.

The email attempted to convince the recipient that their Binance account information had expired and that withdrawals would remain disabled unless the information was updated within 72 hours.

### Affected Entity

**Recipient:** `p***[@]pot`

**Impersonated Entity:** Binance

---

## Alert Classification

### Verdict

**True Positive**

### Threat Type

**Phishing**

### Severity

**Medium**

### Reason for Classifying as True Positive

The email presents multiple independent indicators consistent with a phishing attempt.

The message impersonates Binance and uses Binance branding, formatting, an account-related pretext, and a time-limited threat to create pressure for the recipient to update account information.

The visible sender identity is:

`Binance <do-not-reply[@]ses[.]binance[.]com>`

However, the message was received from infrastructure associated with:

`smtp2[.]wp-cloud[.]fi [84[.]34[.]166[.]151]`

The envelope sender was:

`wpcloud[@]ilonasavola[.]com`

The Message-ID also contains infrastructure associated with:

`ilonasavola-com[.]staging[.]hel2[.]wp-cloud[.]dev`

This creates an identity and infrastructure mismatch between the declared Binance sender and the infrastructure observed in the message headers.

Email authentication also produced multiple anomalies:

- **SPF:** `none`
- **DKIM:** `none`
- **DMARC:** `fail`
- **DMARC action:** `none`

The DMARC result indicates that the message did not pass DMARC. The `action=none` value indicates that no enforcement action was applied by the receiving system.

The email contains a call to action pointing to:

`hxxps://zzdzw[.]com/`

The domain was classified as **malicious** by AlphaMountain.ai and as a **malicious website** by Forcepoint ThreatSeeker in the reputation checks performed during the investigation.

The combination of Binance brand impersonation, sender and infrastructure inconsistencies, authentication anomalies, social engineering, urgency, and a maliciously classified external destination provides sufficient evidence to classify the message as a **True Positive phishing attempt**.

No evidence was collected during the investigation confirming successful credential submission, account takeover, or other victim impact.

---

## Header Analysis

| Header / Indicator | Observed Value |
|---|---|
| From | `Binance <do-not-reply[@]ses[.]binance[.]com>` |
| Reply-To | `do-not-reply[@]ses[.]binance[.]com>` |
| Return-Path | `wpcloud[@]ilonasavola[.]com` |
| Subject | `[Binаnсе] lmmediate verification required for p***[@]hotmail[.]com` |
| Date | Mon, 22 Aug 2022 21:39:41 +0000 |
| Message-ID | `<a6e2feecb5be84894fdbdba6447a7b10@ilonasavola-com[.]staging[.]hel2[.]wp-cloud[.]dev>` |
| SMTP Host | `smtp2[.]wp-cloud[.]fi` |
| SMTP Source IP | `84[.]34[.]166[.]151` |

The `Received` headers identify the earliest observable external SMTP source as:

`smtp2[.]wp-cloud[.]fi [84[.]34[.]166[.]151]`

The observed transport chain then passes through Microsoft Exchange Online / Exchange Online Protection infrastructure before reaching the recipient mailbox.

The relevant external hop was:

`smtp2[.]wp-cloud[.]fi (84[.]34[.]166[.]151)`

to:

`HE1EUR01FT054[.]mail[.]protection[.]outlook[.]com`

The subsequent hops are internal Microsoft mail infrastructure.

The `Return-Path` and Message-ID contain references consistent with `ilonasavola[.]com` / WP Cloud infrastructure, while the `From` field declares `ses[.]binance[.]com`, creating an identity mismatch that warrants investigation.

The Message-ID hostname:

`ilonasavola-com[.]staging[.]hel2[.]wp-cloud[.]dev`

contains `staging` and WP Cloud infrastructure references. This is an observable characteristic of the message metadata but does not, by itself, establish how the email was generated or identify the attacker.

---

## Email Authentication

### SPF

**Result:** `none`

The receiving system reported:

`spf=none (sender IP is 84[.]34[.]166[.]151)`

for:

`smtp[.]mailfrom=ilonasavola[.]com`

The result does not provide a positive SPF authorization for the observed sender IP.

### DKIM

**Result:** `none`

The message was not signed with DKIM.

The authentication result states:

`dkim=none (message not signed)`

No DKIM signature was available to authenticate the message or provide an aligned domain identity.

### DMARC

**Result:** `fail`

The DMARC result was:

`dmarc=fail action=none header[.]from=ses[.]binance[.]com`

The message did not pass DMARC.

The envelope sender domain was:

`ilonasavola[.]com`

while the visible From domain was:

`ses[.]binance[.]com`

No DKIM signature was available to provide an alternative aligned authentication path.

The receiving system recorded `action=none`, meaning that no enforcement action was applied based on the DMARC result.

---

## Social Engineering Analysis

The message employs several social engineering techniques:

- **Brand impersonation:** Binance
- **Pretext:** account information has expired
- **Account threat:** withdrawals are presented as disabled
- **Urgency:** recipient is given 72 hours to act
- **Consequence:** account is threatened with permanent disablement
- **Call to Action:** `UPDATE INFORMATIONS`
- **Recipient targeting:** the recipient address is included in the message
- **Trust indicators:** Binance branding, formatting, images, footer, and copyright text

The subject contains visually deceptive characters:

`[Binаnсе]`

The apparent word "Binance" uses Cyrillic homoglyphs in place of some Latin characters.

The footer also contains a visually deceptive form of the Binance name:

`Вinаncе Team`

The same technique is present in:

`© 2017 - 2022 Вinаncе All Rights Reserved`

The message also contains language anomalies, including:

- `your information has been expired`
- `your informations`
- `your account will get disabled permanently`

These linguistic anomalies are consistent with an attempt to create urgency despite poor language quality.

The objective is to create a credible account-security scenario and pressure the recipient into updating account information through the provided external link.

---

## Domain Analysis

### Domain

`zzdzw[.]com`

The domain is used as the destination of the email's primary call to action:

`hxxps://zzdzw[.]com/`

The domain was classified as **malicious** by AlphaMountain.ai and as a **malicious website** by Forcepoint ThreatSeeker during the reputation checks performed in the investigation.

Current DNS resolution identified:

`zzdzw[.]com → 38[.]38[.]132[.]135`

The resolved IP was associated with:

**ASN:** `54600`

**ASN Name:** `PEG-SV - PEG TECH INC`

The domain uses the following nameservers:

- `a[.]share-dns[.]com`
- `b[.]share-dns[.]net`

The nameservers resolve through Cloudflare infrastructure. The presence of Cloudflare nameservers does not establish that Cloudflare is the hosting provider for the phishing content.

No MX or TXT records were observed in the DNS data collected during the investigation.

Current WHOIS data reported:

**Creation Date:** `2026-05-25T18:00:27Z`

The analyzed email is dated **August 22, 2022**, creating a discrepancy between the email timestamp and the current WHOIS creation date.

Historical WHOIS or DNS data was not established using the free sources available during the investigation. Therefore, the current WHOIS creation date does not by itself establish when the domain was originally used in connection with the analyzed email.

---

## URL Analysis

### Observed URL

`hxxps://zzdzw[.]com/`

The URL is implemented as the CTA behind the visible text:

`UPDATE INFORMATIONS`

The destination domain received malicious classifications from multiple threat-intelligence sources consulted during the investigation.

The URL was **not directly opened** during the investigation.

The domain currently resolves to:

`38[.]38[.]132[.]135`

The resolved IP did not present a negative reputation in the source consulted.

Because the URL was not directly accessed, no final landing-page content, redirect chain, credential-harvesting page, or other payload was directly observed.

The malicious classifications of the domain provide supporting evidence for the phishing assessment, while the investigation does not independently establish the exact content that would have been presented to the recipient at the time of the email.

---

## IOC / Attack Indicators

### Email / Domain Indicators

- `do-not-reply[@]ses[.]binance[.]com`
- `ilonasavola[.]com`
- `ilonasavola-com[.]staging[.]hel2[.]wp-cloud[.]dev`
- `zzdzw[.]com`
- `smtp2[.]wp-cloud[.]fi`

### Network Indicator

- `84[.]34[.]166[.]151` — SMTP source IP
- `38[.]38[.]132[.]135` — current DNS resolution for `zzdzw[.]com`

### URL Indicator

- `hxxps://zzdzw[.]com/`

### Social Engineering Indicators

- `[Binаnсе]` — subject containing Cyrillic homoglyphs
- `Вinаncе Team` — footer containing Cyrillic homoglyphs
- `UPDATE INFORMATIONS` — primary CTA
- `72 hours` — time-limited threat
- `account will get disabled permanently` — consequence used to create urgency

---

## Escalation Decision

**Escalation: Recommended**

The event should be escalated to determine whether the recipient interacted with the phishing message.

No evidence was collected during the investigation confirming or excluding user interaction with the CTA.

Available authentication, proxy, DNS, web, or endpoint telemetry should therefore be correlated to determine whether the URL was accessed and whether any subsequent suspicious activity occurred.

The recipient should also be contacted, where appropriate, to confirm whether the link was clicked or whether credentials or account information were entered.

---

## Recommended Remediation Actions

### Immediate Actions

1. Confirm whether the recipient interacted with the email or the embedded URL.
2. Review available authentication and endpoint logs for activity following receipt of the message.
3. Monitor the affected account for anomalous authentication activity.
4. If credential submission or suspicious authentication is confirmed, initiate the organization's account-compromise response procedure and reset credentials as appropriate.

### Preventive Controls

1. Verify that SPF, DKIM, and DMARC are correctly implemented and enforced for organizational domains.
2. Maintain email security controls capable of detecting brand impersonation, authentication anomalies, and suspicious external destinations.
3. Maintain outbound web filtering and monitoring controls to detect access to suspicious or malicious destinations.
4. Continue monitoring for additional messages using the same sender domain, infrastructure, or URL indicators.
5. Add confirmed phishing indicators to appropriate email security and detection controls.

---

## Final Assessment

The analyzed message is classified as a **True Positive — Phishing**, with **Medium severity**.

The classification is supported by the combination of Binance brand impersonation, sender and infrastructure inconsistencies, SPF/DKIM/DMARC authentication anomalies, social-engineering techniques, urgency and account-disablement threats, linguistic anomalies, Cyrillic homoglyph usage, and a maliciously classified external destination.

The current evidence supports classification of the message as a **Binance impersonation phishing attempt**.

There is currently **no confirmed evidence of successful credential submission, account takeover, or other victim impact**. Further log correlation and recipient verification are therefore required to determine the actual impact.

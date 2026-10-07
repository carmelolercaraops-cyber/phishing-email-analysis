# SOC Incident Report — Microsoft Phishing Email

## Incident Overview

### Time of Activity

**February 6, 2026**

Email received at approximately **17:14 UTC**.

The email reported a supposed suspicious sign-in to a Microsoft account from Russia at **04:32 UTC** on February 6, 2026.

### Affected Entity

**Recipient:** `j***[@]gmail[.]com`

**Impersonated Entity:** Microsoft / Microsoft Account Team

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

The message impersonates the Microsoft Account Team and uses Microsoft-related branding, formatting, and a security-themed pretext to create a credible scenario involving an alleged unauthorized sign-in from Russia. The user is instructed to review the recent account activity and secure the account if the activity is not recognized.

The sender identity is inconsistent with legitimate Microsoft infrastructure. The message was sent from:

`noreply[@]microsoftonline-verify[.]com`

The SMTP connection identifies the sending host as:

`mail[.]microsoftonline-verify[.]com`

associated with:

`vps-291847[.]contabo[.]net [178[.]238[.]225[.]91]`

The domain `microsoftonline-verify[.]com` contains a Microsoft reference but does not correspond to an official Microsoft domain. LevelBlue classified the domain as **suspicious** in VirusTotal. WHOIS/MS lookup performed during the investigation returned no match.

Email authentication also produced multiple failures:

- **SPF:** `fail`
- **DKIM:** `fail`
- **DMARC:** `fail`
- **DMARC policy:** `p=NONE`
- **Disposition:** `dis=NONE`

The DMARC failure did not prevent delivery because the published policy was configured for monitoring rather than enforcement.

The CTA in the email points to:

`hxxps://bit[.]ly/3vF9xKz`

The URL uses the legitimate Bitly URL-shortening service, but in this context the shortened URL obscures the effective destination of the link. No evidence was collected during the investigation confirming the final destination or user interaction with the link.

The combination of social engineering, brand impersonation, anomalous sender infrastructure, authentication failures, suspicious domain characteristics, and a shortened URL provides sufficient evidence to classify the message as a **True Positive phishing attempt**.

---

## Header Analysis

| Header / Indicator | Observed Value |
|---|---|
| From | `Microsoft Account Team <noreply[@]microsoftonline-verify[.]com>` |
| Return-Path | `noreply[@]microsoftonline-verify[.]com` |
| Subject | `[Action Required] Unusual sign-in activity on your account` |
| Date | Thu, 06 Feb 2026 17:14:18 +0000 |
| Message-ID | `<8c4f2e91.a03b7d44.1f2e8a@microsoftonline-verify[.]com>` |
| SMTP Host | `mail[.]microsoftonline-verify[.]com` |
| Infrastructure Hostname | `vps-291847[.]contabo[.]net` |
| SMTP Source IP | `178[.]238[.]225[.]91` |

The source IP `178[.]238[.]225[.]91` did not present detections in the VirusTotal check performed during the investigation. The absence of detections does not establish that the infrastructure is legitimate.

---

## Email Authentication

### SPF

**Result:** `fail`

The sending IP `178[.]238[.]225[.]91` was not authorized by the SPF policy associated with `microsoftonline-verify[.]com`.

### DKIM

**Result:** `fail`

DKIM verification failed for the message.

### DMARC

**Result:** `fail`

The DMARC result was:

`dmarc=fail (p=NONE dis=NONE)`

The message was nevertheless delivered because the domain's DMARC policy was configured with `p=NONE`, and no quarantine or rejection disposition was applied.

---

## Social Engineering Analysis

The message employs several social engineering techniques:

- **Brand impersonation:** Microsoft / Microsoft Account Team
- **Pretext:** alleged unauthorized account access
- **Geographic trigger:** suspicious login reportedly originating from Russia
- **Urgency:** immediate action is encouraged to secure the account
- **Call to Action:** `Review recent activity`
- **Trust indicators:** Microsoft-related branding, visual formatting, images, and email footer

The objective is to create a credible account-security scenario and encourage the recipient to interact with the provided link.

---

## Domain Analysis

### Domain

`microsoftonline-verify[.]com`

The domain contains the Microsoft brand reference but does not correspond to an official Microsoft domain.

VirusTotal classified the domain as **suspicious** through the LevelBlue vendor.

WHOIS/MS lookup performed during the investigation returned no match.

The domain is used in both the sender identity and the SMTP hostname:

- `noreply[@]microsoftonline-verify[.]com`
- `mail[.]microsoftonline-verify[.]com`

These elements are consistent with a possible **brand impersonation** attempt.

---

## URL Analysis

### Observed URL

`hxxps://bit[.]ly/3vF9xKz`

The URL is implemented as the CTA behind the visible text:

`Review recent activity`

Bitly is a legitimate URL-shortening service. In this case, the shortened URL obscures the effective destination before the recipient interacts with it.

During analysis, the URL returned an HTTP **404** response. No confirmed final redirect destination was established.

Therefore, the investigation does not establish that the URL itself was malicious based solely on the observed response.

---

## IOC / Attack Indicators

### Email / Domain Indicators

- `noreply[@]microsoftonline-verify[.]com`
- `microsoftonline-verify[.]com`
- `mail[.]microsoftonline-verify[.]com`
- `vps-291847[.]contabo[.]net`

### Network Indicator

- `178[.]238[.]225[.]91` — SMTP source IP

### URL Indicator

- `hxxps://bit[.]ly/3vF9xKz`

### Social Engineering Indicator

- `91[.]234[.]99[.]42` — IP presented in the email body as the alleged source of the suspicious login

The IP `91[.]234[.]99[.]42` is part of the email's social-engineering pretext and was not identified as the SMTP infrastructure. VirusTotal did not report detections for this IP during the investigation.

---

## Escalation Decision

**Escalation: Recommended**

The event should be escalated to determine whether the recipient interacted with the phishing message.

No evidence was collected during the investigation confirming or excluding user interaction with the CTA. Available authentication, proxy, DNS, web, or endpoint telemetry should therefore be correlated to determine whether the URL was accessed and whether any subsequent suspicious activity occurred.

The recipient should also be contacted, where appropriate, to confirm whether the link was clicked or credentials were entered.

---

## Recommended Remediation Actions

### Immediate Actions

1. Confirm whether the recipient interacted with the email or the embedded URL.
2. Review available authentication and endpoint logs for activity following receipt of the message.
3. Monitor the affected account for anomalous authentication activity.
4. If credential submission or suspicious authentication is confirmed, initiate the organization's account-compromise response procedure and reset credentials as appropriate.

### Preventive Controls

1. Verify that SPF, DKIM, and DMARC are correctly implemented and enforced for organizational domains.
2. Consider strengthening DMARC policy from monitoring to enforcement where operationally appropriate.
3. Maintain outbound web filtering and monitoring controls to detect suspicious external destinations.
4. Continue monitoring for additional messages using the same sender domain, infrastructure, or URL indicators.
5. Add confirmed phishing indicators to appropriate email security and detection controls.

---

## Final Assessment

The analyzed message is classified as a **True Positive — Phishing**, with **Medium severity**.

The classification is supported by the combination of Microsoft brand impersonation, social-engineering techniques, anomalous sender infrastructure, SPF/DKIM/DMARC authentication failures, suspicious sender-domain characteristics, and the use of a shortened URL intended to drive user interaction.

There is currently **no confirmed evidence of user interaction or account compromise**. Further log correlation and user verification are therefore required to determine the actual impact.

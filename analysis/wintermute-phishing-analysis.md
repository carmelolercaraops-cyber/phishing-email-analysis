# SOC Incident Report — Wintermute Phishing Email

## Incident Overview

### Time of Activity

15 February 2025, 09:00 UTC

### Affected Entity

Scallop.io — Business Development Group

## Alert Classification

### Verdict

True Positive

### Threat Type

Spear Phishing / Business Email Compromise-style Social Engineering

### Severity

Medium

### Reason for Classifying as True Positive

The email presents itself as a business development communication from Wintermute Trading and proposes a strategic partnership with Scallop.io. The message claims to continue a previous interaction with a colleague and attempts to establish credibility through impersonation of a named business development employee.

The message contains multiple identity and authentication inconsistencies. The `From` address claims to originate from `wintermute.com`, while the envelope sender and SPF-authenticated domain are associated with `scallop.io`. DMARC therefore fails because the authenticated sending domain is not aligned with the `Header.From` domain.

The email subsequently attempts to move communication to a dedicated Telegram group, introducing an external communication channel outside the established email workflow.

These characteristics provide sufficient evidence to classify the message as a targeted spear-phishing attempt.

## Header Analysis

The message contains seven `Received` headers.

The earliest observed connection shows:

`77[.]37[.]35[.]56 → 100[.]97[.]28[.]83`

followed by processing through MailChannels infrastructure and subsequent delivery through Google's mail infrastructure.

The headers also show:

`100[.]97[.]28[.]83 → relay[.]mailchannels[.]net`

Although these entries correlate through the same internal IP, the headers alone do not establish a fully continuous causal chain between the two connections.

The message was processed by MailChannels and subsequently by Google's mail infrastructure before delivery to the Scallop.io Business Development Group.

The header:

`Authenticated sender: hostingershared`

indicates authentication to MailChannels using the associated sender/account identifier. This does not independently establish the identity or legitimacy of the sender.

## Email Authentication

### SPF

SPF passed because `scallop[.]io` authorizes `209[.]85[.]220[.]69` as a permitted sender.

The SPF-authenticated domain was:

`scallop[.]io`

### DKIM

No DKIM result is present in the observed `Authentication-Results` header.

No DKIM signature was observed that could provide an aligned authentication result for `wintermute[.]com`.

### DMARC

DMARC failed because the SPF-authenticated domain `scallop[.]io` is not aligned with the `Header.From` domain `wintermute[.]com`.

The published DMARC policy was:

`p=NONE`

Therefore, the receiving infrastructure was not instructed to quarantine or reject the message based on the DMARC failure.

## Social Engineering Analysis

The message uses a targeted business-development pretext rather than a generic phishing lure.

The sender claims:

> “I am Louis from Wintermute Trading”

and identifies himself as:

> “Business Development Officer of Wintermute Trading”

The email further claims that the recipient had previously interacted with the sender's colleague and shared their email address. This establishes a false sense of continuity and familiarity.

The message then proposes a strategic partnership involving Scallop.io, digital-asset listings and liquidity solutions. This creates a plausible B2B context intended to establish trust and legitimacy.

The message also uses Wintermute branding and presents a named employee, company website and social-media profiles as additional trust signals.

The communication is subsequently moved to a dedicated Telegram group:

`hxxps://t[.]me/+Jn3tuBa5uX5jNDAx`

This introduces an external communication channel outside the original email conversation.

The observed social-engineering techniques include:

- Brand impersonation
- Identity impersonation
- Pretexting
- Business relationship fabrication
- Authority and credibility cues
- Targeted B2B context
- External-channel redirection

There is no direct evidence in the analyzed email of credential theft, payment requests, malware delivery or data exfiltration. Any such objective remains a hypothesis and is not treated as confirmed activity.

## Domain Analysis

### Domain

The message references multiple domains associated with different identities:

- `wintermute[.]com` — declared sender identity
- `wintermute[.]business` — Message-ID domain and Google Groups discussion reference
- `scallop[.]io` — recipient, envelope sender and SPF-authenticated domain
- `srv1862[.]main-hosting[.]eu` — infrastructure associated with the authenticated sender

The presence of multiple domains does not independently prove malicious activity, but the discrepancy between the declared sender identity and the authenticated sending domain is significant when combined with the social-engineering indicators observed in the body.

## URL Analysis

### Observed URL

`hxxps://t[.]me/+Jn3tuBa5uX5jNDAx`

The URL points to a Telegram group presented as a dedicated communication channel for the alleged partnership.

The domain `t[.]me` is legitimate Telegram infrastructure. The presence of the URL alone does not demonstrate that the destination was malicious.

No evidence was established in the analyzed email confirming the final purpose or content of the Telegram group.

Additional URLs observed in the email include:

- `hxxp://wintermute[.]com`
- `hxxps://t[.]me/[REDACTED]`
- `hxxps://iili[.]io/2DJcBMF[.]jpg`
- `hxxps://img[.]icons8[.]com/ios-filled/50/000000/telegram-app[.]png`
- `hxxps://img[.]icons8[.]com/ios-filled/50/000000/linkedin[.]png`
- `hxxps://upbit[.]com`

These URLs were presented as company, social-media, image or footer resources and were not independently treated as malicious indicators based solely on their presence.

## IOC / Attack Indicators

### Email / Domain Indicators

- `partnerships[@]wintermute[.]com`
- `partnerships[@]wintermute[.]business`
- `u********[@]srv1862[.]main-hosting[.]eu`
- `scallop[.]io`
- `wintermute[.]business`
- `srv1862[.]main-hosting[.]eu`

### Network Indicator

- `77[.]37[.]35[.]56`
- `23[.]83[.]222[.]67`
- `209[.]85[.]220[.]69`

### URL Indicator

- `hxxps://t[.]me/+Jn3tuBa5uX5jNDAx`

### Social Engineering Indicator

- False business-development pretext
- Claimed previous contact with a colleague
- Impersonation of Wintermute Trading personnel
- Fabricated partnership context
- Use of corporate branding and professional identity
- Redirection to a dedicated Telegram communication channel

## Escalation Decision

Escalation is **Recommended**.

The email should be treated as a targeted spear-phishing attempt due to the combination of identity impersonation, business relationship pretexting, authentication/alignment inconsistencies and redirection to an external communication channel.

If the recipient interacted with the Telegram group, further investigation should be performed to determine whether additional social-engineering activity, credential harvesting, financial requests or other malicious activity occurred.

No evidence from the analyzed email alone confirms successful user interaction or compromise.

## Recommended Remediation Actions

### Immediate Actions

- Quarantine the identified email if still present in the environment.
- Block or monitor the observed Telegram invitation URL according to organizational policy.
- Search mail logs for additional messages using the same sender, domains or Message-ID patterns.
- Search for other messages containing the observed Telegram invitation.
- Confirm whether any recipient interacted with the external communication channel.
- If interaction occurred, escalate for further investigation.

### Preventive Controls

- Strengthen email authentication enforcement for DMARC where operationally appropriate.
- Monitor for impersonation of corporate brands and employees.
- Alert on authentication/alignment anomalies involving external business communications.
- Provide awareness training for targeted B2B social-engineering scenarios.
- Establish procedures for independently verifying unexpected partnership or financial communications through known corporate channels.

## Final Assessment

The analyzed message is assessed as a **True Positive spear-phishing attempt** targeting the Scallop.io Business Development Group.

The attack relies primarily on social engineering rather than an obvious malicious attachment or credential-harvesting page. The sender constructs a plausible business relationship, impersonates a Wintermute Trading employee and attempts to establish an external communication channel through Telegram.

The technical evidence supports the assessment through inconsistencies between the claimed sender identity and the authenticated sending infrastructure, including an SPF-authenticated `scallop[.]io` domain combined with a `wintermute[.]com` `Header.From` address resulting in DMARC failure.

There is currently no confirmed evidence of credential theft, financial fraud, malware execution or account compromise. Such outcomes remain unconfirmed hypotheses requiring additional evidence.

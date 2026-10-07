# README.md

# Phishing Email Analysis Lab

A SOC-focused portfolio project for analyzing real-world phishing email samples (`.eml`), investigating email headers and authentication, identifying social-engineering techniques, extracting and contextualizing indicators, and producing structured incident reports.

The project is designed to demonstrate a practical **SOC L1 phishing-triage workflow**, with emphasis on evidence-based analysis and clear escalation decisions.

## Project Objectives

- Analyze raw `.eml` files and email headers
- Trace observed mail infrastructure through `Received` headers
- Evaluate SPF, DKIM, and DMARC results
- Identify sender identity and authentication inconsistencies
- Analyze URLs, domains, IP addresses, and email addresses
- Identify phishing and social-engineering techniques
- Distinguish technical evidence from assessment and hypotheses
- Document findings in structured SOC incident reports
- Define escalation and remediation actions

## Skills Demonstrated

### SOC / Blue Team

- Phishing email triage
- Email header analysis
- SMTP infrastructure analysis
- SPF / DKIM / DMARC analysis
- IOC identification and contextualization
- Social-engineering analysis
- Threat-intelligence enrichment
- Evidence-based incident reporting
- Escalation decision-making
- Detection and response recommendations

### Analytical Approach

The analysis focuses on answering three questions:

1. **What can be established from the available evidence?**
2. **What does that evidence indicate?**
3. **What remains a hypothesis requiring further investigation?**

This distinction is maintained throughout the incident reports to avoid presenting assumptions as confirmed facts.

## Repository Structure

    phishing-email-analysis/
    ├── README.md
    ├── LICENSE
    ├── .gitignore
    │
    ├── samples/
    │   ├── microsoft-phishing.eml
    │   ├── binance-phishing.eml
    │   └── wintermute-phishing.eml
    │
    └── analysis/
        ├── microsoft-phishing-analysis.md
        ├── binance-phishing-analysis.md
        └── wintermute-phishing-analysis.md

## Analyzed Samples

| Sample | Analysis Focus |
|---|---|
| Microsoft phishing | Sender impersonation, SPF/DKIM/DMARC failure, suspicious domain and shortened URL |
| Binance phishing | Sender/domain mismatch, DMARC failure, malicious URL and urgency-based social engineering |
| Wintermute phishing | Targeted B2B pretexting, SPF/DMARC analysis, identity inconsistencies and Telegram redirection |

The samples are analyzed as independent incidents. Findings are based on the evidence available in each original email and on external enrichment performed during the investigation.

## Analysis Methodology

For each email sample, the investigation follows a consistent workflow:

    Raw .eml
       |
       v
    Header Analysis
       |
       +--> From / Reply-To / Return-Path
       |
       +--> Received Chain
       |
       +--> Message-ID
       |
       +--> SPF / DKIM / DMARC
       |
       v
    Content Analysis
       |
       +--> Social Engineering
       +--> Sender Identity
       +--> URLs
       +--> Domains
       +--> Other Indicators
       |
       v
    Threat Intelligence Enrichment
       |
       v
    Evidence Assessment
       |
       +--> Confirmed Evidence
       +--> Analyst Assessment
       +--> Unconfirmed Hypotheses
       |
       v
    SOC Incident Report
       |
       +--> Verdict
       +--> Severity
       +--> Escalation Decision
       +--> Remediation

## Header Analysis

The investigation considers the main fields relevant to phishing triage:

- `From`
- `Reply-To`
- `Return-Path`
- `Received`
- `Message-ID`
- `Authentication-Results`
- `Received-SPF`
- Other relevant transport or mail-processing headers

The `Received` chain is analyzed from newest to oldest to reconstruct the observed mail flow and identify relevant infrastructure.

Authentication results are interpreted in context rather than treated as standalone verdicts. For example, an SPF pass does not establish that the visible sender identity is legitimate if the authenticated domain is not aligned with the `Header.From` domain.

## Email Authentication

Each sample is evaluated for:

- **SPF** — whether the sending IP is authorized by the envelope sender domain
- **DKIM** — whether the message contains a valid cryptographic signature
- **DMARC** — whether the authenticated identity aligns with the visible `From` domain and what policy is applied

Authentication failures are treated as evidence that contributes to the overall assessment rather than as automatic proof of phishing.

## Social Engineering Analysis

The investigation also examines the psychological and contextual techniques used by the message.

Examples include:

- Brand impersonation
- Sender identity impersonation
- Urgency
- Threats of account restriction
- Credential-verification pretexts
- Fabricated business relationships
- Authority and credibility cues
- External-channel redirection
- Targeted B2B communication

The objective is not simply to determine whether an email "looks suspicious", but to explain **how the message attempts to influence the recipient's decision-making**.

## IOC Analysis

Indicators are not treated as a simple data dump.

Each indicator is evaluated in context and classified according to its role in the observed activity.

Examples include:

- Sender and recipient email addresses
- Domains
- IP addresses
- URLs
- Hosting infrastructure
- Message identifiers

Where appropriate, legitimate infrastructure is explicitly distinguished from suspicious or malicious indicators. For example, a legitimate service such as Telegram, Microsoft infrastructure, Google infrastructure, or Cloudflare does not automatically become an IOC simply because it appears in a phishing message.

## Threat Intelligence Enrichment

External sources may be used to enrich observed indicators and validate the investigation.

The enrichment process can include:

- Domain reputation
- URL reputation
- IP reputation
- WHOIS / registration information
- DNS records
- Hosting / ASN information
- Historical observations where available

External enrichment is treated as supporting evidence. It does not replace analysis of the original email.

## Incident Reports

Each analyzed sample has a dedicated SOC incident report under `analysis/`.

The reports document:

- Incident overview
- Alert classification
- Header analysis
- Email authentication
- Social-engineering analysis
- Domain analysis
- URL analysis
- IOC / attack indicators
- Escalation decision
- Recommended remediation actions
- Final assessment

The reports are written to reflect the type of documentation expected from a SOC investigation rather than simply providing a list of extracted indicators.

## Safety and Analysis Boundaries

The project is designed to investigate phishing emails without requiring interaction with potentially malicious infrastructure.

The analysis prioritizes:

- Raw email evidence
- Passive inspection
- Header analysis
- Controlled threat-intelligence enrichment
- Reputation and DNS information
- Documentation of observed URLs and domains

Potentially malicious URLs are not opened solely for the purpose of reproducing a phishing page or interacting with unknown infrastructure.

Where the available evidence does not establish the final payload, landing page, credential-harvesting mechanism, or victim interaction, the report explicitly states that the activity remains unconfirmed.

## Investigation Questions

A SOC analyst could continue the investigation by asking:

1. Did any recipient click the observed URL?
2. Did a recipient submit credentials?
3. Did an endpoint communicate with the identified infrastructure?
4. Did the same email reach additional users?
5. Are similar messages present in the mail environment?
6. Are the observed domains or IPs present in DNS, proxy, firewall, or EDR telemetry?
7. Did authentication anomalies occur after the email was delivered?
8. Did the recipient interact with an external communication channel?

These questions define the transition from initial phishing triage to deeper incident investigation.

## Limitations

This project focuses on **email-level SOC triage and investigation**.

It does not attempt to provide:

- Malware reverse engineering
- Dynamic malware analysis
- Endpoint forensics
- Full campaign attribution
- Complete infrastructure takeover analysis
- Confirmed victim-impact assessment without supporting telemetry

When evidence is insufficient, the reports explicitly distinguish confirmed observations from analyst assessment and hypotheses.

## Portfolio Takeaway

This project demonstrates a repeatable SOC L1 workflow:

> **Triage → Evidence → Authentication Analysis → Social Engineering → IOC Contextualization → Assessment → Escalation → Remediation**

The objective is not simply to identify that an email is phishing.

The objective is to demonstrate **how a SOC analyst reaches that conclusion, what evidence supports it, what remains unknown, and what should happen next.**


# .gitignore

.venv/
__pycache__/
*.py[cod]
.DS_Store
*.log
.env
.idea/
.vscode/

# Local analysis artifacts
*.tmp
*.bak

# India Bank ROI Case Study: Forensic Evidence Intelligence Agent

> **Purpose and evidence standard:** This is an illustrative business case for a large Indian bank, built from the scenario assumptions in this document. It is not a measured customer result, vendor quotation, or independently verified industry benchmark. Each bank should validate alert volumes, loaded staffing costs, investigation time, software and integration costs, and realized case outcomes during a controlled pilot. All amounts are in INR.

## Executive summary

A large Indian bank receives a high volume of suspicious email and digital-fraud alerts while maintaining a small specialist security team. Manual first-line investigation—reviewing message headers, extracting links, checking reputation, correlating evidence, and documenting next steps—can consume scarce analyst time. The Forensic Evidence Intelligence Agent (FEIA) is intended to automate supported evidence triage, follow leads found in the evidence, correlate findings, and prepare a reviewable response report.

Under the scenario below, annual investigation cost falls from **₹55–60 lakh** to approximately **₹20 lakh**, producing **₹35–40 lakh in annual gross recurring savings** if staffing can be reduced, avoided, or redeployed as assumed. With an estimated **₹5–6 lakh upfront investment**, simple payback is approximately **1.5–2.1 months** after steady-state savings begin. First-year net value after setup is **₹29–35 lakh**, and the corresponding first-year net ROI is approximately **483–700%** under the stated cost boundaries.

These results are conditional. The expected benefit is primarily analyst capacity and reduced backlog, not an automatic reduction in fraud losses. Processing 800–1,200 daily alerts in 2–3 seconds per item is not established by the software’s current benchmark evidence. The bank must measure end-to-end throughput, accuracy, review requirements, API latency, and the number of alerts that require human handling before adopting the headline savings.

## Organization: typical large Indian bank

This representative institution is assumed to have **1,000+ employees**, **200–500 branches across India**, and **₹500+ crore in daily transaction volume**. It operates a dedicated security team of **five analysts** and detects **200–300 confirmed cyber incidents annually**. The incident count is distinct from alert volume: hundreds of thousands of daily alerts may include duplicates, benign messages, and low-risk events, while only a smaller share becomes a confirmed incident.

The bank’s phishing and suspicious-message queue is assumed to receive **800–1,200 alerts per day**. These can include fake KYC notices, bank impersonation, UPI collect-request lures, fraudulent customer-care links, malicious attachments, and supplier-payment diversion attempts. This is an illustrative workload, not a sector-wide average. The agent’s supported evidence types—email, URL, and browser-history artifacts—cover only part of a bank’s broader fraud and cyber operations.

## Current state without the agent

Analysts manually gather and interpret evidence across mail gateways, email headers, URL reputation services, browser history, endpoint records, and case-management notes. The working assumptions for this case are:

| Current-state measure | Scenario assumption |
|---|---:|
| Phishing alerts received | 800–1,200 per day |
| Analyst time per investigation | 30–45 minutes |
| Fully investigated alerts per analyst per day | 1–2 |
| Additional partially reviewed alerts per analyst per day | 3–4 |
| Investigation backlog | 100–150 cases at a time |
| Analyst time spent pursuing false positives | 15–20% |
| Security analyst cost | ₹8 lakh/person/year |
| Five-analyst annual personnel cost | ₹40 lakh |
| Additional backlog investigation cost | ₹15–20 lakh/year |
| **Total annual investigation cost** | **₹55–60 lakh** |

The operational pain is broader than this direct cost. Repetitive low-resolution triage can contribute to analyst burnout and false-positive fatigue. A delay in reviewing a malicious link can extend the time before the bank searches for other recipients, checks sign-in activity, or blocks a confirmed indicator. Hiring a good analyst may require months of onboarding and supervised case experience; the six-month onboarding figure is a planning assumption to validate against the bank’s own hiring history. Five people also provide limited resilience during leave, incident surges, and specialist turnover.

The ₹15–20 lakh backlog line is treated here as **additional external, overtime, or otherwise incremental investigation spend**. It must not duplicate analyst payroll already counted in the ₹40 lakh. If the bank cannot demonstrate that it is genuinely incremental, exclude it from the baseline and recalculate savings.

## Proposed deployment and cost

The deployment comprises one hardened agent server, integration with selected evidence sources and case workflows, and a monthly software license. A production implementation should also account for security review, access controls, log retention, backup, vulnerability management, operational support, and model evaluation.

| Deployment cost | Scenario allowance |
|---|---:|
| Agent server, one-time | ₹2–3 lakh |
| Integration and implementation, one-time | ₹1–2 lakh |
| Security hardening, validation, and contingency, one-time | Included in total allowance |
| **Total setup investment** | **₹5–6 lakh** |
| Software license | ₹30,000/month = ₹3.6 lakh/year |
| Maintenance and operating overhead | Approximately ₹0.4 lakh/year |
| **Annual license plus overhead** | **Approximately ₹4 lakh** |

The component ranges add up to ₹3–5 lakh before the security-hardening and contingency allowance; therefore ₹5–6 lakh is a rounded project envelope, not the sum of only the server and integration line items. Obtain written quotes and confirm whether license, support, hosting, and upgrades are included.

## Operating model with FEIA

The case assumes the alert stream remains **800–1,200 per day**. FEIA can automatically analyze supported evidence and may complete an individual machine triage in **2–3 seconds** under suitable conditions. A planning throughput of **500–1,000 alerts per day** is an operational target, not a demonstrated guarantee for a live bank environment. At the low end of capacity and high end of alert volume, **up to 700 alerts/day remain outside that stated automated capacity**. Those alerts need deduplication, upstream filtering, alternate automation, or human triage; the backlog cannot be assumed to clear same day until the bank tests the full queue.

The proposed staffing model retains **two analysts** for high-risk review, quality assurance, incident coordination, and actions requiring approval. Their annual cost is **2 × ₹8 lakh = ₹16 lakh**. FEIA’s annual license and overhead are estimated at **₹4 lakh**, for an annual run cost of **₹20 lakh**. A target false-positive rate of **2–5%** is a hypothesis to validate on a bank-specific, representative dataset. It must be measured alongside precision, recall, false-negative rate, and the share of decisions reviewed by analysts; a lower false-positive rate is not useful if genuine threats are missed.

The agent should not autonomously disable accounts, purge messages, or block infrastructure without authorized approval. The expected workflow is automated evidence collection and prioritization, followed by human validation for consequential containment.

## ROI calculation

### Annual recurring savings

| Calculation | Low case | High case |
|---|---:|---:|
| Current annual investigation cost | ₹55 lakh | ₹60 lakh |
| Annual operating cost with agent | ₹20 lakh | ₹20 lakh |
| **Annual gross recurring savings** | **₹35 lakh** | **₹40 lakh** |
| Less one-time setup | ₹6 lakh | ₹5 lakh |
| **Year 1 net value after setup** | **₹29 lakh** | **₹35 lakh** |

### Payback and ROI

- **Payback period:** Setup divided by monthly gross savings. Monthly gross savings are approximately ₹2.92–3.33 lakh (₹35–40 lakh ÷ 12). A ₹5–6 lakh setup therefore yields approximately **1.5–2.1 months** simple payback, assuming the bank realizes steady-state savings promptly.
- **First-year net ROI:** `(Year 1 gross savings − setup investment) ÷ setup investment`. At the conservative end, `(₹35 lakh − ₹6 lakh) ÷ ₹6 lakh ≈ 483%`; at the optimistic end, `(₹40 lakh − ₹5 lakh) ÷ ₹5 lakh = 700%`. This excludes financing cost, taxes, internal change effort, and unpriced risks.
- **Three-year cumulative net value:** At constant annual savings and operating costs, `(3 × ₹35–40 lakh) − ₹5–6 lakh` gives approximately **₹99–115 lakh** before discounting and any additional costs. This is close to, but not always above, ₹100 lakh at the conservative end.

These calculations assume the reduction from five analysts to two creates a real budget saving or allows the bank to avoid planned hires. If the three analysts remain on payroll and are simply reassigned, the benefit is **capacity released**, not immediate cash savings. Report those separately.

## Additional benefits: material but not booked as hard ROI

- **Lower burnout and attrition risk:** Less repetitive queue work may improve job quality and retention. A possible ₹10–15 lakh avoided retraining or replacement cost is not included in the ROI; it requires validated turnover and hiring data.
- **Reduced exposure window:** Earlier triage may help investigators identify a campaign or a compromised mailbox sooner. The avoided loss could be substantial, but no ₹1–10 crore breach-avoidance benefit is booked without a defensible counterfactual and incident evidence.
- **Improved compliance readiness:** Consistent logs, preserved findings, and documented approvals can support internal audit and incident-response processes. They do not guarantee regulatory compliance or eliminate fines.
- **Higher-value security work:** Analysts can spend more time on threat hunting, control tuning, confirmed fraud, and complex investigations.
- **Customer trust:** Faster, well-governed response to UPI, KYC, and bank-impersonation campaigns can support customer confidence, although brand value is difficult to attribute to a single tool.
- **Law-enforcement handoffs:** Structured evidence summaries may improve escalation quality where the bank shares information with authorized investigators, subject to legal and evidentiary requirements.

## Scalability: use workload, not branch count alone

A 500-branch institution does not automatically save ₹50 lakh annually, nor does a 1,000-branch institution automatically save ₹80 lakh. Branch count alone does not determine phishing-alert volume, analyst workload, or the number of investigations that the agent can safely handle. The representative case yields **₹35–40 lakh per bank per year** only under its stated staffing and incremental backlog-cost assumptions. Larger savings require a separate model using measured cases, adoption costs, and staffing decisions for that bank.

For a consortium of **10 banks**, multiplying the representative gross saving by ten gives **₹350–400 lakh (₹3.5–4 crore) annually** before consortium setup, shared operations, tenant isolation, integration, support, and adoption costs. This is a full-participation scenario—not a forecast—and each member’s data and approval controls must remain appropriately separated.

## Adoption timeline

| Period | Activity and exit criteria |
|---|---|
| Weeks 1–2 | Provision and harden the server; agree data access, retention, audit, and approval controls; connect a limited evidence source. |
| Week 3 | Pilot with one team using historical and live, appropriately controlled cases; measure latency, precision, recall, false positives, and analyst review effort. |
| Weeks 4–8 | Expand in stages after security and operational review; train staff; integrate case workflows; retain rollback and manual triage paths. |
| Month 3 onward | Operate only at validated capacity; monitor model drift, API availability, backlog, cost per reviewed case, and human override rates. |

The proposed schedule is an implementation target, not a guarantee. Production rollout may take longer due to procurement, privacy, security testing, core-system integration, and regulatory review.

## Competitive advantage and Bharat context

Earlier adoption can give a bank a **response-speed advantage** if the pilot proves that the agent handles more relevant evidence without increasing missed threats. The stated **50–100× case-throughput ambition** should be tested against the same evidence mix and human-review policy; it must not be inferred from a seconds-per-item benchmark alone. A defensible advantage comes from improving customer protection and investigative consistency—not from claiming perfect detection.

For Indian banks, this is also a domestic capability opportunity: support the **Make in India** cybersecurity ecosystem, reduce avoidable dependence on imported tools where a locally operated option meets requirements, and protect Indian citizens’ financial data with auditable controls. Banks may also explore carefully governed service partnerships that extend triage to SME suppliers and customers. Any such model needs consent, security boundaries, data protection, and clear incident ownership.

## Decision and pilot scorecard

Approve a bounded pilot only after confirming a bank-specific cost baseline and agreeing measurable success criteria. Track daily eligible alerts, automated coverage, median and 95th-percentile time to a human-reviewed disposition, precision, recall, false-positive and false-negative rates, backlog age, confirmed incidents identified, analyst hours released, total cost per reviewed case, and approval/override rates. Compare results with a control or historical baseline; document excluded cases and service failures.

**Bottom line:** Under the stated scenario, FEIA could produce **₹35–40 lakh in gross recurring annual savings** and **₹29–35 lakh in first-year net value after setup**, while improving analyst capacity. These are plausible case-study outputs only if the bank verifies the alert mix, staffing economics, incremental backlog cost, end-to-end throughput, and detection quality. No avoided fraud, regulatory saving, or customer-loss reduction is included as booked ROI.

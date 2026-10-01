# Forensic Engine: Investigation Workflow

## Overview

Forensic Engine is a digital-forensics triage agent for suspicious email, URLs, and browser-history evidence. It does more than run a fixed checklist: it identifies the evidence modality, selects an initial tool plan, examines intermediate findings, and can dispatch follow-up analysis when a tool reveals a new lead. It then correlates results, calculates a transparent risk assessment, and prepares an auditable report and response recommendations. High-risk containment actions remain subject to human approval.

The workflow below describes the investigation from evidence intake to a reviewed response. Sample findings and confidence values are illustrative: a real investigation must report what its evidence and configured integrations actually return. An unknown or unavailable result must not be treated as a clean bill of health.

## The eight-step investigation

### Step 1: Evidence Intake

**Description:** An investigator submits a suspicious artifact, such as an RFC 822 email (`.eml`), a URL, plain-text alert content, or supported browser-history data. The intake should retain the original artifact, record the source and acquisition context, and associate it with an investigation identifier. Evidence handling should preserve an original copy and document any transformations so that later review can distinguish source evidence from extracted indicators.

**Example input:** A phishing email in `.eml` format, including its headers, MIME body, and embedded links.

**Agent action:** The request is checked and routed by evidence type. For email, the parser extracts standard headers, `Received` hops, authentication-result text, and readable body content, while attempting to skip attachments during body extraction. Unsupported or malformed material should be identified for review rather than silently treated as safe. Organizations should apply their own access controls, retention policy, privacy rules, and chain-of-custody procedures at upload.

### Step 2: Classification & Analysis

**Description:** The agent classifies the submitted material and selects the initial analysis path. Email evidence goes to email forensics; a direct URL goes to URL analysis and reputation enrichment; browser-history evidence goes to browser forensics. The result may be described in an investigator-friendly triage label.

**Example classification:** “Email evidence, partial; suspected phishing; high priority.” In a broader forensic queue, a compatible label could be “Fingerprint evidence, partial, high priority,” but fingerprint evidence is not a supported input type in this digital-email workflow.

**Agent reasoning:** The planner uses the request type and content cues to infer the modality and choose one or more starting tools. Classification is a routing decision, not a verdict. The investigation log should make clear which tools were selected and why; low-confidence or incomplete inputs should be escalated for analyst review.

### Step 3: Email Forensics (if applicable)

**Description:** The email tool parses RFC 822 headers and message content, examines sender identity consistency, reads reported SPF and DKIM results, extracts embedded URLs, and evaluates phishing-related content and model features. Header fields such as `From`, `Return-Path`, `Received`, `Authentication-Results`, `Subject`, and `Message-ID` can help reconstruct the message path and preserve pivot points for subsequent investigation.

**Example output:** “SPF: FAIL in the supplied authentication results; DKIM: FAIL in the supplied authentication results; visible sender and return path differ; urgent account-verification language detected.”

**Confidence:** A report may show, for example, “99% confidence” only if a specific calibrated model or evidence rule produced and justifies that value. It must not be assigned as a universal confidence for SPF failure. In this implementation, SPF/DKIM are read from authentication-result headers included in the message; this is not a fresh DNS-based verification of the sender’s domain. A failure is a strong indicator to investigate, not by itself proof of who authored a message.

### Step 4: Autonomous Lead Follow-Up

**Description:** The agent reacts to a finding instead of stopping after the initial tool run. If email analysis extracts embedded URLs, the investigation loop can automatically send those URLs to the URL forensics tool. A suspicious URL discovered during browser-history analysis can similarly trigger URL examination.

**Example:** EmailTool finds a link to `https://secure-example.invalid/verify`; the agent records the discovery as the trigger, dispatches URLTool without waiting for the user to copy and resubmit it, and attaches the child result to the same investigation.

**Why this is autonomous:** The follow-up tool is selected from an intermediate observation made during execution. The system records the trigger and reason in its agent-action trace. This differs from a static program that always runs every tool in a fixed order regardless of what the evidence contains. Autonomy here means bounded investigation planning and tool orchestration; it does not mean that the agent independently decides guilt or executes destructive response actions.

### Step 5: URL / Database Analysis

**Description:** URL analysis extracts structural features (such as direct-IP hosting, suspicious TLDs, credential-related tokens, transport security, and character entropy), applies the configured classifier, and obtains reputation information when the integration is available. Browser-history forensics can identify visited URIs and access context, then submit suspicious candidates for deeper URL analysis.

**Example output:** “Suspicious URL: direct IP or risky TLD observed; credential-related path tokens present; URL model probability elevated; reputation source reports detections.” A result might be summarized as “Malicious URL — registered 2 days ago, `.top` TLD” only if an actual registration-age lookup ran and returned that registration date. The current URL feature extraction skips WHOIS, so registration age must not be invented; a risky TLD or model score alone does not establish when a domain was registered.

**Risk score:** A URL model might return, for example, an 87% phishing probability. This is the model's score for that artifact—not the final incident risk score, not an 87% certainty of criminal intent, and not a replacement for reputation or analyst review. If an API key is unavailable or a service returns an error, the report should identify that limitation; a mock or unavailable reputation result is not a verified clean lookup.

### Step 6: Evidence Correlation

**Description:** The correlator combines evidence items from the email, URL, reputation, and browser-history stages. It looks for compound patterns and preserves links between each conclusion and its supporting sources. This is where independent indicators can become a coherent attack narrative.

**Example:** “Display-name impersonation + SPF/DKIM failure in received results + credential-harvesting URL features + reputation detections = suspected compound phishing campaign.”

**MITRE ATT&CK mapping:** A message delivering a malicious link may map to **T1566.002 – Spearphishing Link**. Mapping is a classification aid, not proof that every listed behavior occurred. The report should indicate which observations support the mapping and distinguish directly observed evidence from inference.

### Step 7: Risk Assessment

**Description:** The agent produces a case-level score from 0–100 and a severity of `LOW`, `MEDIUM`, `HIGH`, or `CRITICAL`. The assessment combines the highest and average model probabilities with severity-weighted evidence and correlation findings. In the current implementation, a score of 80 or more is `CRITICAL`, 60–79.9 is `HIGH`, 35–59.9 is `MEDIUM`, and below 35 is `LOW`, with critical/high evidence flags also affecting the outcome.

**Example output:** “Risk Score: 94/100; Severity: CRITICAL; confidence: 94%.” The report should explain the drivers—for instance, multiple high-severity authentication and sender-mismatch findings, a high URL score, and corroborating reputation evidence—rather than present the number alone. Confidence in the agent’s assessment is distinct from the risk score. `HIGH` and `CRITICAL` outcomes require human review before destructive containment.

### Step 8: Recommendation & Report

**Description:** The agent generates an auditable report containing the investigation ID, input type, tools used, evidence items, findings, correlations, risk rationale, and recommended next actions. A response playbook may propose blocking a malicious domain or IP, searching and purging copies of the email, reviewing proxy or endpoint logs, and revoking sessions or resetting exposed credentials.

**Example recommendations:** “Add confirmed malicious indicators to approved DNS/proxy controls; search mailboxes using the message ID and purge confirmed copies; review sign-in logs; revoke active sessions and reset credentials if exposure is confirmed; check mail-forwarding rules.”

**Human approval:** Blocking, tenant-wide purge, account suspension, credential reset, and other disruptive steps require approval by the authorized operator. Recommendations are not execution receipts. The report should identify the owner, approval state, and any follow-up evidence needed before action.

## Workflow diagram

```text
Email Input (.eml)
        |
        v
  Agent Planner
        |
        v
   EmailTool ------------------------------+
        |                                  |
        | extracts embedded URL            |
        +---- autonomous discovery --------+
                                           v
                                      URLTool
                                           |
                                           v
                                  ReputationTool
                                           |
 Browser/other evidence ------------------+
        |                                  |
        +---------------------> Correlation
                                      |
                                      v
                                  RiskScore
                                      |
                                      v
                                   Playbook
                                      |
                                      v
                              Human review/approval
                                      |
                                      v
                          Auditable investigation output
```

## Complete example: suspected Indian bank impersonation

**Input:** An employee submits a `.eml` message claiming to be from the “State Bank Security Desk.” It says the recipient’s UPI access will be suspended unless identity details are verified immediately. The visible `From` address uses a bank-like display name, the technical return path belongs to an unrelated domain, the supplied authentication results show SPF failure, and the message contains a link to a lookalike verification page. This is a fictional training example, not a claim about a real bank or incident.

**Investigation trace:**

1. The intake records an email artifact and assigns an investigation ID. The original message is retained according to the organization’s evidence policy.
2. The planner classifies the input as email evidence and schedules `EmailForensicsTool`.
3. Email analysis extracts the sender, return path, message ID, received hops, authentication-result text, urgency cues, and embedded link. It reports the observed SPF failure and sender mismatch, clearly attributing authentication status to the supplied headers.
4. Discovery of the verification link automatically triggers `URLForensicsTool`; the investigator does not have to resubmit it.
5. URL analysis identifies suspicious structure, such as a credential-related path and an unusual TLD, and records the classifier result. Reputation lookup is recorded as positive, negative, unavailable, or unscanned according to the actual integration response. No domain-age claim is made unless an age lookup supplied it.
6. The correlator links the sender mismatch, authentication failure, urgency, and URL indicators into a suspected spearphishing-link narrative, with a possible ATT&CK mapping to T1566.002.
7. **Illustrative output:** `Risk Score: 94/100 | Severity: CRITICAL | Human review required: Yes`. The score rationale cites the observed and inferred signals and their sources.
8. The generated playbook recommends validating the message ID, searching for other recipients, quarantining or purging confirmed copies, checking whether anyone visited the link, reviewing sign-in and forwarding-rule activity, and blocking confirmed indicators. An authorized human reviewer approves disruptive actions.

**Illustrative report excerpt:**

```text
Case: INV-EXAMPLE
Input: RFC 822 email (.eml)
Findings:
  - SPF failure reported in supplied Authentication-Results header
  - Visible sender identity differs from Return-Path
  - Urgent UPI/account-verification language detected
  - Embedded lookalike URL analyzed automatically
  - Reputation: [actual integration result required]
Correlation: Suspected spearphishing link (MITRE ATT&CK T1566.002)
Risk: 94/100 — CRITICAL (illustrative)
Human review: Required before containment
Recommended actions: Search message ID; assess link clicks and sign-ins;
review forwarding rules; block confirmed indicators; remediate exposed accounts.
```

**Timeline:** “45 minutes (manual) → 2.1 seconds (with agent) = 99.9% faster” is an illustrative comparison for automated triage processing, not a measured end-to-end incident-resolution guarantee. A 45-minute manual baseline compared with 2.1 seconds is about 99.92% less elapsed processing time for that bounded task. Evidence acquisition, external API latency, analyst review, containment approval, and remediation are additional time and should be measured separately.

## Performance comparison

| Metric | Current | With Agent | Improvement |
|---|---:|---:|---:|
| Analysis time | 25–45 min | 1.8–3.2 sec | ~99.9% faster for automated triage processing* |
| Cases/month | 8–12 | 500–1,000 | ~50–100x target capacity; depends on paired endpoints* |
| Accuracy | 70% | 95%+ | +25 percentage points at the 70%/95% comparison* |

These are planning and demonstration figures, not independently validated production benchmarks. The elapsed-time improvement excludes evidence collection, network lookup delays, human review, and incident response. Capacity depends on case complexity, staffing, integrations, and required review. Accuracy should be measured on a representative, independently adjudicated dataset using precision, recall, false-positive rate, and confidence intervals; “+25%” in the table means percentage points, not relative percentage improvement. Pilot reporting should define each metric, include failed and inconclusive cases, and compare the same workload and cost boundaries.

## Why this is agentic—not merely programmatic

A static pipeline runs the same sequence for every input, whether or not a step is relevant. Forensic Engine starts by inferring the evidence type, plans an initial tool set, observes tool outputs, and changes its next action when it discovers a lead—for example, dispatching URL analysis because an email contains a link. Its execution trace records tool choice, trigger, duration, findings, and downstream correlation, making the investigative path reviewable. The agent’s autonomy is bounded: it orchestrates analysis and proposes actions, while human operators retain authority over consequential containment and remediation.

The central value is therefore not simply a fast classifier. It is a traceable investigation loop that can move from artifact to extracted indicator to corroborating evidence and a reviewable response plan—while preserving the distinction between observed facts, model-generated assessments, external-service results, and human decisions.

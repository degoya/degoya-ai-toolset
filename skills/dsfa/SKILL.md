---
name: dsfa
description: Determines whether a data protection impact assessment (DPIA, German "DSFA", GDPR Art. 35) is required and drafts a structured DPIA for a feature, product or AI function: description of processing, necessity, risks to data subjects, measures, residual risk. Use for "do we need a DPIA?", "data protection impact assessment", "DSFA", "privacy risk assessment", "threshold analysis".
---

# Data protection impact assessment (DPIA / DSFA)

The result is a **draft** to be agreed with the data protection officer (DPO). Responsibility stays with the controller. For general GDPR duties see skill `eu-legal-compliance`.

## Step 1: Threshold analysis (is a DPIA required?)
A DPIA is mandatory where processing is likely to result in a high risk (Art. 35(1)). Mandatory cases in Art. 35(3) and indicators from the supervisory guidelines (WP 248 rev.01, endorsed by the EDPB; two or more criteria usually mean a DPIA):
- [ ] Evaluation/scoring, profiling
- [ ] Automated decision-making with legal or similarly significant effect
- [ ] Systematic monitoring
- [ ] Sensitive data (Art. 9/10: health, biometrics, religion …) or highly personal data
- [ ] Large-scale processing
- [ ] Matching or combining datasets
- [ ] Data of vulnerable persons (children, employees, patients)
- [ ] Innovative technology (e.g. AI, biometrics)
- [ ] Processing prevents data subjects from exercising a right or using a service/contract

Also check the **DPIA list** of the competent supervisory authority (Germany: DSK list and state lists). Document the result with reasons, even when no DPIA is required.

## Step 2: DPIA draft (Art. 35(7))
1. **Systematic description:** purpose, legal basis, data categories, data subjects, recipients/processors, third countries, retention, data flow (diagram), systems used.
2. **Necessity and proportionality:** legitimate purpose? Data minimisation? Less intrusive means? Transparency and data subject rights implemented?
3. **Risk assessment** from the data subjects' perspective (not the company's): per risk likelihood × severity (negligible / limited / significant / maximum).
   Typical risks: unauthorised access, algorithmic discrimination, identity theft, reputational damage, loss of control over data, function creep, wrong decisions.
4. **Mitigating measures** (technical and organisational, Art. 25/32): pseudonymisation, encryption, role and permission concept, retention periods, human review, transparency, training. Per measure: which risk it reduces.
5. **Residual risk** after measures. If it remains high: prior consultation with the supervisory authority (Art. 36).
6. **DPO opinion** and, where appropriate, views of data subjects (Art. 35(9)), sign-off, review date.

## Output format
```
## DPIA: <processing>
Date: <date> · Controller: … · DPO: …
### 1. Threshold analysis – result: DPIA required yes/no (reasons)
### 2. Description of processing
### 3. Necessity and proportionality
### 4. Risks
| # | Risk to data subjects | Likelihood | Severity | Risk level |
### 5. Measures
| # | Measure | Risk addressed | Status | Owner |
### 6. Residual risk and result
### 7. Open points for the DPO
```

## Rules
- Invent nothing; list missing information as open points.
- For AI systems also classify under the AI Act (skill `eu-legal-compliance`, ai-labelling.md).
- A DPIA is a process: plan a review whenever the processing changes.
- Write the output in the user's language (German authorities expect German documentation).

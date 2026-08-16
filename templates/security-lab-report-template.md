# Security Laboratory Report Template

> Copy this file into your submission directory and replace every instruction inside angle brackets.
> Delete instructional text before submitting. Do not invent evidence, timestamps, impact, or certainty.

---

# <Assessment or Investigation Title>

## Document control

| Field | Value |
|---|---|
| Student / analyst | <name> |
| Report ID | <for example, LAB-NET-001> |
| Version | <1.0> |
| Date | <YYYY-MM-DD> |
| Classification | <Training / Confidential / other teacher-approved label> |
| Laboratory | <laboratory name> |

## 1. Executive summary

<Write this last. In 100–200 words, explain what was assessed, why it matters, the most important
result, and the recommended next action. Avoid command output and unnecessary jargon. A manager
should understand this section without reading the technical details.>

### Overall result

<Pass, partial, failed, exposed, restricted, inconclusive, or another result appropriate to the lab.>

### Finding summary

| ID | Finding title | Severity | Status |
|---|---|---|---|
| F-001 | <short factual title> | <Informational–Critical> | <Open / Mitigated / Accepted / Not applicable> |

## 2. Scope and authorization

### In scope

- <hosts, IP addresses, files, processes, interfaces, or scripts examined>
- <allowed ports, actions, and time window>

### Out of scope

- <networks, hosts, accounts, or actions that were explicitly excluded>

### Rules of engagement and safety

- <isolation method, authorization, privilege limits, snapshots, stop conditions>
- <actions deliberately not performed and why>

## 3. Environment

| Asset ID | Role | Hostname | Address / path | OS or version | Notes |
|---|---|---|---|---|---|
| A-001 | <server/client/file/script> | <hostname if relevant> | <IP/path> | <version> | <purpose> |

<Tool versions that materially affect results should be recorded here. Do not fill the report with
versions that have no relevance.>

## 4. Objectives

1. <measurable objective>
2. <measurable objective>
3. <measurable objective>

## 5. Methodology

<Describe the phases followed and why. Example phases include discovery, collection, validation,
analysis, control implementation, and retesting. State how you reduced false positives and preserved
original evidence.>

### Tools used

| Tool | Purpose | Important options / version |
|---|---|---|
| <tool> | <why it was used> | <only relevant details> |

## 6. Evidence register

Assign an ID to every important artifact. Store evidence under `evidence/` using clear filenames.

| Evidence ID | Description | Source | Collection time (UTC) | SHA-256 / integrity note |
|---|---|---|---|---|
| E-001 | <what the artifact proves> | <host/file/command> | <YYYY-MM-DDTHH:MM:SSZ> | <hash or reason hashing is not useful> |

Rules:

- Refer to evidence IDs from findings and observations.
- Include only relevant output; raw output can go in an appendix or evidence file.
- A screenshot must show enough context to identify the source and must have a caption.
- Never edit raw evidence silently. Derivatives must be identified as derivatives.
- Redact secrets and personal data; document that redaction occurred.

## 7. Findings

Create one subsection per security-relevant finding. Not every observation is a vulnerability.

### F-001 — <Finding title>

| Field | Value |
|---|---|
| Severity | <Informational / Low / Medium / High / Critical> |
| Affected asset(s) | <asset IDs> |
| Status | <Open / Mitigated / Accepted / Not applicable> |
| Related evidence | <evidence IDs> |

**Description**

<Explain the condition clearly and factually.>

**Evidence**

<Quote or summarize the minimum evidence needed to prove the condition. Refer to evidence IDs.>

**Security impact**

<Explain what could realistically happen. Do not inflate impact merely because a tool colored a line red.>

**Likelihood and assumptions**

<Describe required access, preconditions, uncertainty, and possible false positives.>

**Recommendation**

<Give a specific, proportionate action. Include verification guidance where useful.>

**Retest result**

<After a control or fix, record the test performed, expected result, actual result, UTC time, and
supporting evidence. Use `Not retested` when appropriate.>

## 8. Technical observations

Use this section for relevant facts that do not justify a formal finding.

### O-001 — <Observation title>

- **What was observed:** <fact>
- **Why it matters:** <interpretation>
- **Evidence:** <evidence IDs>
- **Uncertainty:** <limitations or alternative explanations>

## 9. Activity timeline

| Time (UTC) | Action | Asset | Result | Evidence ID |
|---|---|---|---|---|
| <time> | <authorized action> | <asset ID> | <result> | <ID> |

## 10. Conclusions and recommendations

### Conclusion

<State whether the objectives were met and what the evidence supports. Separate confirmed facts from
inference.>

### Prioritized recommendations

| Priority | Recommendation | Owner | Verification |
|---:|---|---|---|
| 1 | <specific action> | <student/administrator/teacher> | <how to prove completion> |

## 11. Limitations

- <missing visibility, unavailable privileges, restricted scope, tool limitations, time limits>
- <anything that prevents a stronger conclusion>

An inconclusive result is acceptable when explained. A confident fictional result is not.

## Appendix A — Commands and selected output

```text
$ <command>
<selected output>
```

For every command, state:

- where it was run;
- why it was run;
- what the output means;
- whether it changed the system.

## Appendix B — Report quality checklist

- [ ] Executive summary is understandable without command output.
- [ ] Scope and out-of-scope boundaries are explicit.
- [ ] Assets have stable IDs.
- [ ] Important evidence has IDs and collection times in UTC.
- [ ] Claims refer to evidence.
- [ ] Facts, interpretations, and assumptions are distinguishable.
- [ ] Severity matches realistic impact and likelihood.
- [ ] Recommendations are specific and testable.
- [ ] Retesting distinguishes expected from actual results.
- [ ] Secrets and personal information are absent or redacted.
- [ ] Raw output is selected for relevance rather than pasted indiscriminately.
- [ ] Limitations and uncertainty are documented.
- [ ] Spelling, filenames, headings, and timestamps are consistent.

# Cybersecurity materiality determination record

A working pattern for the contemporaneous record of how an organization decided whether a cybersecurity incident was material, opinionated about the separation between facts and judgment that makes the record defensible a year later.

This is a process template, not legal advice. Materiality is a legal determination made with counsel, and the conclusions in any real record belong to the disclosure decision-makers rather than to the security function. What this document describes is the artifact that captures the decision, which is the part security is usually asked to produce and usually has no format for.

The worked detail is written against US securities disclosure, because that regime has the sharpest published expectations about documenting the process and because its two-clock structure is the part organizations most often get wrong. The structure is not specific to it. An organization outside that regime, or a private one, still needs a contemporaneous record of a significance judgment made under time pressure with incomplete facts, and every section below survives the substitution: replace the standard, the decision-makers, and the deadlines with the ones that bind you. Do that substitution explicitly rather than by analogy, because the thresholds and the identity of the decision-maker are exactly what changes.

## What this record is, and what it is not

This is the internal memorandum documenting a materiality determination for a specific cybersecurity incident: the facts known and unknown at the time, the factors weighed, who participated, what was concluded, and when.

It is not the disclosure filing, not the [postmortem](blameless-postmortem-template.md), and not the incident timeline. The postmortem asks why this happened and what we change. This record asks whether a reasonable investor would consider it important, which is a different question with a different audience, a different clock, and a different owner. Keeping them in one document produces a memo that is too technical for the disclosure committee and too legally hedged to drive engineering change.

The reason this artifact exists in its own right is that the record is what gets examined. Regulators reviewing a determination look at the process, who was involved, and the basis for the conclusion, all as they stood at the time. A conclusion reached correctly but documented afterward from memory is materially weaker than the same conclusion written down as it was made.

## Who owns the determination

Security does not determine materiality. This is the single most important structural point in the document and it is worth stating in the policy that governs the process.

| Role                                    | Responsibility                                                                                                             |
| --------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| Incident commander                      | Establishes and maintains the facts. Distinguishes confirmed from suspected. Flags every material change in understanding. |
| Security leadership                     | Translates technical findings into business consequence. Owns nothing in the conclusion.                                   |
| Disclosure committee, or its equivalent | Makes the determination. Typically finance, legal, and a senior operating executive.                                       |
| Legal counsel                           | Owns the materiality standard, the privilege posture, and the interaction with outside counsel.                            |
| Financial reporting                     | Quantifies known and reasonably likely financial impact, and owns the comparison to established materiality thresholds.    |
| Executive sponsor                       | Convenes the committee and signs the record.                                                                               |

An organization without a standing disclosure committee should name the participants in advance rather than assembling them during an incident. The composition is not an incident-time decision, and assembling it under pressure is how a determination slips past "without unreasonable delay".

## The two clocks, and the one everyone gets wrong

For an SEC registrant, the obligation is to file within four business days of determining that an incident is material, and to make that determination without unreasonable delay after discovery. Those are two separate clocks and only the second one starts automatically.

The common and consequential error is to treat discovery as the start of a four-business-day window. It is not. The four days run from determination. What constrains the period between discovery and determination is the reasonableness standard, and a determination deferred while the organization is capable of making it is exactly the pattern that attracts scrutiny. Record the discovery timestamp, record every point at which the committee convened, and record the reason for any gap. A documented reason for a delay is a defensible delay; an undocumented one is a gap in the record.

The clock is also not the only clock. Most organizations sit under several regimes at once with materially shorter deadlines, and a team that optimizes its incident process around the securities-disclosure timeline will miss a faster one. List every applicable clock in the record header so the determination happens with the full picture visible:

```text
Applicable notification clocks for this incident
  Securities disclosure    Determination without unreasonable delay; file within 4 business days of it
  Data protection          Regulator notification, jurisdiction-dependent, commonly 72 hours from awareness
  State breach laws        Per-state, triggered by resident count and data type
  Sector regulator         As applicable to the industry and the affected system
  EU network security      Early warning obligations measured in hours, where in scope
  Contractual              Customer contracts, commonly 24 to 72 hours; check the top ten by revenue
```

Populate this from the actual obligations, not from a template. The value is in noticing during the incident that a contractual 24-hour clock expired while the committee was still assembling.

## Header

```text
Record ID:              MAT-2026-03
Related incident ID:    SEC-2026-Q1-014
Incident discovered:    2026-03-04 14:22 UTC
Committee convened:     2026-03-05 13:00 UTC
Determination made:     2026-03-06 16:30 UTC
Determination:          Not material / Material
Version:                3 (supersedes v2 of 2026-03-05; see reassessment log)
Participants:           <names and roles>
Counsel:                <internal and external>
Privilege posture:      <as directed by counsel>
Distribution:           <restricted list>
```

Version the record and never overwrite it. Reassessment is normal and expected; a record that shows one version and a clean conclusion is less credible than one that shows the determination changing as facts arrived, because the second is what actually happens.

## The facts ledger

Keep facts and judgment in separate sections. This is the discipline that makes the record hold up, and it is the one most internally written memos skip.

Every fact carries a status and an as-of time. Suspected facts are not promoted to confirmed without a named source.

| Fact                                                                              | Status                    | As of (UTC)      | Source                                                  |
| --------------------------------------------------------------------------------- | ------------------------- | ---------------- | ------------------------------------------------------- |
| Unauthorized access to the AccountView Classic admin role via credential stuffing | Confirmed                 | 2026-03-04 18:00 | Authentication logs, forensic review                    |
| Customer records exposed to the session                                           | Confirmed, 3,180 accounts | 2026-03-05 22:00 | Query audit log reconstruction                          |
| Bulk export of the customer table                                                 | Ruled out                 | 2026-03-05 22:00 | Egress and database audit logs, complete for the window |
| Persistence beyond the session                                                    | Unknown                   | 2026-03-06 09:00 | Forensics in progress, expected 2026-03-09              |
| Billing system integrity affected                                                 | Ruled out                 | 2026-03-05 16:00 | Reconciliation against prior close                      |

The unknowns matter as much as the knowns. A determination made with named open questions and a stated basis for proceeding is defensible. A determination that presents only what was known reads, in hindsight, as though the open questions were never asked.

## Factors weighed

Address both dimensions explicitly and in writing. A determination that rests only on the dollar figure is the most common weak record, because a quantitatively small incident can be qualitatively material and the memo needs to show that possibility was considered rather than skipped.

**Quantitative.** Direct response and remediation cost. Business interruption and lost revenue. Expected legal, regulatory, and notification cost. Insurance recovery, stated separately rather than netted, because the gross figure is the one that gets compared to the threshold. Reasonably likely future impact, not only impact already incurred. Compare to the established quantitative materiality thresholds used in financial reporting, and record the comparison rather than asserting the conclusion.

**Qualitative.** Nature and scope of the data involved. Whether the incident touched a system central to the business model or to a stated competitive strength. Reputational and customer-trust consequence, including the concentration question of whether the affected customers are a small number of large relationships. Regulatory exposure and the likelihood of an investigation. Litigation exposure. Whether the incident reveals a control weakness that calls other representations into question, which is the qualitative factor most often underweighted and the one most likely to matter later. Whether management or the board would want to know, which is a rough but useful proxy.

**Aggregation.** Related occurrences are considered together rather than individually. A series of intrusions by the same actor, or repeated exploitation of the same weakness, can be material in the aggregate while no single event is. State explicitly whether aggregation was considered and what was aggregated. This paragraph is short in most records and its absence is conspicuous.

## The conclusion

Write the conclusion as a reasoned paragraph, not as a checkbox. It states what was determined, the two or three factors that drove it, the open questions that remain, and what would change the answer.

> Based on the facts confirmed as of 2026-03-06 16:30 UTC, the committee determined the incident is not material. The determination rests primarily on the confirmed scope of 3,180 affected accounts against a customer base of approximately 200,000, the exclusion of bulk exfiltration on the basis of complete egress logging for the window, the absence of any effect on billing integrity or service availability, and estimated total cost below the quantitative threshold applied in financial reporting. The committee weighed the qualitative factor that the affected system is a customer-facing system of record and that the access path was covered by an active security exception, and concluded that neither alters the determination at this scope. Forensic confirmation that no persistence mechanism was established is outstanding and expected 2026-03-09. A finding of persistence, or any expansion of confirmed record count beyond approximately 25,000, triggers immediate reassessment.

The last sentence is the one that matters most and the one most often missing. Naming the reassessment triggers in advance converts a later change of conclusion from a reversal into the process working as designed.

## Reassessment

Materiality is determined on the facts available and facts change. Log each reassessment rather than editing the original.

| Version | Date (UTC)       | Trigger                                                   | Outcome                                                       |
| ------- | ---------------- | --------------------------------------------------------- | ------------------------------------------------------------- |
| 1       | 2026-03-05 09:15 | Initial assessment, scope unknown                         | Deferred; insufficient facts, reconvene within 24 hours       |
| 2       | 2026-03-05 18:40 | Preliminary scope of 40,000 accounts                      | Deferred; scope figure not yet reliable, forensics continuing |
| 3       | 2026-03-06 16:30 | Confirmed scope of 3,180 accounts, exfiltration ruled out | Not material                                                  |

Two things this table does. It shows the determination was pursued continuously rather than deferred, which is the reasonableness question answered with evidence. And it preserves the earlier reasoning, so a later reversal shows the trigger that caused it rather than looking like a changed story.

## Record the negative determinations

Every determination gets a record, including and especially the ones that conclude an incident is not material.

The instinct is to document only the incidents that get disclosed, on the theory that a memo about a non-event creates exposure. The instinct is wrong for two reasons. The first is that a not-material determination is precisely the decision most likely to be second-guessed later, and the only defense against hindsight is a contemporaneous record showing the facts as they stood and the reasoning applied. The second is that the pattern across determinations is itself informative: a sequence of individually immaterial incidents through the same control weakness is the aggregation question, and an organization that keeps no records of negative determinations cannot see the pattern in its own history.

Keep a determination index alongside the individual records, listing every incident that entered the process and its outcome. The index is the artifact that answers "how many of these have there been" without a discovery exercise.

## Handling, retention, and privilege

The record is a sensitive document about a sensitive event, and the template is incomplete without saying where it lives. Decide this before the first incident, not during one.

**Storage.** Not in the incident-management tool, and not in a shared workspace whose access list is the security team plus everyone who was ever added to it. The record belongs in the location counsel designates, with an access list of named individuals rather than a group, and with the sources it cites reachable from it. Access is auditable, because "who read this and when" is a question that gets asked later.

**Retention.** Set by the records retention schedule and by counsel, never by the default lifecycle of whatever tool it happens to sit in. The common and avoidable failure is a determination record stored somewhere with an automatic purge shorter than the period over which the determination could be questioned. Retain superseded versions for the same period as the final one; the point of versioning is defeated if the earlier versions age out.

**Privilege.** Counsel decides the posture and the marking, and the security function's job is to avoid undermining it. Two practices do most of the work. Keep the facts ledger factual, with no speculation about liability, fault, or what a regulator will think, because a technical document salted with legal conclusions is both weaker evidence and a harder privilege argument. And route legal conclusions through counsel rather than drafting them and asking for review, which is a different thing and reads differently.

**Legal hold.** When a hold attaches, the record and everything it cites is in scope, including the incident chat channels the timeline was reconstructed from. Say so in the incident-response procedure so the channels are preserved rather than auto-expired, which is the most common way a hold arrives too late to matter.

## Relationship to the other artifacts

- The [postmortem](blameless-postmortem-template.md) is a separate document with a separate purpose. It may cite this record and must not replace it. Where an active [exception](../risk-management/security-exception-record.md) covered the access path, both documents say so.
- The determination process, its participants, and the count of incidents that entered it belong in the [quarterly board update](../board-reporting/quarterly-security-update-template.md). Governance of the disclosure process is a standing board interest independent of any single incident.
- Where the incident involved an AI system, the relevant row from the [AI system register](../ai-governance/ai-system-register-pattern.md) supplies the ownership, action scope, and reversibility facts the committee will ask for.

## Common failure modes

1. **Security making the call.** The determination is a legal and financial judgment. Security supplies facts and consequence analysis; the disclosure committee decides.
2. **Treating discovery as the start of a four-day clock.** The four days run from determination. The period before determination is governed by a reasonableness standard, and delay without a recorded reason is the exposure.
3. **Facts and judgment mixed.** A narrative memo where the reader cannot tell what was confirmed from what was assumed. Keep the ledger separate and status-marked.
4. **Reconstructing the record afterward.** A memo written after the incident closes, describing what the team remembers deciding, is worth a fraction of a contemporaneous one. Write it as it happens, even in rough form.
5. **No negative determinations on file.** Only the disclosed incidents have records, so the immaterial ones are invisible and the aggregation question cannot be answered.
6. **Quantitative only.** The memo compares a dollar figure to a threshold and stops. The qualitative factors are where a small incident becomes a material one.
7. **No reassessment triggers.** The conclusion is stated without saying what would change it, so a later reversal reads as a changed story rather than as new facts.
8. **One clock in view.** The process is built around the securities timeline while a contractual or sector obligation measured in hours expires unnoticed.
9. **Sensitive record, default storage.** The memo lives in the incident tool or a broadly shared drive, under whatever retention that tool applies, and the superseded versions age out before anyone asks about them.

# Blameless incident postmortem

A working template for a security incident postmortem written in the blameless style. Designed for incidents that warrant a structured review (typically severity 1, 2, and near misses with material learning value), and structured to feed action items into the regular engineering and program backlog rather than dying inside an incident-management tool.

## What "blameless" actually means

Blameless postmortems are not about avoiding accountability. They are about separating two questions that incident reviews often conflate: "what caused this to happen, and how do we prevent it" versus "who is at fault, and what should happen to them." Mixing those questions in the same room produces defensive answers and shallow root causes.

Accountability for decisions and behaviors is real and is handled in management 1:1s, performance processes, or HR channels, not in the postmortem. The postmortem assumes good intent from everyone involved and focuses on the conditions that made the failure possible, the signals that were missed or delayed, and the system changes that reduce the probability or impact of recurrence.

## When to write one

- Any severity 1 or 2 security incident, regardless of customer or regulatory impact.
- Any incident where regulatory notification was issued.
- Any near miss where a small change in circumstance would have produced a material incident.
- Any incident where the response itself revealed a gap (slow detection, slow containment, miscommunication, missing runbook).

Sev 3 and below typically do not warrant a full postmortem unless they reveal a pattern across multiple incidents.

## Header

```text
Incident ID:        SEC-2026-Q1-014
Title:              <one-line description, not a tool name>
Severity (final):   SEV2
Status:             Closed / In remediation
Detection time:     2026-03-04 14:22 UTC
Containment time:   2026-03-04 17:48 UTC
Resolution time:    2026-03-05 09:10 UTC
Related risk ID:    OPS-014
Related records:    EXC-2026-031 (exception active over the failure path)
                    MAT-2026-03 (materiality determination)
Postmortem author:  <name>
Reviewers:          <2 to 4 names, including one engineering manager outside the immediate team>
Distribution:       Engineering, Security, Affected business owners
```

The three related identifiers are worth carrying in the header rather than buried in the analysis. They are the first thing a reader from outside engineering looks for, and their absence is itself informative: an incident with no matching risk register row means the risk was never enumerated, which is a finding in its own right.

## Summary

Three to five sentences. What happened in plain language, who was affected, what the customer impact was, how the incident was resolved. A non-security executive should be able to read only the summary and walk away with a correct mental model.

## Impact

Quantify where possible. Avoid vague language ("some customers", "limited impact"). If you do not have the numbers yet, say so and date when you will.

- **Customers affected:** count, segment, geography if relevant.
- **Data affected:** record count, classification, whether the data left the trust boundary.
- **Systems affected:** named services or capabilities.
- **Duration of degraded service:** start, end, partial-vs-full.
- **Regulatory exposure:** statutes triggered, notifications issued or pending, deadlines.
- **Financial exposure:** estimated direct cost, estimated remediation cost, estimated revenue impact.

If the answer to any line is "none", write "none". Blanks read as undisclosed.

## Timeline

A chronological list of events with UTC timestamps. Include:

- The earliest event we can identify retrospectively, even if it was not noticed at the time.
- The first signal anyone (human or automated) detected.
- Each escalation and ownership handoff.
- Each major decision and who made it.
- Containment, eradication, recovery, and verification points.
- Customer or regulator communications sent.

Two columns: timestamp, event. Keep entries terse; the narrative goes in the analysis sections, not in the timeline.

| Timestamp (UTC)  | Event                                                                                                                                          |
| ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| 2026-03-03 02:11 | First credential-stuffing attempt against the AccountView Classic admin role, observed in retrospective log review (not detected at the time). |
| 2026-03-04 14:22 | Detection fires on admin authentication from an unrecognized ASN; SOC analyst paged.                                                           |
| 2026-03-04 14:35 | Incident declared at SEV3 by on-call lead.                                                                                                     |
| 2026-03-04 15:10 | Severity upgraded to SEV2 after session replay showed customer-record access.                                                                  |
| ...              | ...                                                                                                                                            |

## What went well

Two to five items. The postmortem reinforces good behavior in addition to identifying gaps. Include both technical and process items.

- "Detection fired within 8 minutes of the privilege escalation step, well inside the published SLO."
- "Incident commander rotation worked smoothly across two timezones; no decisions stalled at handoff."

## What went poorly

Two to five items. Be specific. Avoid passive voice. Identify the system or process, not a person.

- "The SOC runbook for this alert family had not been exercised since Q3 last year, and three of the steps referenced retired tooling."
- "Customer communication was delayed 90 minutes because no one on the incident-response bridge had publish access to the status page."

## Root causes and contributing factors

Distinguish causes from contributing factors. Use 5-whys, Ishikawa, or a similar method, but show your work; do not just assert "root cause: X".

- **Root cause:** the change in system state without which this incident would not have occurred. Most incidents have one or two.
- **Contributing factors:** the conditions that made the root cause possible or made detection / response slower. There are usually several.
- **Trigger:** the proximate event that initiated the chain. The trigger is rarely the most useful place to intervene; the conditions are.

Be careful with human error. "Operator ran the wrong command" is almost never a complete root cause. The useful question is why the system permitted, defaulted to, or failed to validate the wrong command.

Be equally careful with the opposite framing. Where an active [security exception](../risk-management/security-exception-record.md) covered the failure path, say so explicitly and name the record. The exception is a contributing factor and is frequently the root cause, and a postmortem that omits a live waiver over the exact gap that failed is not a complete analysis. This is not an accountability finding against whoever approved it; the exception was a documented decision made with stated compensating controls, and what the postmortem contributes is evidence about whether those controls performed as the approval assumed. That evidence is worth more at the next renewal review than anything else the program will produce.

## Detection and response analysis

Three short subsections.

- **Detection.** How long from earliest evidence to first human awareness? What signal eventually caught it, and what signals should have caught it earlier? Were the earlier signals present in our data but unalerted, or simply not present?
- **Containment.** How long from detection to containment? What slowed it down? Were the containment actions reversible if we had been wrong about scope?
- **Communication.** Internal communication latency, external communication latency, accuracy of early messages, post-event corrections.

## Action items

A short, tracked list. Each item has an owner, a category, a due date, and a tracking ID in the engineering backlog. Categories help future-you see whether the program is investing in the right places over time.

| ID    | Category           | Action                                                                                  | Owner    | Due        | Tracking  |
| ----- | ------------------ | --------------------------------------------------------------------------------------- | -------- | ---------- | --------- |
| AI-01 | Preventive control | Enforce hardware-backed MFA on the affected admin role across all environments.         | `<name>` | 2026-04-15 | ENG-12044 |
| AI-02 | Detective control  | Add an alert for the specific authentication-anomaly pattern observed in the timeline.  | `<name>` | 2026-03-31 | SOC-4881  |
| AI-03 | Response           | Refresh the runbook for this alert family and exercise it in a tabletop within 60 days. | `<name>` | 2026-05-01 | SOC-4882  |
| AI-04 | Communication      | Grant the on-call incident commander a status-page publishing role.                     | `<name>` | 2026-03-20 | CORP-2210 |

Categories worth using: preventive control, detective control, response, recovery, communication, training, governance, supplier.

A postmortem with more than ~8 action items is usually un-shippable. Pick the items that move the needle; defer the rest to a separate hardening backlog with a date.

## Glossary

A short list of terms used in the postmortem that an executive distribution may not know. Two or three lines per term. The glossary is for the reader's benefit, not yours.

## Distribution and follow-through

- Publish to the agreed internal distribution within 14 days of incident closure for SEV1, 30 days for SEV2.
- Track action items in the engineering backlog with the postmortem ID; do not let them live only inside the incident-management tool.
- Re-review the postmortem at the next quarterly security update and surface any action items that have slipped past their due date.

## Where this connects

- The [materiality determination record](materiality-determination-record.md) runs in parallel on a different clock, with a different owner and a different question. The postmortem asks why this happened and what we change; the determination asks whether a reasonable investor would consider it important. Do not merge them. The postmortem may be cited by the determination as a source of facts, and it must not be the vehicle for the determination.
- The realized risk is a row on the [risk register](../risk-management/risk-register-pattern.md). Name it. If there is no such row, that is a finding worth stating: the risk was never enumerated, and the register's coverage is the thing to fix.
- Any active [exception](../risk-management/security-exception-record.md) over the failure path is named in the header and analysed in the causes section.
- Material incidents, their detection paths, and the status of their action items appear in section 3 of the [quarterly board update](../board-reporting/quarterly-security-update-template.md). Brief the committee chair when the incident closes, not when the deck is assembled.
- Action items of the "exercise this within 60 days" kind, like AI-03 above, land in the [tabletop exercise pattern](tabletop-exercise-pattern.md). That document deliberately reuses this action-item format, so exercise findings and incident findings enter one backlog on equal footing, which matters because findings from an event that did not really happen are the first to be deprioritized.

## Common failure modes

1. **Naming individuals as causes.** "Engineer X deployed the bad change." Replace with the system condition that allowed the change to reach production without the safeguard.
2. **Treating timeline accuracy as the goal.** A perfect timeline with no analysis teaches nothing. Spend more pages on causes and actions than on the timeline.
3. **Action items without owners and dates.** These never ship. Every item has both, before the document is signed off.
4. **Re-litigating decisions.** "We should have done Y instead of X." Frame as "in retrospect, the decision criteria did not include Z; future runbooks should require Z." Decisions made with the information available at the time are not on trial.
5. **Closing the loop quietly.** Action items shipping six months later, with no broader announcement, lets the institutional muscle memory of the incident fade. Brief the closure at the next program review.

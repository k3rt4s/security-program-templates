# Decision rights register

A working pattern for recording who holds each decision a security program depends on, opinionated about the split between the criteria for a decision and the assignment of it.

Most security programs can describe how each of their decisions should be made. Far fewer can say, without a meeting, who makes it. The gap shows up at the worst possible moment: an exercise stalls because nobody in the room can revoke a vendor's production credential, or an incident waits three hours for someone to establish who is allowed to take a product offline. This register is the artifact that closes it, and its value is less in the rows it contains than in the ones it reveals to be empty.

## Criteria live with the domain, assignment lives here

The organizing rule, and the thing that keeps this from becoming a duplicate of every other document in a program.

The **criteria** for a decision are what constitutes it, at what threshold it escalates, and what evidence it requires. Criteria are domain knowledge, they are useless out of context, and they belong in the document that owns the domain. The residual-score tiers in the [exception record](../risk-management/security-exception-record.md) are criteria. The materiality factors in the [materiality determination record](../incident-response/materiality-determination-record.md) are criteria.

The **assignment** is which named person holds the authority. That is an organizational fact rather than a domain one. It changes when people change, it has to be consistent across domains, and it is the half that has to be visible in one place.

One rule makes the split hold rather than creating two sources of truth: a domain document states criteria and points here for the holder. It never names the holder in place. A role name written into a table in a domain document goes stale silently, because nothing prompts anyone to revisit it when the person leaves.

## What this is, and what it is not

It is a list of decisions, each with a named holder, a named delegate, an escalation path, and a pointer to the document that defines when the decision is made.

It is not a RACI matrix. RACI answers four questions of which only one carries consequences, and the predictable outcome is that the three non-binding columns absorb the effort while the accountable column ends up holding a team name. If a program already maintains a RACI, this register is the one column of it that matters, filled in properly.

It is not an org chart, not a policy, and not a program charter. A charter as a genre is mission, scope, governance forums, and escalation, most of which is written once and read never. The part with operational consequence is the decision rights, and separating it is what makes it maintainable.

## The unit is a decision, not a role

Rows describe decisions someone can be woken up to make, phrased so that the person reading at 3am knows whether it applies to them.

> **DR-014.** Authorize taking the customer-facing portal offline in response to an active security incident.

Not a row: "The CISO is responsible for information security." That is a job description. It cannot be executed against, it cannot be found to be missing, and it will sit unchallenged for years.

The test for a candidate row is whether you can imagine the sentence "nobody could say who holds this" being a finding. If not, it is not a decision right.

## Columns

| Column               | Purpose                                                        | Notes                                                                                  |
| -------------------- | -------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Decision ID          | Stable identifier                                              | Never reuse                                                                            |
| Decision             | One sentence, imperative, recognizable under pressure          | The most important column                                                              |
| Trigger or threshold | When this decision arises                                      | A pointer, not a restatement. The criteria live in the source document                 |
| Holder               | One named person, with their role                              | A role alone is not an assignment. Roles are held by people and people are unavailable |
| Delegate             | One named person                                               | Not optional. See below                                                                |
| Escalation           | Who decides if neither is reachable, or if the holder declines | The path, not a mailbox                                                                |
| Criteria source      | The document that defines the decision                         | Keeps this register from restating domain content                                      |
| Last confirmed       | Date the holder acknowledged holding it                        | Not the date it was assigned. See below                                                |

## The delegate is not optional

An authority with a single holder is unavailable a meaningful fraction of the time once travel, leave, illness, and sleep are counted, and incidents show no preference for business hours. A register that names one person per decision has documented a single point of failure rather than removed one.

Name a delegate for every row, and record where the delegate's authority differs from the holder's, because a delegate with a lower limit is a real and reasonable arrangement that has to be written down rather than discovered mid-incident.

## Confirmation, not publication

A named holder who has not been told they hold it is not a holder. This sounds obvious and it is the most common defect in these registers, because assignment happens in a document review and never leaves the document.

Record the date the holder acknowledged the assignment, not the date the row was written. Re-confirm annually and immediately on any change of holder, delegate, or scope. This is the same discipline the exception record applies to compensating controls, where a verification date rather than a checkbox is what makes the record honest, and it fails the same way when skipped.

## The completeness test

The register earns its place by making absences visible, so finding the missing rows is the work rather than a follow-up to it.

Two sweeps, run when the register is built and again annually:

**Walk the domain documents.** Every place a program document says a decision is made, approved, authorized, declared, or signed off, there should be a row. This repository's set alone generates the starting list below.

**Walk the scenario families.** Take each family from the [tabletop exercise pattern](../incident-response/tabletop-exercise-pattern.md) and trace the decisions it forces. Exercises find missing authorities reliably, which is useful, but they find them one scenario at a time and only after the exercise has been designed and run. The register finds them by inspection, which is considerably cheaper.

A decision with no row is not a gap in the register. It is a gap in the organization, and the register is simply the first place it becomes visible.

## Starting set

The decisions the rest of this repository already implies, with the document that defines each one. A program that fills only these has a working register.

| Decision                                           | Criteria source                                                                              |
| -------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| Accept a risk, at each residual-score tier         | [Risk register](../risk-management/risk-register-pattern.md)                                 |
| Approve a security exception, at each tier         | [Exception record](../risk-management/security-exception-record.md)                          |
| Convene the disclosure committee                   | [Materiality determination record](../incident-response/materiality-determination-record.md) |
| Determine that an incident is material             | [Materiality determination record](../incident-response/materiality-determination-record.md) |
| Declare an incident and set its severity           | [Postmortem](../incident-response/blameless-postmortem-template.md)                          |
| Take a revenue-generating product offline          | [Tabletop exercise pattern](../incident-response/tabletop-exercise-pattern.md)               |
| Authorize a ransom payment                         | [Tabletop exercise pattern](../incident-response/tabletop-exercise-pattern.md)               |
| Notify a regulator, per obligation                 | [Regulatory applicability register](../compliance/regulatory-applicability-register.md)      |
| Approve a new agentic AI use case                  | [AI system register](../ai-governance/ai-system-register-pattern.md)                         |
| Stop a running AI system                           | [AI system register](../ai-governance/ai-system-register-pattern.md)                         |
| Onboard a vendor that failed its tier's assessment | [Vendor tiering](../third-party-risk/vendor-risk-tiering-pattern.md)                         |
| Revoke a vendor's production credential            | [Vendor tiering](../third-party-risk/vendor-risk-tiering-pattern.md)                         |
| Override an access review's revoke finding         | [Access review](../identity/access-review-pattern.md)                                        |
| Disable an unowned non-human identity              | [Access review](../identity/access-review-pattern.md)                                        |

Two of these are worth noticing. Revoking a vendor's production credential and stopping a running AI system are both authorities that programs discover they have not assigned, usually while needing to exercise them, and both appear in this set only because two other documents independently reported finding them missing.

## Three views worth producing

The register is a table; the value comes from reading it three ways.

**Single-holder decisions.** Rows where the delegate is empty or is the same person. This is the availability view and it is the one that predicts a stalled incident.

**Concentration.** One person holding many critical decisions is a resilience exposure regardless of how capable they are, and it belongs on the [risk register](../risk-management/risk-register-pattern.md) as a row that no individual decision would ever surface. It also tends to describe the person least able to take a holiday, which makes it worth raising for reasons beyond risk.

**Vacancy.** Rows whose holder or delegate has left, changed role, or moved organization. Wire this to the joiner, mover, and leaver process rather than to an annual review, because a departure is exactly when the register goes wrong and exactly when nobody thinks to check it.

## Where this connects

- Every domain document in this set defines criteria for at least one decision and points here for the holder. The pointer runs one way on purpose.
- Concentration and single-holder findings are rows on the [risk register](../risk-management/risk-register-pattern.md).
- A decision that cannot be assigned, because no role in the organization plausibly holds it, is not a register defect to leave blank. Raise it as a gap, and if the organization chooses to run without it, that is an [exception](../risk-management/security-exception-record.md) with an expiry.
- The [tabletop exercise pattern](../incident-response/tabletop-exercise-pattern.md) tests whether an assignment survives contact with a real decision under time pressure. The register says who; the exercise finds out whether they knew, were reachable, and acted.
- Count of single-holder critical decisions is a candidate indicator for the [metrics catalog](../metrics/security-metrics-catalog.md) and the [board update](../board-reporting/quarterly-security-update-template.md). It is unusually legible to a board, because it describes a risk they understand from every other part of the business.

## Common failure modes

1. **Roles instead of people.** "Security leadership" is a criteria expression, not an assignment. A decision is held by a person.
2. **No delegate.** One holder per decision documents a single point of failure rather than removing one.
3. **Assignment without confirmation.** The holder was never told. Record the acknowledgment date, not the assignment date.
4. **Restating criteria.** The register grows into a summary of every other document, drifts from all of them, and becomes the version nobody trusts. Point, do not copy.
5. **Job descriptions as rows.** "Responsible for information security" cannot be found to be missing, so it never is.
6. **Built once.** Assignment goes wrong at departures and reorganizations, so tie the vacancy view to the leaver process rather than to an annual cycle.
7. **Blanks left as blanks.** An unassignable decision is treated as an incomplete document. It is an unmade organizational decision, and it should be raised rather than formatted around.
8. **Turned into a RACI.** Three of the four columns do not bind, and the effort goes into them.

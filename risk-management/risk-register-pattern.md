# Risk register pattern

A working pattern for an enterprise risk register, opinionated about the things that most organizations get wrong the first time they build one.

## What a risk register is, and what it is not

A risk register is a curated list of the specific things that could go wrong, scored against an honest likelihood and impact scale, with named owners and an explicit treatment decision. It is the artifact a CISO points at in a board meeting when asked "what do you actually worry about, and what are we doing about each one."

A risk register is not a list of controls, not a list of audit findings, not a list of vulnerabilities, and not a list of compliance gaps. Those each have their own artifact and their own lifecycle. Mixing them collapses signal.

## Risk statement format

A risk statement is the single highest-leverage field on the register. Most registers get this wrong by writing risks as nouns ("phishing", "ransomware", "third-party access") or as control gaps ("no MFA on legacy app"). Neither tells anyone what they are deciding.

Use the form:

> A `<threat source>` may exploit `<vulnerability>` in `<asset>`, causing `<consequence>`.

Worked example, row `OPS-014` from the [Ambervale thread](../README.md#the-worked-example):

> A financially motivated external attacker may exploit weak authentication in AccountView Classic, the legacy customer portal, causing exposure of up to 200,000 customer PII records and a notifiable breach under state breach-notification laws.

This format forces clarity on four dimensions a board cares about: who is doing this to us, how, against what, and how bad. If a risk cannot be written this way, it is probably not a risk; it is a vulnerability, a control gap, or an issue.

## Columns

| Column                | Purpose                                           | Notes                                                                                                                                      |
| --------------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Risk ID               | Stable identifier                                 | Never reuse; use a prefix to indicate domain (e.g. `OPS-014`, `THIRD-008`).                                                                |
| Risk statement        | One sentence in the format above                  | This is the single most important column.                                                                                                  |
| Domain                | Categorical grouping                              | Examples: operational, third-party, compliance, financial, strategic, reputational, technology. Use a small fixed taxonomy, not free text. |
| Threat source         | Who/what realises the risk                        | External attacker, insider (malicious), insider (negligent), supplier, natural event, regulatory change.                                   |
| Inherent likelihood   | 1 to 5, before any controls                       | Defined in the rubric below.                                                                                                               |
| Inherent impact       | 1 to 5, before any controls                       | Defined in the rubric below.                                                                                                               |
| Inherent score        | Likelihood times impact                           | Computed; do not edit by hand.                                                                                                             |
| Existing controls     | The controls that actually reduce this risk today | Reference control IDs from your control library, not free text. If a row has no controls listed, residual = inherent.                      |
| Control effectiveness | High / Medium / Low / Untested                    | Untested controls do not reduce residual likelihood.                                                                                       |
| Residual likelihood   | 1 to 5, after current controls                    |                                                                                                                                            |
| Residual impact       | 1 to 5, after current controls                    | Impact rarely changes with controls; usually only likelihood does.                                                                         |
| Residual score        | Likelihood times impact                           | Computed.                                                                                                                                  |
| Treatment             | Avoid / Mitigate / Transfer / Accept              | A row in "Accept" requires named-executive sign-off recorded under decisions.                                                              |
| Owner                 | One person, not a team                            | Use a name, not a job title. The owner changes when the person changes.                                                                    |
| Review cadence        | Quarterly / Semi-annual / Annual                  | Higher residual score = more frequent review.                                                                                              |
| Next review date      | Calendar date                                     | Drives the operational rhythm.                                                                                                             |
| Status                | Open / Treatment-in-progress / Accepted / Closed  | "Closed" means the asset is gone or the threat source is no longer credible, not "we are tired of looking at it".                          |

## Likelihood rubric (5-point)

Anchor likelihood to a stated time horizon. Without a horizon, "likely" is meaningless. Use a 12-month horizon unless a strategic risk warrants longer.

| Score | Label          | Definition for a 12-month horizon                                                           |
| ----- | -------------- | ------------------------------------------------------------------------------------------- |
| 1     | Rare           | Has not occurred in the last 5 years and has no clear mechanism today.                      |
| 2     | Unlikely       | Has occurred to peer organizations but not to us; mechanism exists but is well controlled.  |
| 3     | Possible       | Has occurred at peer organizations within the last 12 months, or to us in the last 5 years. |
| 4     | Likely         | Has occurred to us in the last 12 months, or is actively being attempted against us.        |
| 5     | Almost certain | Occurs continuously or will occur within the period barring a control change.               |

## Impact rubric (5-point)

Anchor impact in money where possible, and in the secondary dimensions you actually care about (regulatory, customer trust, life-safety, mission). Pick the highest dimension that applies for the row.

| Score | Label    | Financial    | Regulatory / legal                          | Customer / reputational                    |
| ----- | -------- | ------------ | ------------------------------------------- | ------------------------------------------ |
| 1     | Minimal  | Under $50k   | Internal policy issue only                  | No external visibility                     |
| 2     | Minor    | $50k - $250k | Reportable internal control deficiency      | Small customer-facing degradation          |
| 3     | Moderate | $250k - $1M  | Reportable to a regulator; no fine expected | Local press; isolated customer impact      |
| 4     | Major    | $1M - $10M   | Material regulatory finding; fine likely    | National press; broad customer impact      |
| 5     | Severe   | Over $10M    | Enforcement action; license at risk         | Sustained reputational damage; trust event |

Calibrate these bands to the organization. The thresholds above are illustrative for a mid-market US enterprise.

## Treatment options and the bar for "Accept"

- **Avoid:** stop doing the activity that generates the risk. Decommission the system, exit the line of business, do not sign the contract.
- **Mitigate:** add or strengthen controls to lower likelihood or impact. Most rows live here.
- **Transfer:** shift financial consequence via insurance, contract, or to another party. Transfer never eliminates a risk; it changes who pays.
- **Accept:** carry the residual risk without further action. The bar is named-executive sign-off, recorded with the rationale, recorded for a finite period, and re-reviewed at expiry. The record that holds that sign-off is the [security exception and risk acceptance record](security-exception-record.md); an Accept treatment with no such record behind it is an undocumented gap wearing the paperwork of a decision.

## Where this connects

The register is the spine of the program and most of its rows point somewhere else. Keep the links by ID in both directions.

- A row treated by deliberately not meeting a requirement has a [security exception record](security-exception-record.md) behind it. That record's compensating controls are what appear in this register's existing-controls column, and its expiry date is what should drive this row's next review.
- The top rows, with movement since last quarter, are section 1 of the [quarterly board update](../board-reporting/quarterly-security-update-template.md). Rows that never appear there are not being governed, they are being filed.
- When a risk materializes, the [postmortem](../incident-response/blameless-postmortem-template.md) names the register row. A realized risk that was scored Unlikely is a calibration finding, and the honest move is to re-score the neighbors that share its reasoning rather than only the row that fired.
- A [tabletop exercise](../incident-response/tabletop-exercise-pattern.md) finding that will not be remediated is a row here, or an exception if it means a stated requirement is knowingly unmet. Findings from a simulated event are the ones most likely to be filed and forgotten, and the register is where they stop being optional.
- Risks about AI systems are written here in the standard form and cite the relevant row on the [AI system register](../ai-governance/ai-system-register-pattern.md). The AI register inventories; this register scores.
- The [control evidence acceptance register](../compliance/control-evidence-acceptance-register.md) supports the effectiveness judgment for controls named here. Evidence that is Pending, Rejected, Expired, or Partial for the risk's scope does not justify a tested effectiveness rating; where the unsupported assertion creates material consequence, this register receives that consequence rather than the evidence gap itself.
- Third parties are tiered by exposure in the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md), and a specific vendor failure is a row here like any other. Concentration across the critical tier, several vendors sharing one region or one identity provider, is a separate row that has no natural owner and will go unwritten unless the program claims it.
- This register says an Accept requires named-executive sign-off. Which executive, for which tier, is an assignment and lives in the [decision rights register](../governance/decision-rights-register.md). Concentration of critical decisions in one person is itself a row here, and it is one no individual decision would ever surface.

## Common failure modes

1. **Inflated severity.** Every risk is "high". Apply the impact rubric honestly. If 80 percent of rows are score 5, the rubric is wrong or the discipline is missing.
2. **Risks as control gaps.** "We do not have MFA on the legacy portal" is not a risk; it is a control gap. The risk is the consequence of an attacker exploiting that gap. Write the risk; track the gap on a separate remediation backlog.
3. **Owner = team.** Teams do not own risks; people do. When the owner moves on, the row is reassigned by name.
4. **No review cadence.** The register becomes a snapshot rather than a living instrument. Tie the cadence to residual score and put dates in a calendar.
5. **Closing risks because they are stale.** A risk is closed only when the asset is decommissioned or the threat source is no longer credible. Otherwise it stays open and gets re-treated.

## Minimum viable register

If a register is brand new, start with 10 to 25 rows that cover the crown jewels and the top business-process dependencies, taken from the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md) rather than assembled here. Building that list first is worth the two weeks it takes, because a register seeded from a workshop reflects who argued hardest rather than what the business cannot lose. Resist the urge to fill it to 200 rows in the first quarter. A small register that is actually maintained beats a large register that is not.

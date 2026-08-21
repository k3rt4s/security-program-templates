# Security metrics catalog

A working catalog of security metrics where every entry carries the two fields almost nobody publishes: how the number is gamed, and what it stays silent about.

Metrics catalogs are abundant and nearly all of them are lists of definitions. A definition is the easy half. The hard half is knowing what each number does when someone has an incentive to move it, and what it leaves out, because a program steered by metrics that nobody has stress-tested optimizes for the measurement rather than the outcome, and does so invisibly.

## What a metric is for

A metric exists to change a decision. That is the whole test, and it is worth applying before adopting anything below.

Name the decision the number informs and the threshold at which the decision changes. If neither can be named, the number is a report rather than a metric. Reports are fine and this is not an argument against having them; it is an argument against treating them as instruments and putting them on a dashboard where they consume attention and imply that someone is steering by them.

Three questions before any metric is adopted. All three have to be answerable.

1. What decision does this change, and at what threshold?
2. How would someone make this number improve without the underlying thing improving?
3. What is it silent about?

Question 2 is not cynicism about the team. Gaming is usually unintentional: a definition gets tightened for a good reason, a scope narrows during a migration, an inventory loses assets nobody noticed. The number improves and no one lied. Knowing the mechanism in advance is what lets you notice.

## The denominator problem

Most security metrics are ratios, and most of them improve when the denominator shrinks. This is the single most common way a security dashboard becomes wrong while remaining accurate.

Coverage percentages improve when scope narrows. Patch latency improves when the asset inventory quietly loses assets. Crown-jewel control coverage improves when a capability drops off the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md). Any "percent of in-scope X" improves when the definition of in-scope tightens. None of these require anyone to act in bad faith, and none of them are visible if only the ratio is reported.

The fix is cheap and almost nobody does it: publish the denominator next to the ratio, every time, and treat a change in the denominator as an event worth explaining. An MFA coverage figure that moved from 94 to 97 percent while the identity count fell by 1,200 is not an improvement, it is a question.

## When the denominator comes from the thing being measured

The fix above assumes the denominator is capable of being wrong. Where it is not, the metric fails in a way that publishing it cannot reach.

An endpoint agent coverage figure taken from the endpoint agent console is 100 percent by construction. A machine with no agent does not appear in the console, so it is missing from the numerator and the denominator alike, and the ratio is arithmetically correct while answering a question nobody asked: of the machines running the agent, how many are running the agent. The shape repeats wherever a tool reports its own coverage. Patch compliance from the patch manager, which cannot see the host that never enrolled. MFA coverage from the identity provider, which cannot see the local account on the appliance. Backup success rate over the systems configured for backup.

This is not the shrinking denominator described above, and it survives that section's remedy. A shrinking denominator was right and moved, which is why showing it works. A self-referential denominator was never right and never moves, so publishing it alongside the ratio changes nothing: both figures come from the same source, they agree because of that rather than despite it, and the agreement reads as corroboration.

So a coverage claim needs its denominator from an authority independent of the thing being measured, and the authority is worth naming in the metric's definition rather than assumed. The way to get one is to reconcile the sources that should already agree: the directory, the tool's own console, the asset or configuration system, and the address space itself from lease records, network ranges, or cloud provider inventory. For anything scoped to people rather than machines, the HR record is the independent authority and it is usually the only one nobody argues with.

Join any two and the disagreement is the finding. Assets in the directory with no agent reporting. Agents reporting from assets in no inventory, which is the more interesting direction and the one a coverage percentage can never surface. Addresses answering that no system claims. The count of machines the directory knows about and the console does not is a better metric than the coverage percentage it replaces, and it is what the program was trying to say all along.

The repository already holds this move once, scoped narrowly. The [AI system register](../ai-governance/ai-system-register-pattern.md) runs four discovery channels rather than a survey and reports discovery coverage rather than row count, for exactly this reason: a register built from the systems that registered themselves is complete with respect to itself. The generalization is that the same reasoning applies wherever a number carries the word coverage. The [evidence readiness register](../incident-response/evidence-readiness-register.md) asks it of a log source in its coverage confidence column, where an agent that stopped reporting leaves a retention window that looks intact and is not.

The cost is two exported inventories and a join, which is the lowest implementation bar on this page and the reason there is no good excuse for the console figure.

## Activity and effect

Leading versus lagging is the usual split and it is less useful than activity versus effect.

An activity measure counts work performed: assessments completed, reviews conducted, tickets closed, training assigned. An effect measure counts exposure removed: vendors whose access was narrowed, privileged accounts eliminated, systems brought inside a control boundary.

Programs report activity because it is available and it moves reliably. The trouble is that activity measures are always achievable by working harder and never require the program to be right about anything. Pair each one with an effect measure for the same area and show them together, which is a quiet way to teach the difference to an audience that has only ever been shown the first kind.

## The catalog

Terse by design. The definition column is the part you would adapt; the last two columns are the part worth reading.

### Vulnerability and patch management

| Metric                             | Definition                                                               | Gaming mode                                                        | Blind spot                                                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------- |
| Critical patch latency             | Days from vendor release to 95% deployment, severity-critical only       | Narrow what counts as critical; let the asset inventory lose hosts | Assets not in the inventory at all, which is where the unpatched population concentrates                                                |
| Vulnerability backlog age          | Median age of open findings above a severity threshold                   | Close items as risk-accepted or won't-fix; re-scope the scanner    | Says nothing about exploitability or whether the asset is reachable                                                                     |
| Finding arrival vs closure rate    | Findings created per month against findings closed per month             | Report closure alone, which reads as progress in either direction  | Whether the closures were the right ones. See [exposure prioritization](../vulnerability-management/exposure-prioritization-pattern.md) |
| Classes eliminated                 | Root causes fixed, e.g. a base image, not individual findings closed     | Define "class" narrowly so routine fixes each count as one         | Says nothing about whether the eliminated classes were the ones that mattered. Pair with arrival rate, which is what it should move     |
| Exposure of internet-facing assets | Count of known-exploited vulnerabilities on externally reachable systems | Define "externally reachable" narrowly                             | Depends entirely on the accuracy of the external inventory, which is usually the weakest inventory                                      |

### Identity and access

| Metric                       | Definition                                                            | Gaming mode                                                                  | Blind spot                                                                                                                                       |
| ---------------------------- | --------------------------------------------------------------------- | ---------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| MFA coverage                 | Percent of in-scope identities with phishing-resistant MFA            | Tighten "in-scope" to exclude the hard populations                           | Non-human identities, which are usually the majority of principals and routinely excluded by definition                                          |
| Standing privileged accounts | Count of accounts holding privilege permanently, not just in time     | Move privilege into a group or a role and count the role as one              | A low standing count paired with broad, unreviewed just-in-time elevation is not the improvement it appears to be                                |
| Orphaned account age         | Days between a departure and account disablement                      | Redefine orphaned; measure only employees                                    | Third-party and vendor accounts, which are the ones offboarding misses. See [vendor tiering](../third-party-risk/vendor-risk-tiering-pattern.md) |
| Revocation lag               | Days from an access review decision to the access actually going away | Report campaign completion instead, which measures whether reviewers clicked | Says nothing about whether the reviews were any good. See [access review](../identity/access-review-pattern.md)                                  |

### Detection and response

| Metric               | Definition                                                          | Gaming mode                                                           | Blind spot                                                                                                                           |
| -------------------- | ------------------------------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Mean time to detect  | Median from earliest evidence to first human awareness              | Start the clock at triage rather than at earliest evidence            | Survivorship. It is computed only over incidents that were detected, so the population it excludes is exactly the one you care about |
| Mean time to contain | Median from detection to containment                                | Declare containment at the first action rather than at verified scope | Whether the containment was correct, or was later found to have missed a foothold                                                    |
| Detection coverage   | Percent of relevant adversary techniques with a validated detection | Count rules written rather than detections validated by test          | Rules that exist, are counted, and do not fire in production conditions                                                              |

### Third-party risk

| Metric                      | Definition                                                               | Gaming mode                                                   | Blind spot                                                                 |
| --------------------------- | ------------------------------------------------------------------------ | ------------------------------------------------------------- | -------------------------------------------------------------------------- |
| Review SLA                  | Percent of in-scope vendors reviewed within their tier's cadence         | Re-tier vendors downward to shrink the in-scope set           | Pure activity. Every review can be on time while the exposure is unchanged |
| Tier reductions             | Count of vendors moved down a tier by narrowing their access             | Reclassify on paper without changing the access mode          | New vendors arriving at the top tier faster than existing ones are reduced |
| Critical-tier concentration | Count of Tier 4 vendors sharing a region, identity provider, or upstream | None worth the effort, which is unusual and makes it valuable | Only as good as the fourth-party answers behind it                         |

### AI governance

| Metric                       | Definition                                                             | Gaming mode                                                       | Blind spot                                                                                                           |
| ---------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Discovery coverage           | Number of the four discovery channels run in the period, not row count | Report registered systems instead, which is the flattering number | A channel can run and be misconfigured. See the [AI system register](../ai-governance/ai-system-register-pattern.md) |
| Agentic shutdown currency    | Percent of agentic systems whose shutdown was drilled this quarter     | Drill the easy system repeatedly                                  | Measures a procedure, not whether anyone would decide to invoke it. Pair with exercise findings                      |
| Ungoverned reclassifications | Count of systems that gained tool access without re-entering the gate  | Stop looking                                                      | Found only if something is watching for it, so a zero is meaningful only alongside how you looked                    |

### Program and governance

| Metric                         | Definition                                                | Gaming mode                                                                 | Blind spot                                                                                                                                          |
| ------------------------------ | --------------------------------------------------------- | --------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| Aged exceptions                | Count past their second renewal, by approval tier         | Close and reopen under a new record ID                                      | The undocumented gaps, which never became exceptions and so never appear here at all                                                                |
| Risk review currency           | Percent of register rows reviewed within their cadence    | Touch the review date without re-scoring                                    | Register coverage. A fully current register of the wrong twenty risks scores perfectly                                                              |
| Exercise participation         | Percent of named decision owners attending their scenario | Count attendance rather than participation                                  | Whether any finding shipped. See [tabletop exercises](../incident-response/tabletop-exercise-pattern.md)                                            |
| Phishing simulation click rate | Percent of simulated phishing clicked, by population      | Make the simulations easier, which happens gradually and without a decision | Says little about whether real phishing succeeds. The report rate is the more useful companion, because reporting is the behavior you actually want |

## Pair the metrics that cover each other

Several entries above have a blind spot that another entry covers exactly. Publish those together, as a pair, rather than scattered across a dashboard where the relationship is invisible.

| Pair                                                            | What the pairing exposes                                                              |
| --------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Third-party review SLA with tier reductions                     | Work performed against exposure removed                                               |
| MFA coverage with the identity denominator                      | Whether coverage improved or the population shrank                                    |
| Agent coverage with a directory-sourced device count            | Whether the console can see what it is not installed on                               |
| Mean time to detect with detection coverage                     | Whether fast detection reflects capability or only counts what was already detectable |
| Standing privileged accounts with just-in-time elevation volume | Whether privilege was removed or relocated                                            |
| Phishing click rate with report rate                            | Whether people stopped clicking or only stopped engaging                              |
| Agentic shutdown currency with exercise findings                | Whether the procedure works and whether anyone would invoke it                        |

## Retire metrics deliberately

Review the set annually and retire on a stated rule rather than on preference.

A metric that has sat inside its threshold for eight consecutive quarters and has never triggered an action is measuring either a solved problem or nothing at all. Both are reasons to remove it. The cost of a stale metric is not the effort of producing it; it is that it occupies one of the six to ten slots a governance audience will actually absorb, and it teaches that audience that the dashboard is decoration.

Retire it explicitly, say why, and keep the historical series. Quietly dropping a metric that has gone the wrong way is the one move that destroys trust in the whole set.

## Where this connects

- The [quarterly board update](../board-reporting/quarterly-security-update-template.md) carries a small indicator set, six to ten, with published thresholds and owners. That section is the selection problem; this catalog is the source it selects from. Do not put this catalog in front of a board.
- Do not aggregate to a single security score. A composite number hides the movement that matters, cannot be acted on, and invites comparison against organizations with different scopes. If a committee insists, publish the components alongside it and expect the components to be what actually gets discussed.
- Metrics about known gaps come from the [exception register](../risk-management/security-exception-record.md) and the [risk register](../risk-management/risk-register-pattern.md). Neither can measure the gaps nobody wrote down, which is the standing blind spot across this entire page.
- Where a metric is also an audit or certification evidence artifact, note that in the [control mapping](../compliance/control-framework-mapping-pattern.md) so a definition change is recognized as a mapping event rather than a reporting tweak.

## Common failure modes

1. **Definition without decision.** The catalog entry is precise and nobody can name what changes at what threshold. That is a report.
2. **Ratios without denominators.** The percentage improves because the scope shrank. Publish the denominator every time and explain its movement.
3. **A denominator from the same tool.** The coverage figure and the population it is measured against both come from the console, so the ratio is near perfect and means nothing. Source the denominator from an independent authority and report the disagreement between them.
4. **Activity only.** Everything on the dashboard counts work performed, so the program can be fully busy and fully ineffective without the numbers noticing.
5. **Survivorship treated as performance.** Detection metrics computed over detected incidents, presented as a statement about detection.
6. **Silent definition drift.** A definition tightens for a defensible reason mid-year and the series is compared across the change. Version the definitions and mark the break in the chart.
7. **The composite score.** One number, no action, invites false comparison.
8. **Immortal metrics.** Nothing is ever retired, the set grows, and the audience stops reading. Retire on a rule, keep the history, and never drop one quietly because it moved the wrong way.

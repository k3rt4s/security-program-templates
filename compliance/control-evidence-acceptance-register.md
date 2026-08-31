# Control evidence acceptance register

A working pattern for the standing record of whether evidence supports control assertions, opinionated about the difference between receiving an item and accepting it.

Control evidence is usually managed as a collection problem. An audit asks for documents, the program requests them, owners upload files, and a tracker turns green when something arrives. That workflow proves that a file moved. It does not prove that the file supports the assertion being made.

The distinction matters anywhere the same evidence is reused. A quarterly access-review export may be excellent evidence for one population, irrelevant to another, and stale for a third. Calling the document "received" hides all three judgments. This register records them.

## What this is, and what it is not

It is the standing index of control-evidence acceptance decisions. Each row says that a named evidence item was reviewed against one canonical control assertion, for one scope and one period, and was accepted, partially accepted, rejected, or allowed to expire.

It is not an evidence repository, an audit request list, a control library, a framework mapping, or an assessment report. Those have different lifecycles:

- The repository stores the evidence itself and applies its access, retention, and legal-hold rules.
- The request list moves evidence from a custodian to a reviewer.
- The control library defines what should operate.
- The [control mapping](control-framework-mapping-pattern.md) says how the canonical control relates to target frameworks.
- An assessment report states what an assessor concluded for one engagement.

This register stores metadata and the acceptance decision. It links to evidence in its system of record rather than becoming a second, less governed copy of sensitive material.

## The unit is an assertion, not a file

One row per file produces an inventory. The useful unit is narrower:

> One evidence item, reviewed against one canonical control assertion, for one stated scope and period.

The same file may therefore have several acceptance rows. That is not duplication. It is the honest record that evidence can be sufficient for one claim and insufficient for another.

An identity-provider export can establish that phishing-resistant MFA was enabled for six named administrator accounts on the export date. It cannot establish that those were all administrator accounts, that the setting operated throughout the quarter, or that an excepted legacy role was covered. Those are different assertions, and each needs its own basis.

This also prevents a common category error: design evidence is not operating evidence. A policy can prove that a requirement was approved. It cannot prove that the requirement operated. A screenshot can prove a configuration at one moment. It cannot prove operation across a period. Accept evidence only for the claim it can actually carry.

## Collection state and assurance decision are separate

Do not put requested, received, and accepted in one Status column. They answer different questions and create false progress when combined.

**Collection state** records movement:

- **Not requested:** no collection action has started.
- **Requested:** a named custodian owes an item by a stated date.
- **Received:** an item arrived and its integrity and readability can be checked.
- **Unavailable:** the item cannot be produced for the required scope or period.

**Assurance decision** records judgment:

- **Pending:** the item has not been evaluated. It supports no assertion yet.
- **Accepted:** it is sufficient for the precise assertion, scope, and period recorded in the row.
- **Partial:** it supports a bounded part of the assertion, with the uncovered remainder named.
- **Rejected:** it does not support the assertion. Record why rather than requesting a more convenient copy of the same thing.
- **Expired:** it was previously accepted and is no longer current enough for the claim.

Received plus Pending is ordinary. Received plus Rejected is useful. Both states expose work that a single green checkbox hides.

## The acceptance test

An item is accepted only when a reviewer can answer all of these from the row and the linked evidence.

1. **What exact assertion does it support?** Name the control behavior, not the framework heading.
2. **Where did it come from?** Record the system of record, the collection path, and whether the producer could select or alter the population.
3. **What scope does it cover?** Name the systems, populations, environments, and exclusions. "Production" is rarely a sufficient boundary.
4. **What period does it cover?** A point-in-time configuration and a quarter of operating effectiveness are different evidence.
5. **How was it validated?** Record the check performed, not "reviewed." Reconcile the population, inspect a sample, rerun the query, verify the signature, or reproduce the result.
6. **What did it establish?** Record the factual conclusion separately from the acceptance decision. Accepted evidence can establish that a control failed.
7. **What remains uncovered?** A Partial decision without a remainder is an optimistic Accepted decision with a different label.
8. **When does the decision expire?** Evidence goes stale because the control, population, system, or obligation changes, not because the file ages uniformly.
9. **Where may it be reused?** Reuse is allowed only through a mapping whose target remainder the evidence also satisfies.

Acceptance is not a statement that the control is good. It is a statement that the item is sufficient to support the particular claim written in the row. An accepted test failure is still accepted evidence, and it should create a finding rather than be rejected for being inconvenient.

## Columns

| Column                             | Purpose                                                  | Notes                                                                         |
| ---------------------------------- | -------------------------------------------------------- | ----------------------------------------------------------------------------- |
| Acceptance ID                      | Stable identifier for this decision                      | Never reuse. One evidence item may have several acceptance IDs                |
| Evidence item ID                   | Stable identifier for the linked item                    | Points to the system of record; does not imply acceptance                     |
| Canonical control                  | Source control from the control mapping                  | Evidence attaches here, not separately to every target framework              |
| Assertion tested                   | The precise behavior or outcome the item may support     | Written narrowly enough that Accepted is decidable                            |
| Evidence description               | What the item contains                                   | Describe it without copying sensitive contents into the register              |
| Evidence location                  | Governed system of record and reference                  | A link or repository reference, not an attachment                             |
| Custodian                          | One person accountable for production                    | Not the reviewer and not a group mailbox                                      |
| Scope covered                      | Systems, populations, environments, and exclusions       | State the denominator or authoritative population where one exists            |
| Period covered                     | Point in time or start and end dates                     | Must match the assertion's required period                                    |
| Provenance                         | How the item was generated and collected                 | Include filters, query version, signer, or collection method as applicable    |
| Validation performed               | The independent check applied                            | Reconciliation, reperformance, sample, signature check, or another named test |
| Evidence conclusion                | The factual result the item establishes                  | Positive, negative, excepted, or unknown; never infer it from acceptance      |
| Collection state                   | Not requested / Requested / Received / Unavailable       | Workflow only                                                                 |
| Assurance decision                 | Pending / Accepted / Partial / Rejected / Expired        | Judgment only                                                                 |
| Uncovered remainder                | What the item does not prove                             | Mandatory for Partial; useful for Accepted where edge cases remain            |
| Freshness trigger                  | Date or event that forces re-evaluation                  | Prefer system and scope changes over arbitrary annual dates                   |
| Target reuse                       | Target controls whose mapping and remainder permit reuse | A derived claim, not a second acceptance decision                             |
| Linked finding, exception, or risk | What receives a failed or bounded assertion              | Use stable IDs and keep the lifecycle in the owning artifact                  |
| Reviewer and date                  | Person who made the acceptance decision and when         | Separate from the custodian and control operator                              |

The register may store a content hash when evidence is exported and immutable. A hash proves that the reviewer and auditor received the same bytes. It does not prove that the bytes are complete, authentic, or sufficient, so it supplements the acceptance test and never replaces it.

## Attach evidence to the canonical control

The [control mapping](control-framework-mapping-pattern.md) already takes a position that direction matters and coverage is partial by default. Evidence follows the same direction.

Tag each acceptance to the canonical source control. Generate the target-framework evidence index by walking the mapping:

```text
Target requirement
  <- mapping qualifier and target remainder
Canonical source control
  <- accepted evidence rows for the required scope and period
Evidence items in their governed systems of record
```

Do not copy the same evidence row into one index per framework. Copies drift, reviewers reach different decisions about the same item, and an expired item remains green in whichever spreadsheet nobody opened.

Reuse fails honestly in two ways:

- The mapping is Partial and the evidence supports only the shared portion. The target remainder needs separate evidence.
- The mapping claims Full, but the accepted item cannot support the target's scope or period. That is evidence that the mapping is too generous. Downgrade the mapping or narrow the assertion.

The second is the valuable failure. It turns an audit surprise into maintenance of the source artifact that made the unsupported promise.

## Scope and period are part of the evidence

Evidence without scope and period is a sample whose selection method nobody recorded.

For population-based controls, record the authority that supplied the denominator and reconcile the evidence population against it. An access-review completion report sourced from the review tool cannot prove that every privileged account was reviewed if the tool only knows about accounts imported into it. The denominator comes from an authority independent of the process being tested, following the same rule the [metrics catalog](../metrics/security-metrics-catalog.md) applies to coverage measures.

For operating-effectiveness claims, the period must be visible in the evidence or reproducible from its provenance. Twelve monthly tickets can support a twelve-month claim if the population is complete and the procedure is consistent. One year-end screenshot cannot.

When scope or period is short, use Partial and write the remainder. Do not repair the row with prose about what probably happened outside the evidence window.

## Freshness is event-driven

Evidence does not all expire annually. A policy may remain current until its owner, requirement, or approval changes. A configuration export may be stale after the next deployment. A quarterly review expires when the next quarter closes without a replacement.

Use the event that invalidates the acceptance decision:

- The control design, system, owner, or in-scope population changes.
- The evidence-producing query or tool changes.
- A linked exception is granted, renewed, or expanded.
- A framework revision changes the target remainder.
- A public or contractual assertion changes the required scope.
- The next required operating period closes.

Keep a date where the event is calendar-bound, and keep the event where it is not. "Review annually" is not a freshness rule for evidence invalidated by every production release.

## Exceptions and failures stay visible

An active exception inside the asserted scope prevents full acceptance unless the assertion explicitly excludes it. The evidence item may still be accepted as Partial, with the [exception record](../risk-management/security-exception-record.md) named as the remainder. It may not be accepted as though the excepted population were absent.

Failed-control evidence is still evidence. If a trustworthy test demonstrates that the control did not operate, accept the item as sufficient evidence for the tested assertion and record a negative evidence conclusion. Send the resulting issue to the remediation backlog, and put the consequence on the [risk register](../risk-management/risk-register-pattern.md) where warranted. Rejecting a failed test because it does not support a positive outcome destroys the audit trail precisely when it becomes useful.

## Derived views

Maintain one register and generate the views its consumers need.

- **Per-target audit index:** target requirement, mapping qualifier, accepted source-control evidence, remainder, and evidence location.
- **Pending acceptance queue:** received items with no assurance decision, sorted by the assertion date they block.
- **Freshness queue:** accepted and partial decisions approaching their date or event trigger.
- **Unsupported assertions:** target controls, customer commitments, or board claims with no current accepted evidence.
- **Exception conflicts:** accepted assertions whose scope intersects an active exception.

The per-target audit index is a deliverable. It is not a separately maintained artifact. If it can disagree with the register, it will.

## Worked rows

Ambervale examples below are fictional and show the decisions at a width a page can hold. A real register also carries location, provenance, custodian, validation method, evidence conclusion, and reviewer.

| Acceptance ID | Evidence item                                               | Canonical control and assertion                                                                | Scope and period                                                              | Decision | Remainder / link                                                                                                                                  |
| ------------- | ----------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CAE-041`     | `EVD-2026-118`, completed access-review export              | `IAM-AC-07`, all standing production privileged access was reviewed and dispositions completed | Privileged human and non-human identities in production, 2026 Q2              | Accepted | None; population reconciled to the identity provider and cloud account inventories                                                                |
| `CAE-052`     | `EVD-2026-143`, phishing-resistant MFA configuration export | `IAM-STD-04`, privileged roles require phishing-resistant MFA                                  | Production administrator roles at 2026-06-30                                  | Partial  | AccountView Classic administrator role excluded by `EXC-2026-031`; assertion cannot be reused as Full                                             |
| `CAE-063`     | `EVD-2026-167`, shutdown drill record                       | `AI-OPS-04`, every live agentic system has a tested shutdown within the required quarter       | `AI-012`, 2026 Q2                                                             | Accepted | Supports the `AI-012` row and board indicator for this system only; it does not establish coverage of the full agentic population                 |
| `CAE-071`     | `EVD-2026-181`, query-audit retrieval test                  | `LOG-07`, record-level query evidence can be retrieved inside the response target              | AccountView Classic, test performed 2026-07-08 against a date 10 days earlier | Partial  | Retrieval succeeded, but the 14-day source window remains shorter than the program's dwell-time requirement; linked to `EV-07` and risk treatment |

Three distinctions are doing the work.

`CAE-041` is Accepted because its population was reconciled to authorities independent of the review tool. The completion report by itself would have been Partial.

`CAE-052` does not let a valid exception disappear from an audit assertion. The item is useful and the overall claim is still bounded.

`CAE-063` prevents one successful AI shutdown drill from becoming a statement about every agentic system. The [AI system register](../ai-governance/ai-system-register-pattern.md) supplies the population; the drill record supplies evidence for one member of it.

`CAE-071` connects without duplicating the [evidence readiness register](../incident-response/evidence-readiness-register.md). That register says which incident question the source can answer and over what window. This acceptance row says what the retrieval test proves about a control assertion. The same test can participate in both artifacts because the artifacts make different decisions.

## Operating rhythm

Make acceptance part of normal evidence production, not an audit-season cleanup.

- On receipt, verify readability and provenance, then leave the assurance decision Pending until the acceptance test is complete.
- Before an audit, customer answer, certification assertion, or board claim is issued, query for current accepted evidence and visible remainders.
- On every freshness trigger, expire the affected acceptance rows automatically or by a named review task. Do not leave them Accepted while waiting for replacements.
- Quarterly, sample accepted rows and reperform the recorded validation. A reviewer checking that the columns are filled is reviewing the register, not the evidence.
- On framework revision, update the mapping first. Re-evaluate evidence only where the target remainder changed.

The reviewer should be independent of the evidence producer for assertions that support certification, public claims, or executive reporting. A solo program may not have structural independence available. In that case, record the overlap and obtain a second review for the small subset of claims with external consequence rather than pretending separation exists across the whole register.

## Minimum viable register

Start with the controls behind three kinds of statements:

1. An active certification or audit assertion.
2. A customer, contractual, or public commitment.
3. A board indicator or crown-jewel control claim.

Do not begin by indexing every policy and screenshot in the organization. Twenty current acceptance rows protecting statements someone outside the security team relies on are more useful than five hundred received files nobody evaluated.

A spreadsheet is sufficient. Store metadata and governed references, keep collection and assurance in separate columns, and generate the first per-target view with a filter or join. Add workflow tooling only when the number of active decisions makes the spreadsheet unreliable.

## Where this connects

- The [control mapping](control-framework-mapping-pattern.md) supplies the canonical control, the target relationship, and the remainder that evidence reuse must satisfy. A failed reuse test can require the mapping to be downgraded.
- The [regulatory applicability register](regulatory-applicability-register.md) names the obligations and voluntary commitments whose evidence must stay current. Its Evidence column should cite acceptance IDs rather than invent a parallel index.
- The [customer assurance package](../customer-assurance/assurance-package-pattern.md) may assert a control only as confidently as the current acceptance decision and mapping permit. Partial evidence produces a qualified answer, not an aspirational yes.
- The [AI system register](../ai-governance/ai-system-register-pattern.md) supplies the use-case population and operating facts. A row in that inventory is not by itself proof that a control operated; drills, tests, and reconciliations receive their own acceptance decisions here.
- The [evidence readiness register](../incident-response/evidence-readiness-register.md) governs what can prove incident facts. Its retrieval tests can also support logging and response control assertions without turning either register into the other.
- The [risk register](../risk-management/risk-register-pattern.md) receives the consequence of an unsupported or failed control assertion. This register records the evidence judgment, not the risk treatment.
- The [quarterly board update](../board-reporting/quarterly-security-update-template.md) consumes audit posture and selected control indicators. A board claim with no current accepted evidence appears in the unsupported-assertions view before it reaches the deck.
- The [security exception record](../risk-management/security-exception-record.md) bounds claims that intersect a deliberate departure. Its compensating-control verification dates should cite accepted evidence rows.
- The [decision rights register](../governance/decision-rights-register.md) names who may accept evidence supporting a certification, public statement, customer commitment, or executive claim, and who provides the second review where independence is required.

## Common failure modes

1. **Received means accepted.** The request tracker turns green when a file arrives, and nobody records whether it supports the claim.
2. **One row per file.** A document accepted for one assertion is silently reused for every topically related control.
3. **Framework-specific copies.** The same item lives in several audit indexes with different freshness and review decisions. Attach it to the canonical control and derive the views.
4. **No scope or period.** A screenshot and a policy are used to support a year of operating effectiveness across an unnamed population.
5. **Design evidence treated as operation.** The procedure says the control should run, so it is accepted as proof that the control ran.
6. **Partial with no remainder.** The row uses a cautious label and makes an unqualified claim.
7. **Failed tests rejected.** Evidence that shows a control did not operate is discarded because it cannot support the desired conclusion.
8. **Evergreen acceptance.** The system, query, population, or exception changes and the row stays Accepted until the next audit finds it.
9. **Exceptions outside the evidence view.** A control is waived for part of the scope and the audit index still claims Full coverage.
10. **Evidence copied into the register.** Sensitive exports accumulate in a spreadsheet or shared folder that has weaker access, retention, and preservation controls than their source systems.

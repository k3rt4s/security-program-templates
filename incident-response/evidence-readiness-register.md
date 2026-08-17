# Evidence readiness register

A working pattern for the standing record of what an organization could prove after an incident, opinionated about retention measured against dwell time and what an unanswerable question costs.

Every determination in this repository rests on a sentence that nobody writes down in advance. The [materiality determination record](materiality-determination-record.md) rules out bulk export of the customer table on the basis of egress and database audit logs "complete for the window", and that one line carries the conclusion: 3,180 affected accounts rather than 200,000. It is available only to an organization whose egress logging existed, covered the window, was still retained when someone went looking, and could be produced by a person who was reachable. Which of those four were true is settled long before the incident, and almost no program can say which.

This is a readiness artifact, not legal advice. Whether a given absence of evidence obliges a notification is a legal judgment made with counsel against the duties that bind you. What this document describes is the record that determines whether counsel has anything to work with.

## What this is, and what it is not

It is the standing list of the evidence sources an incident would be reconstructed from, each with a named owner, a retention window, a statement of whether the organization has the right to obtain it and how long that takes, and the incident question it can answer.

It is not a logging standard, not a SIEM source inventory, not the incident-response plan, and not a chain-of-custody procedure. A source inventory enumerates what is collected; this one selects and states what each source could prove. That is the same distinction the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md) draws against a configuration database, and it fails the same way when ignored: four hundred accurately catalogued sources cannot answer the single question this exists to answer, because completeness and sufficiency are different properties.

It is also not an argument for collecting more. Most of what this produces are retention, access, and contractual findings against sources already being collected. Those are cheaper to fix than anything new, and they are the ones that decide the outcome.

## Scope it by the crown jewels, not by the estate

An evidence register built across everything is a logging inventory with a new column, and it will be maintained for one quarter.

Build it against the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md), one section per capability, following the same trace. That bounds the work, it puts the shared dependencies in scope automatically, which is where the evidence gaps concentrate for the same reason the exposure does, and it inherits a decision the business has already made rather than reopening it. A capability that was not worth tracing is not worth a preservation plan.

## The test is whether a question can be answered, not whether a source is collected

Rows are sources. The completeness test is a small set of questions, and the register only works when both exist and are read against each other.

Four questions decide the size of an incident. Each is answered from evidence, or it is answered by presumption.

| Question                                        | Answered from                                                 | What it decides                                           |
| ----------------------------------------------- | ------------------------------------------------------------- | --------------------------------------------------------- |
| Who accessed what, and from where               | Authentication, authorization, and administrative action logs | Whether unauthorized access is established at all         |
| What left the environment, and how much         | Egress and network flow, database audit, data loss prevention | Whether this is an access incident or an exfiltration one |
| How long were they in, and did anything persist | Endpoint telemetry, configuration and identity change history | The window every other question has to be asked across    |
| Which records, specifically                     | Application-level query and object-access audit               | The number that goes in a notification letter             |

The fourth is the one most programs cannot answer and the only one that produces a defensible population. Infrastructure telemetry establishes that a session occurred; only application-level audit says which customers it touched. An organization with excellent network logging and no query audit can prove an intrusion happened and cannot bound it, which is the worst of both positions.

For each crown-jewel capability, take the four questions and name the source that answers each one and the window it can answer across. A cell you cannot fill is the finding, and producing those cells is the point of the exercise. Do it on paper before the incident, because the first hour is the worst time to discover which of the four you are blind to.

## Retention is the whole document

Retention windows are set by storage cost and then described as policy. Ninety days is a price point, and thirty days is a cheaper one.

The number to compare a window against is not a compliance minimum. It is how far back an investigation would have to look, which is dwell time, and the honest source for that is your own postmortems. The worked incident's [postmortem](blameless-postmortem-template.md) timeline opens with a credential-stuffing attempt at 2026-03-03 02:11 that was "observed in retrospective log review, not detected at the time", a day and a half before discovery. Every confirmed fact in the materiality record was recovered from a window that happened to still be open.

Intrusions that go unnoticed for months are ordinary rather than exotic, and against a thirty-day window each of the four questions above resolves to Unknown. Not through any failure of the investigation, and not for want of effort during the incident: the answer was determined at procurement, by an arithmetic nobody performed out loud.

So record, per source, the retention window and the earliest date it can currently answer for. Compare that against the longest dwell in your own incident history rather than the average, because the average incident is not the one that ends up in front of a regulator. Where the two do not meet, that is a funded change or a row on the [risk register](../risk-management/risk-register-pattern.md) with the exposure stated in these terms, not a gap to leave in a table.

One qualifier worth writing into the row rather than assuming: a source is complete for a window only if it was actually collecting throughout it. An agent that stopped reporting, a forwarder that silently dropped, or a system onboarded halfway through produces a window that looks intact and is not. Complete for the window is a claim about coverage, and it is worth being able to demonstrate rather than assert.

## Unknown is the expensive answer

The consequential thing about missing evidence is that it does not leave the question open. It decides it against you.

Many US state breach statutes define a reportable breach in terms of unauthorized access to personal information that could reasonably result in harm to the individual. Where unauthorized access is established and scope is not, counsel will commonly advise proceeding on the assumption of harm rather than resting on an absence of evidence, because absence of evidence is not evidence of absence and no regulator is obliged to read it as one. The threshold, the presumption, and how much unrebutted uncertainty is tolerable all vary by jurisdiction and by the duty in question, so the position here is the shape of the argument rather than the answer, and the answer is counsel's against the rows in your own regulatory register. The missing log therefore does not save a notification, it enlarges one, from the population that was demonstrably reached to the population that could have been.

In the worked incident that is the distance between 3,180 records and roughly 200,000. Notification cost, credit monitoring, and the litigation exposure that follows a six-figure notification exceed the retention line item by orders of magnitude, and the comparison comes out the same way nearly every time it is made.

Make that comparison explicitly at the point the retention decision is taken, because it is the only framing in which the decision is decidable. Retention is not a logging expense. It is the price of being able to bound a population, and the organization is buying that whether or not it knows it.

## Collection rights, the column nobody has

Retention answers whether the evidence still exists. Collection rights answer whether you can obtain it, and this is the column that surprises people during the incident instead of before it.

- **Vendor-held evidence.** The log exists on a plan tier you do not hold, in a seven-day window, or is producible only through a support request with no committed timeframe. A third party that processes your data owes you production on a deadline and in a usable format only if a contract says so. That clause is negotiated at contracting or it is not available at all, and the tier that decides whether it is worth negotiating is in the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md). A vendor holding privileged access into your environment can create evidence about your incident that you have no right to read.
- **Employee endpoints and personal devices.** The most common call is not a ransomware note, it is a departing employee who emailed files to themselves or walked out with a box. Whether you may lawfully examine the device, the personal mailbox, or the browsing history depends on the acceptable-use terms accepted in advance and on the jurisdiction, and in several jurisdictions employee monitoring is constrained regardless of what the policy says.
- **Cross-border and key custody.** Moving evidence to a forensic provider is a data transfer, and data encrypted under a customer-held or vendor-held key is not readable by you no matter who retains it.

Record the right, its basis, meaning the contract clause, the plan tier, or the policy that grants it, and the observed lead time to obtain it. A source that arrives in five business days is not available to a 72-hour clock, and the row should say so beside the clock it fails. Those clocks come from the [regulatory applicability register](../compliance/regulatory-applicability-register.md), which is where the deadlines are already enumerated.

## Preservation is not legal hold, and the difference costs the case

Two authorities per source, and they are rarely the same person: who can suspend the expiry, and who can produce an extract.

Preservation fires on suspicion. It happens at incident declaration, before anyone knows whether this is a breach, and it consists of cheap reversible acts: suspend the retention job, snapshot the volume, export the window, freeze the mailbox, pause the auto-expiry on the incident channel. A legal hold is a different instrument. It attaches on reasonable anticipation of litigation, it is a legal judgment made by counsel, and it arrives later by design.

The failure mode is waiting for the second before doing the first. The hold is issued in week two against a thirty-day window that rolled on day nine, and the [materiality record](materiality-determination-record.md) notes the same pattern for the incident channels the timeline is reconstructed from. Preservation costs storage and an hour of someone's time, it is defensible even when the incident turns out to be nothing, and nobody has ever been criticized for having preserved too much.

So make preservation a named step inside incident declaration itself, executed against the sources named in this register by the people named in it, and it takes minutes rather than a meeting. That single change is the highest-value thing this document produces.

## The three calls, in order

At the point an incident is a true positive rather than a suspicion, three engagements happen in a fixed order, and none of them is a security decision.

1. **Counsel.** Engaging the forensic provider through counsel is what gives the investigation's work product a privilege argument. Retaining the provider directly generally forfeits it, and that cannot be repaired afterwards by re-papering the engagement.
2. **The cyber insurance carrier.** Notification is typically a condition of coverage, and most policies constrain which counsel and which forensic providers may be retained. Retaining your own before the carrier is engaged is how an organization pays for an investigation it was insured for.
3. **The forensic provider**, engaged by counsel rather than by you.

The order is the entire content. What belongs in this document is only that these three decisions have named holders and named delegates before the incident, and that the criteria for them, meaning what counts as a true positive, are written down where the responders will look. The contact list, the policy numbers, and the engagement letters belong in the incident-response plan and in the carrier's own material, not here. This repository holds no plans.

## Columns

| Column              | Purpose                                                 | Notes                                                               |
| ------------------- | ------------------------------------------------------- | ------------------------------------------------------------------- |
| ID                  | Stable identifier                                       | Never reuse                                                         |
| Source              | The log, telemetry, or record set                       | Named as the people who operate it name it                          |
| Capability served   | Crown-jewel capability, from the inventory              | A source serving none is out of scope until something claims it     |
| Question answered   | Which of the four, and at what granularity              | The column that makes this a register rather than an inventory      |
| Retention           | Window, and the earliest date it can answer for today   | A date, not a policy reference                                      |
| Coverage confidence | Whether collection is demonstrably continuous across it | Complete for the window is a claim, not a default                   |
| Collection right    | Owned, contractual, discretionary, or none              | Cite the clause or plan tier for anything not owned                 |
| Lead time           | Observed time from request to usable extract            | Measured, not quoted. Compare against the tightest clock            |
| Preservation owner  | Who can suspend expiry, with a delegate                 | Reachable out of hours or the row does not function                 |
| Production owner    | Who can produce the extract                             | Frequently a different team with no incident obligation             |
| Last retrieval test | Date, and the measured time it took                     | Untested retrieval is an assumption, exactly like untested recovery |

Two columns carry the weight, and neither is retention. Collection right, because it is the one that cannot be fixed during the incident. And last retrieval test, because every other column is a claim about a system, while that one is the only evidence that the claim is true.

## Worked rows

Ambervale, scoped to `CJ-003`, issue and collect customer invoices, and read against the March 2026 incident.

Seven of the eleven columns, shown at the width a page will hold. Capability served is constant across this extract and comes from the section heading. Coverage confidence and the two owner columns are dropped here for space and are not optional in a real register, since a row with no reachable preservation owner cannot be executed against and a retention window with no coverage claim behind it is the assertion the last failure mode warns about.

| ID      | Source                                   | Question answered                     | Retention                  | Collection right                       | Lead time                | Last retrieval test |
| ------- | ---------------------------------------- | ------------------------------------- | -------------------------- | -------------------------------------- | ------------------------ | ------------------- |
| `EV-01` | Perimeter egress and network flow        | What left, in bulk                    | 90 days                    | Owned                                  | Under 1 hour             | 2026-01-14, 40 min  |
| `EV-02` | Billing database audit log               | What was queried at the table level   | 90 days                    | Owned                                  | 4 hours                  | 2026-01-14, 5 hours |
| `EV-07` | AccountView Classic query audit          | Which customer records, specifically  | 14 days                    | Owned                                  | 2 hours                  | 2025-11-03, 3 hours |
| `EV-09` | Identity provider sign-in and admin logs | Who accessed what, and from where     | 30 days on current tier    | Owned, longer window is a licence tier | Under 1 hour             | 2026-01-14, 20 min  |
| `EV-12` | Endpoint telemetry                       | Persistence and lateral movement      | 30 days, 7 days raw events | Owned                                  | 1 hour                   | Never               |
| `EV-16` | Payment processor `TP-016` access logs   | Third-party access to cardholder flow | Vendor-held, undisclosed   | None. No production clause in contract | 5 business days observed | Never               |

Three rows are doing something worth noticing.

`EV-01` is the row the entire materiality determination rests on. It is unremarkable, it was never anybody's project, and without it the conclusion is Unknown at a population of 200,000.

`EV-07` is the near miss. Fourteen days of retention on the only source that yields a record-level count, against activity that began on 2026-03-03 and was reconstructed on 2026-03-05. The determination cleared the window with twelve days to spare, and the same incident noticed a month later, which is not an unusual interval, produces a notification two orders of magnitude larger. Its last retrieval test is also four months stale, which is how a window that exists on paper turns out to have stopped collecting in week two.

`EV-16` fails on a column that has nothing to do with logging. The evidence exists, it is retained by somebody competent, and the contract gives no right to obtain it on a deadline. Five business days against a 72-hour contractual clock means the register's honest entry for third-party access is not Unknown, it is Unknowable, and the fix is a contract amendment at renewal rather than anything the security team can build.

## Test it by retrieving, not by reviewing

A register reviewed for accuracy is a register of claims that have been read carefully.

Once a quarter, pick two rows and actually retrieve: name a date sixty days back, request the extract through the path the register describes, and record the elapsed time and whether the data was usable. It finds the agent that stopped reporting, the retention setting that was changed during a cost exercise, the export that produces an unparseable format, and the owner who left. Every one of those is invisible to a review and obvious to a retrieval.

This is the same rule the [tabletop exercise pattern](tabletop-exercise-pattern.md) applies when it refuses to let a scenario assume a capability nobody has performed, and the same reason the crown jewel inventory records a recovery test date rather than a recovery plan. An unexercised capability is a belief.

## Where this connects

- The [materiality determination record](materiality-determination-record.md) consumes this directly. Every Confirmed and every Ruled out in its facts ledger traces to a row here, and every Unknown that stays unknown because evidence was unavailable should say so in the ledger rather than reading as an investigation still in progress. That distinction matters to whoever reviews the determination later.
- The [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md) scopes the register and supplies the traces. A dependency appearing under several capabilities carries its evidence gap into all of them at once.
- The [postmortem](blameless-postmortem-template.md) is the honest source of dwell time, and it is where an evidence gap discovered during response becomes an action item. "We could not determine X because Y was not retained" is a finding about the program, not a limitation to note and move past.
- The [tabletop exercise pattern](tabletop-exercise-pattern.md) tests the register under time pressure, and an exercise that asks for a specific extract from a specific source at T+30 finds more than any review of the table will.
- The [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md) determines which vendors are worth an evidence production clause, and any vendor with privileged access holds evidence about incidents in your environment.
- The [regulatory applicability register](../compliance/regulatory-applicability-register.md) supplies the clocks that lead times are measured against, and its evidence column should reference the same rows rather than creating a second set.
- Where the [customer assurance package](../customer-assurance/assurance-package-pattern.md) answers a logging, monitoring, or forensic-capability question, the answer is generated from this register. An assurance answer claiming a capability that no row supports is the reconciliation failure that document describes, in the place it is hardest to defend.
- Authority to trigger preservation across named sources, to attach a legal hold, and to engage outside counsel on an incident are rows in the [decision rights register](../governance/decision-rights-register.md). The criteria for each live here; the holder and delegate live there.
- Coverage of the four questions across crown-jewel capabilities is a board-legible indicator and belongs in the [metrics catalog](../metrics/security-metrics-catalog.md) with its gaming mode recorded, which is that coverage improves when a source is added and improves identically when a capability quietly leaves the crown-jewel list.

## Common failure modes

1. **A logging inventory with a new column.** The register enumerates every ingested source and says nothing about what any of them could prove. Scope it to the crown jewels and organize the test around the four questions.
2. **Retention benchmarked against a compliance minimum.** The window satisfies a standard and is shorter than the dwell time in the organization's own last three incidents. Compare against your incident history, not against a control.
3. **Blind at the record level.** Network and authentication telemetry are excellent, application query audit does not exist, and the incident can be proven but not bounded. That is the gap that turns a scoping question into a mass notification.
4. **Waiting for the legal hold.** Preservation is deferred until counsel issues a hold, by which time the window has rolled. Preservation fires on suspicion at declaration and is a separate, cheaper, reversible act.
5. **Collection rights discovered during the incident.** The vendor's logs, the personal device, or the customer-keyed data turn out to be unobtainable at the moment they are needed. The fix is at contracting and at policy, months earlier.
6. **Lead times quoted rather than measured.** The vendor's stated response time and the observed one differ, and the difference is discovered against a 72-hour clock.
7. **Owners who are teams.** Preservation and production owners are group mailboxes, unreachable at 2am, with no delegate. A named person or the row does not function.
8. **Reviewed but never retrieved.** Every column is accurate as documented and the agent stopped reporting in March. Retrieve two rows a quarter and record the elapsed time.
9. **Complete for the window, asserted.** The phrase that carries a determination is written in from the source's configured retention rather than from evidence that collection was continuous across it.

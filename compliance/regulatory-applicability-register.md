# Regulatory applicability register

A working pattern for the register of obligations that actually bind an organization, opinionated about the unit of registration and about the three sources of obligation that programs track unevenly.

Every organization has a rough sense of which regimes apply to it. Almost none can answer the question that matters during an incident, which is what specific duty is triggered right now, how long the clock is, and whose name is against it. The gap between those two states is this register, and it is the artifact the [materiality determination record](../incident-response/materiality-determination-record.md) assumes exists when it tells you to list every applicable notification clock.

## Register the obligation, not the regulation

The single decision that makes the difference. A register whose rows are named for regulations is an orientation document. A register whose rows are named for duties is an operational instrument.

"We are subject to the general data protection regime" cannot be executed against. It has no trigger, no deadline, and no owner, and during an incident it sends someone to read a statute under time pressure.

The unit is one duty:

> **REG-018.** On becoming aware of a personal data breach affecting EU residents, notify the lead supervisory authority within 72 hours. Owner: General Counsel. Evidence: notification record and the awareness timestamp.

That row is actionable at 3am by someone who has never read the underlying regulation. One regulation typically produces between three and fifteen such rows, and the decomposition is most of the work. It is also where the value is, because the act of decomposing surfaces duties nobody had noticed.

## Three sources of obligation

Programs track the first source diligently, the second unevenly, and the third almost never. All three produce duties with real consequences, and during an incident they are indistinguishable in urgency.

**Statutory and regulatory.** The obvious source. Sector regulators, data protection regimes, securities disclosure, critical infrastructure rules.

**Contractual.** Customer agreements, data processing agreements, and partner contracts routinely impose notification duties measured in hours, considerably tighter than any statute. These arrive through procurement and legal rather than through compliance, so they are frequently absent from the compliance register while being fully enforceable. Register them from the actual executed agreements of the largest customers by revenue, not from the standard template, because the negotiated ones are the tight ones.

**Voluntary and self-imposed.** Certifications you hold, public claims on a trust page, answers given in completed customer security questionnaires, and commitments made in a sales cycle. These have no regulator behind them, which is exactly why they get missed, and the consequence of breaching one is a misrepresentation claim rather than a fine. This category also determines what is not available for exception, which is the connection the [exception record](../risk-management/security-exception-record.md) already names: an exception that makes an existing public statement untrue is a legal question rather than a risk question.

An organization that tracks only the first source will build its incident process around the slowest clocks it faces.

## Columns

| Column              | Purpose                         | Notes                                                                        |
| ------------------- | ------------------------------- | ---------------------------------------------------------------------------- |
| Obligation ID       | Stable identifier               | Never reuse                                                                  |
| Duty                | One sentence, in the imperative | The most important column. Executable by someone who has not read the source |
| Source              | Statute, contract, or voluntary | With the specific citation, contract, or published claim                     |
| Trigger             | The event that starts the clock | Written as an observable fact, not a legal conclusion                        |
| Clock               | Duration and what it runs from  | Duration, not a date. See below                                              |
| Owner               | One person                      | Not "Legal". A person, same convention as the risk register                  |
| Evidence            | What proves the duty was met    | Where it is stored and who can produce it                                    |
| Applicability basis | Why this binds us               | The fact that would have to change for it to stop applying                   |
| Last verified       | Date                            | Obligations change without telling you                                       |

Two of these carry more weight than the rest.

**Trigger, as an observable fact.** "On becoming aware of a personal data breach" is a legal characterization, and during an incident the people watching the clock cannot evaluate it. Translate it into what someone can actually observe, and record both: the legal trigger, and the operational condition that should cause someone to raise their hand. The gap between the two is where notification deadlines are missed, because the incident team was waiting for a determination that nobody had asked them to seek.

**Applicability basis, not just applicability.** Record the fact that makes the row apply: we hold EU resident data, we are a registrant, we serve a covered entity, we published this claim. This is what makes the register maintainable, because reviewing an applicability basis is a fast, factual check, while re-deriving applicability from scratch is a project.

## Duration, never dates

Do not record compliance deadlines as calendar dates. The register dates instantly, and a stale date is worse than no date because someone will act on it.

Record the duration and its starting event. The date is computed at incident time from the actual trigger, which is the only time it is ever correct. A register full of dates is a document that was accurate on the day it was written.

The one exception is a genuine one-time event with a fixed statutory date, such as an initial registration deadline or a regime coming into force. Keep those in a separate horizon list, described below, rather than mixed into the operational rows.

## Applicability is re-derived on business change

An annual review is the wrong cadence, because applicability does not change annually. It changes when the business changes, and the business does not consult the compliance calendar before doing so.

Trigger a re-derivation on any of these, and name the person who is supposed to notice each one:

- Entering a new geography or market.
- Handling a new category of data, particularly health, biometric, financial, or children's data.
- Serving a new customer segment, especially a regulated one. This is the most common route by which obligations arrive, and they arrive contractually rather than statutorily.
- Shipping a product capability that changes what the business does rather than how it does it.
- An acquisition, which imports the acquired entity's full obligation set including its contracts.
- Crossing a threshold: revenue, headcount, user count, transaction volume, or becoming designated under a sector regime.

The point of the list is that each item is something the business does deliberately and announces internally. The register's maintenance problem is not detection, it is that nobody has been told that these announcements are their cue.

## The clock inventory

Produce one derived view from this register and keep it where the incident process can reach it in seconds: every notification duty, sorted tightest first.

```text
Notification clocks, tightest first
  24h   Contractual, top-10 customers by revenue, per executed DPA        Owner: <name>
  24h   Sector regulator early warning, where designated                  Owner: <name>
  72h   Data protection authority, personal data breach                   Owner: <name>
  4 bd  Securities disclosure, after a materiality determination          Owner: <name>
  30d   State breach notification, varies by state and resident count     Owner: <name>
```

This view exists because incident processes get built around the deadline people happen to know, which is usually not the tightest one. The [materiality determination record](../incident-response/materiality-determination-record.md) names this as a failure mode: a team optimizing for the securities timeline while a contractual clock measured in hours expires unnoticed. This inventory is the fix, and it only works if it is derived from the register rather than maintained separately.

## Separate the horizon watchlist

Keep two lists, not one.

The register holds what binds you now. A separate watchlist holds what is coming: regimes in transposition, rules with future application dates, proposals credible enough to plan against. Mixing them is how a register fills with rows that do not yet apply, which trains everyone to skim it, which defeats the one thing it exists for.

Each watchlist entry carries the expected application date, the applicability basis that would bring it into scope, and a decision point: the date by which the organization has to decide whether to prepare. Move the entry into the register when it applies, and not before.

## Where this connects

- The clock inventory feeds the [materiality determination record](../incident-response/materiality-determination-record.md) directly. That document's header block lists applicable clocks; this register is where they come from.
- The voluntary source category determines what cannot be excepted in the [exception record](../risk-management/security-exception-record.md). An exception that falsifies a published claim or a questionnaire answer routes to counsel rather than through the risk process.
- Obligations map to controls in the [control framework mapping](control-framework-mapping-pattern.md), and the evidence column here should reference current acceptance IDs from the [control evidence acceptance register](control-evidence-acceptance-register.md) rather than creating a parallel set or treating a received file as proof.
- Contractual obligations arrive with vendors and customers, so the register's contractual rows and the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md) draw on the same agreements. A vendor acquisition is a reassessment trigger there and a re-derivation trigger here.
- Failure to meet an obligation is a risk on the [risk register](../risk-management/risk-register-pattern.md), written in the standard form. The register of duties is not a risk list.
- The owner column here names who is accountable for a duty. Who is authorized to make the notification, and their delegate, is a decision right and lives in the [decision rights register](../governance/decision-rights-register.md). On a 24-hour clock those are not the same question and cannot share one name.
- Exercise the tightest clocks in a [tabletop](../incident-response/tabletop-exercise-pattern.md). A 24-hour contractual notification duty that has never been rehearsed is a duty in a spreadsheet.

## Common failure modes

1. **Rows named for regulations.** Nothing can be executed against them, so during an incident somebody reads a statute under time pressure. Decompose into duties.
2. **Contractual obligations absent.** They live in procurement and legal, they are frequently the tightest clocks, and the compliance register never learns about them.
3. **Voluntary commitments untracked.** A trust page claim and a questionnaire answer are enforceable in practice. They also silently constrain the exception process.
4. **Calendar dates instead of durations.** The register is accurate on the day it is written and misleading afterward.
5. **Legal triggers with no operational translation.** The incident team cannot evaluate "on becoming aware," so nobody starts the clock.
6. **Annual review cadence.** Applicability changes when the business changes. Attach re-derivation to business events and tell the people who make those decisions that they are the trigger.
7. **Horizon items mixed into the register.** The register fills with things that do not yet apply, and people stop reading it.
8. **No clock inventory.** The register exists, is correct, and is not reachable in the first hour of an incident, which is the only hour it was built for.

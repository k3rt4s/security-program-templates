# Crown jewel inventory

A working pattern for the short list of things a business cannot lose, opinionated about the unit of selection and about where crown jewels are actually found, which is rarely where they are named.

Several artifacts in a security program quietly assume this list already exists. A new risk register is supposed to start from the crown jewels. Access reviews are scoped to crown-jewel systems. A board indicator reports control coverage across them. In practice the list is either absent, or it is a tag applied to a few hundred rows in a configuration database by someone who had to decide quickly, which is the same as absent for every purpose above.

## What this is, and what it is not

It is the enumerated set of business capabilities the organization cannot operate without, each traced down to the systems, data, and third parties it depends on, with a named business owner and a stated tolerable outage.

It is not a configuration management database, not a data map, and not a system catalog. Those enumerate; this one selects and explains. The difference is decisive: a CMDB with forty thousand accurate rows cannot answer a single question this document exists to answer, because completeness and significance are different properties and only one of them can be discovered by scanning.

Where a CMDB exists, this inventory references it rather than duplicating it. Where one does not, do not build one first. The crown jewel list is achievable in weeks and the CMDB is a program.

## The unit is a business capability, not a machine

The choice that determines whether the list is useful, and the one asset tooling makes for you if you let it.

Tools enumerate hosts, because hosts are discoverable. Ask what matters and you get back a list of servers, each of which someone can defend and none of which answers the question. The resulting inventory is complete, accurate, and unable to tell an incident commander whether the thing that just went down matters.

Start from what the business does. Take the capabilities that produce revenue, discharge a legal obligation, or keep a licence or certification, then trace downward to what each depends on. The trace is the artifact; the endpoint alone is not.

> **CJ-003. Issue and collect customer invoices.**
> Depends on: the billing engine, the rate tables, the customer master record, the payment processor, and the identity provider that fronts all four.

Five dependencies, of which one is a third party, one is a data set rather than a system, and one is shared infrastructure nobody had previously listed as business-critical. A host-based inventory names the billing engine and stops.

## The selection test is duration, not importance

Importance is not decidable. Every system has an owner who considers it important, and a list built by asking is a list of everything.

Ask instead how long the business can operate without it before the consequence becomes unacceptable, and make the business owner give a number. Duration is decidable, it is answerable in one conversation, and it produces a short list, because most things that feel critical turn out to be tolerable for a week once someone has to say so out loud.

| Tolerable outage | Treatment                                                                        |
| ---------------- | -------------------------------------------------------------------------------- |
| Hours            | Crown jewel. Full trace, named owners, tested recovery, on the risk register     |
| Days             | Crown jewel if it discharges a legal or contractual obligation, otherwise not    |
| Weeks or more    | Not a crown jewel, regardless of cost, sensitivity, or how strongly anyone feels |

Keep the list short enough that an incident commander can hold it in their head. If it runs past roughly twenty capabilities, the test is being applied too generously and the result is a configuration database with a new column.

## Crown jewels are usually dependencies, not the named system

The observation that makes the trace worth doing rather than skipping to the answer.

The thing people name is the thing they interact with. The thing that actually stops the business is frequently one level down and shared: the identity provider that fronts everything, an internal certificate authority, one rate table maintained by hand, a message broker, a single engineer who is the only person who has ever performed the restore.

Trace two levels down from each capability and stop. One level misses the shared infrastructure, which is where the real exposure sits. Three levels is unbounded and will consume the exercise. Two levels finds nearly everything worth finding, and it terminates.

Then read the traces together rather than separately. A dependency appearing under several capabilities is a concentration finding, structurally identical to concentration across critical vendors, and like that one it is invisible in any single row and belongs on the [risk register](../risk-management/risk-register-pattern.md) as its own entry.

## You cannot build this by asking

A survey of system owners returns everything, weighted by how strongly each owner writes. Build it from sources that already encode significance.

- **Revenue and obligation mapping, with finance rather than IT.** Which capabilities produce the money and which discharge the obligations. This is the only source that reflects the business rather than the estate.
- **The recovery priority order from continuity planning**, where it exists. Someone has already made these judgments under a different name.
- **Incident history.** What has actually caused business impact in the last two years. Reality has a shorter list than any workshop.
- **The escalation test.** What would cause someone to call the chief executive at the weekend. Crude, fast, and surprisingly accurate.

Run one negative sweep as well. Anything tagged production in the configuration database that no capability trace claims is either an undiscovered dependency or a decommissioning candidate, and both are worth knowing. That sweep is usually where the unlisted shared infrastructure surfaces, and the decommissioning half is the cheapest lever in [exposure prioritization](../vulnerability-management/exposure-prioritization-pattern.md), since findings are counted per asset and removing the asset removes them permanently.

## Columns

| Column                | Purpose                                                | Notes                                                |
| --------------------- | ------------------------------------------------------ | ---------------------------------------------------- |
| ID                    | Stable identifier                                      | Never reuse                                          |
| Capability            | What the business does, in the imperative              | Not a system name                                    |
| Tolerable outage      | Duration, set by the business owner                    | The selection test. A number, not a label            |
| Consequence of loss   | What happens after that duration                       | Revenue, obligation, licence, safety                 |
| Business owner        | One named person, outside IT                           | Sets the duration and owns the consequence           |
| Technical owner       | One named person                                       | Owns the trace and keeps it current                  |
| Depends on            | Systems, data sets, and third parties, two levels down | The trace. Third parties reference the vendor record |
| Recovery, last tested | Date, and measured time to recover                     | Untested recovery is an assumption, not a capability |
| Linked risk IDs       | Rows on the risk register                              | The inventory selects; the register scores           |

Two of these carry the weight. The duration, because it is the test. And the recovery test date, because a crown jewel whose recovery has never been performed end to end is a capability the organization believes it has, which is exactly the belief a [tabletop exercise](../incident-response/tabletop-exercise-pattern.md) is designed to interrogate with its no-magic-capabilities rule.

## It has to be reachable during an incident

An inventory that lives in a wiki behind the identity provider is unavailable during precisely the incident where it matters most, and this is not hypothetical, since the identity provider is on most organizations' dependency traces.

Keep a current offline copy where the incident process can reach it, refresh it on a schedule, and include it in the same handling as the incident contact list. The test is whether someone can answer "does this system matter" in the first ten minutes without needing anything that might itself be affected.

## Where this connects

- A new [risk register](../risk-management/risk-register-pattern.md) is seeded from this inventory. Build it in that order: a register assembled in a workshop reflects who argued most persuasively, while one seeded from the traces here reflects what the business cannot lose.
- Access review populations in the [access review pattern](../identity/access-review-pattern.md) are scoped partly to crown-jewel systems, and they resolve here rather than through the risk register.
- Third-party dependencies in a trace are the same relationships tiered in the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md). A third party appearing on a crown-jewel trace is Tier 4 by operational dependency regardless of what it costs.
- Crown-jewel control coverage is a board indicator in the [quarterly board update](../board-reporting/quarterly-security-update-template.md), and it is only as meaningful as this list. Report the denominator alongside it, per the [metrics catalog](../metrics/security-metrics-catalog.md), because a coverage percentage improves when the crown-jewel list quietly shrinks.
- The [evidence readiness register](../incident-response/evidence-readiness-register.md) is scoped by this list and follows the same traces, which is what keeps it from becoming a logging inventory. A shared dependency carries its evidence gap into every capability above it, exactly as it carries its outage.
- Authority to declare a crown-jewel capability degraded, and to invoke its recovery, is a row in the [decision rights register](../governance/decision-rights-register.md).
- Where a capability discharges a legal or contractual obligation, cite the row from the [regulatory applicability register](../compliance/regulatory-applicability-register.md). That is what makes the days tier decidable rather than a matter of opinion.

## Common failure modes

1. **Hosts instead of capabilities.** The list enumerates servers, is accurate, and cannot say what the business would lose. Start from what the business does.
2. **Importance as the test.** Everything is important to somebody, so the list becomes everything. Ask for a tolerable duration and make the business owner give the number.
3. **Stopping at the named system.** The trace ends at the thing people interact with, and the shared dependency that would actually stop the business is never listed. Go two levels down.
4. **Traces read one at a time.** Concentration across capabilities is invisible row by row and is usually the largest finding in the exercise.
5. **Built by survey.** Owners nominate their own systems and the list reflects who wrote most persuasively. Build it from revenue, obligations, recovery order, and incident history.
6. **Too long to hold.** Past roughly twenty capabilities, nobody carries it and the test was applied too generously.
7. **Untested recovery.** The row asserts a recovery capability nobody has performed. Record the date and the measured time, or record that it is untested.
8. **Unreachable when needed.** The inventory sits behind a dependency that appears in its own traces.

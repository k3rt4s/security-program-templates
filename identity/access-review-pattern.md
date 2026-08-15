# Access review and entitlement certification

A working pattern for reviewing who can do what, opinionated about the two choices that decide whether a campaign finds anything: who reviews, and what is small enough to review well.

Access review is the control most likely to be performed diligently and produce nothing. The failure is structural rather than a matter of effort, and it comes from asking the wrong person to make a judgment they have no basis for, about a population too large for anyone to consider carefully. Both are design decisions, and both are usually made by whatever the identity governance tool defaults to.

## What this covers

The design of a recurring certification campaign and the record it produces. Not the joiner, mover, and leaver automation underneath it, which is a different problem solved by different means, and not the entitlement model itself.

The relationship between the two is worth stating up front, because it determines scope. Lifecycle automation is how access is supposed to be correct. Review is how you find out that it is not. A program that reviews everything is compensating for automation it does not trust, at a cost that scales with headcount, and it should fix the automation rather than certify around it.

## The reviewer is usually the wrong person

Manager attestation is the default in almost every tool and it produces approve-all. This is not laziness, and treating it as a training problem is why it persists.

The mechanism is an asymmetry the reviewer cannot escape. A people manager typically does not know what a given entitlement actually grants, because the entitlement is named for a system rather than a capability. They have no way to determine whether it is still needed without asking the person, who will say yes. And the cost of being wrong runs in one direction only: revoking something needed produces a ticket, an interruption, and a visible complaint within hours, while leaving something unneeded produces nothing anyone will ever see. Given that asymmetry, approving everything is the rational response, and a rational response to a badly framed question is not fixed by asking more insistently.

Pick the reviewer who can answer the question "should this person be able to do this," which is a different question from "does this person report to me."

| Population                                                | Reviewer who can actually answer                     | Why                                                                                                                              |
| --------------------------------------------------------- | ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| Standing privileged access                                | The owner of the system it grants privilege over     | Only they know what the role can do and what would break without it                                                              |
| Access to regulated or customer data                      | The data owner, not the requester's manager          | The obligation attaches to the data, so the accountability should too                                                            |
| Access crossing a trust boundary, including third parties | The relationship owner named in the vendor record    | See [vendor tiering](../third-party-risk/vendor-risk-tiering-pattern.md); the manager has no visibility into a vendor's staffing |
| Non-human identities                                      | The owner of the service or integration that uses it | There is no manager, which is why these go unreviewed                                                                            |
| General application access, low sensitivity               | Nobody                                               | Do not review it. Fix the lifecycle automation and spend the attention elsewhere                                                 |

That last row is the one that makes the rest affordable, and it is the row most programs will not write down.

## Review less, and review what can do damage

A campaign that covers every identity against every entitlement on an annual cycle is theatre with an audit trail. The volume guarantees shallow decisions, and shallow decisions across a large population are worse than deep decisions across a small one, because they produce evidence of diligence without the diligence.

Scope by what the access can do, using the same instinct as [vendor tiering](../third-party-risk/vendor-risk-tiering-pattern.md): not who holds it, not what it costs, but what it permits.

- Standing privilege, meaning privilege held permanently rather than requested when needed.
- Access to regulated data, customer data, or the systems on a crown-jewel dependency trace in the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md).
- Access that crosses an organizational boundary: contractors, vendors, partners, and anyone whose departure you would not hear about.
- Non-human identities, covered below.
- Anything granted through an emergency or break-glass path since the last campaign, regardless of what it grants.

Everything outside that set is a lifecycle problem. Reviewing it is a way of feeling thorough while diluting the reviews that matter.

## Non-human identities

The population almost no campaign covers, and usually the majority of principals in the environment. Service accounts, integration credentials, CI tokens, API keys, and the identities that agentic AI systems authenticate with.

They go unreviewed for a structural reason: certification tooling is built around a manager hierarchy and these have no manager. The result is that the accounts with the broadest permissions, the longest lifetimes, and the weakest rotation are the ones no process examines.

Review them against the service that uses them rather than against a person. Four questions, which are the same four the [AI system register](../ai-governance/ai-system-register-pattern.md) asks of an agentic identity:

1. What service uses this, and is that service still running?
2. What permissions does it actually hold, read back from the target system rather than from the request that created it?
3. When was the credential last rotated?
4. Is it unique to one service, or shared? Shared identities make attribution impossible during an incident.

An identity whose owning service cannot be identified is not a review finding to defer. It is an unowned credential, and the correct default is to disable it and see who complains, in a window where you are watching for the complaint.

## Movers, not leavers

Leavers are largely a solved problem, because a departure is an event that somebody notices and that finance and IT both act on. The accumulation happens with internal transfers, where the new access is granted because it is needed and the old access is retained because nobody owns removing it.

Over several moves, a long-tenured employee accumulates the union of every role they have held, which is frequently the broadest access in the organization and belongs to someone nobody thinks of as privileged. Make the transfer an explicit revocation event with a named owner, and measure entitlement change across transfers rather than only the campaign result. If a campaign is finding transfer accumulation, the campaign is doing cleanup that the mover process should have done, at a much higher cost and a year late.

## The campaign record

```text
Campaign ID:        AR-2026-Q2
Scope:              Standing privileged roles and non-human identities in production
Population:         412 entitlement assignments across 47 systems
Opened:             2026-04-06
Decisions due:      2026-04-24
Revocations due:    2026-05-08
Owner:              <name>
```

Fields the record has to carry to function as evidence, and to function as anything at all:

| Field                                                                      | Why it is there                                                                                                    |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Reviewer, by name                                                          | Not the role, not the team. The person who made the judgment                                                       |
| Decision and date                                                          | Keep, revoke, or modify, with the timestamp                                                                        |
| Basis, for a keep decision                                                 | One line. "Still required for X" is enough; a blank basis is an approve-all in disguise                            |
| What was revoked, and when it was revoked                                  | The decision is not the deliverable. The revocation is                                                             |
| Exception reference, where a review said remove and the business said keep | An [exception record](../risk-management/security-exception-record.md), with an expiry, not a note in the campaign |

## Revocation lag is the metric

A campaign that decides to remove 90 entitlements and removes them 60 days later did almost nothing, and it reports as a success in every tool that measures completion.

Measure decision to revocation, not campaign completion. Completion measures whether the reviewers clicked; revocation lag measures whether the access went away, which is the only outcome the control exists to produce. Publish both together, as an activity measure beside an effect measure, in the sense the [metrics catalog](../metrics/security-metrics-catalog.md) uses.

Two supporting numbers worth keeping: the revocation rate, because a campaign that revokes nothing across a privileged population is evidence about the review rather than about the access, and the reversal rate, meaning how often a revocation had to be undone. A reversal rate of zero suggests the reviews were too cautious to remove anything real.

## Where this connects

- Populations to review come from the dependency traces in the [crown jewel inventory](../assets/crown-jewel-inventory-pattern.md), which is also where the shared infrastructure nobody thinks of as business-critical shows up. Access nobody is willing to review is a row on the [risk register](../risk-management/risk-register-pattern.md).
- A keep decision that overrides the review's finding is an [exception record](../risk-management/security-exception-record.md) with an expiry and a named approver, not a comment field. Who may make that override, and who may disable an unowned credential, are rows in the [decision rights register](../governance/decision-rights-register.md); this document says which reviewer is capable of judging a population, which is a separate question from who is authorized to overrule them.
- Non-human identities used by agentic systems are registered in the [AI system register](../ai-governance/ai-system-register-pattern.md), and this campaign is where their scope gets read back and verified against what was intended.
- Third-party accounts reviewed here are the same accounts the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md) expects to be revoked at offboarding. A campaign that finds live credentials for a terminated vendor is reporting an offboarding failure, and that is where the fix belongs.
- The campaign is the evidence behind access control requirements in several frameworks at once, so record it in the [control mapping](../compliance/control-framework-mapping-pattern.md) as a single evidence item mapped outward rather than producing a separate artifact per framework.
- Orphaned account age and standing privileged account count are in the [metrics catalog](../metrics/security-metrics-catalog.md), with the gaming modes that apply to both.

## Common failure modes

1. **Manager attestation at scale.** The reviewer cannot determine what the entitlement grants and pays no cost for approving. Pick a reviewer who can answer the question.
2. **Reviewing everything.** Volume forces shallow decisions and dilutes the reviews that would have found something. Scope by what the access can do.
3. **Non-human identities excluded by default.** The tool has no manager for them, so they are silently out of scope, which leaves the broadest and oldest credentials unexamined.
4. **Decision without revocation.** The campaign closes on decisions made. Measure decision to revocation and treat the revocation as the deliverable.
5. **Keep decisions with no basis.** A blank justification field is approve-all wearing a review's paperwork.
6. **Transfers unhandled.** Accumulation happens at internal moves and the campaign is left to clean it up a year later at far greater cost.
7. **Unowned credentials deferred.** An identity whose owning service cannot be named is treated as research to do later. Disable it while someone is watching for the complaint.
8. **Certifying around broken automation.** The campaign exists because nobody trusts the lifecycle process. Fix the lifecycle process; the review is not a substitute for it.

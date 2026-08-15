# Security exception and risk acceptance record

A working pattern for the record that documents a deliberate departure from policy, opinionated about the discipline that keeps an exception register from becoming permanent.

Every security program accumulates gaps it has decided to live with. The difference between a mature program and an immature one is not the number of gaps, it is whether each one is written down, owned, bounded, and priced. This is the artifact that does that, and it is the operational other half of the Accept treatment on the [risk register](risk-register-pattern.md).

Two words appear throughout and they are not interchangeable. The **record** is the per-decision document described below: one departure, one scope, one approver, one expiry. The **register** is the index of every record, and it is what everyone outside the security team actually consumes, because the questions that get asked are aggregate ones: how many are open, how many are past their second renewal, which ones sit at the executive tier, which expire this quarter. Design the record so the register is a query over the fields rather than a separately maintained summary. The register needs no template of its own; it needs the record's fields to be consistent enough to sort on.

## Exception versus risk acceptance

These two terms get used interchangeably and they are not the same thing. The distinction is worth holding because it changes who approves.

An **exception** is a departure from a stated requirement: a policy, a standard, a control specification, a contractual commitment. The question it answers is one of compliance. Something is required and this system does not do it.

A **risk acceptance** is a decision to carry residual risk without further treatment. The question it answers is one of appetite. Something could go wrong and we are choosing not to spend more to prevent it.

They overlap without being identical. Most exceptions carry an implicit risk acceptance for their duration, which is exactly why an exception approved on compliance grounds alone is a mistake. Not every risk acceptance needs an exception, because a system can be fully policy-compliant and still carry residual risk above appetite.

Keep one register with a Type field rather than two registers. The intake, the approval tiering, the expiry discipline, and the review cadence are the same for both, and splitting them reliably produces two backlogs that no one reconciles. What differs is the approval question, and the record makes that explicit:

- For an exception: what requirement is not being met, for how long, and what reduces the exposure meanwhile.
- For a risk acceptance: what residual risk we are carrying, why the cost of treatment exceeds the exposure, and when we will re-test that judgment.

## The record

```text
Record ID:            EXC-2026-031
Type:                 Exception / Risk acceptance
Title:                No phishing-resistant MFA on the AccountView Classic administrator role
Requirement waived:   IAM-STD-04 §3.2 (phishing-resistant MFA on all privileged roles)
Scope:                AccountView Classic production admin role, 6 named accounts. Does not
                      extend to the customer-facing role or to any other environment.
Linked risk ID:       OPS-014
Requested by:         <name>, Director of Platform Engineering
Accountable owner:    <name>, VP Engineering
Approver:             <name>, Chief Financial Officer (residual 16, executive tier)
Security review:      <name>, CISO
Approved:             2026-01-14
Expires:              2026-07-31
Renewal count:        1
Status:               Active / Expired / Withdrawn / Superseded
```

Every field earns its place, and three of them are the ones that get left out.

**Scope, stated as a boundary rather than a system name.** An exception written against "the legacy portal" expands to fit whatever anyone later calls the legacy portal. Name the accounts, the environments, the interfaces. State what the exception explicitly does not cover, because that sentence is what stops the scope drift that turns one waiver into a category.

**The requirement waived, cited precisely.** Not "our MFA policy" but the clause. If the requirement cannot be cited, one of two things is true and both matter: the policy does not actually say what people believe it says, or there is no policy and this is an undocumented practice being formalized under the wrong instrument.

**Renewal count.** The single most useful number in the register, and the one almost nobody tracks. See below.

## Why the exception exists

Two paragraphs, not two sentences, because this is the section a future reviewer reads when deciding whether the reasoning still holds.

State the business or technical constraint in concrete terms: what was tried, what it would cost to comply, what breaks if compliance is forced now. "It is hard" is not a constraint. "The vendor's authentication module predates SAML support, the upgrade path requires the 4.x migration, and the migration is scheduled for Q3 with the replacement product" is a constraint, and it is also a testable claim that the reviewer at expiry can check.

Then state the alternative that was rejected and why. Exceptions granted without a rejected alternative on the record are usually exceptions nobody looked for a way around.

## Compensating controls

The heart of the record, and the section most often filled with intentions.

Each compensating control is named specifically, mapped to the part of the exposure it reduces, assigned an owner, and marked with the date it was last verified as operating. An untested compensating control does not reduce residual risk, exactly as an untested control does not reduce residual likelihood on the risk register. Carry the same rule in both places or the two artifacts will disagree about the same system.

| Control                                                         | Exposure it reduces                                         | Owner    | Last verified |
| --------------------------------------------------------------- | ----------------------------------------------------------- | -------- | ------------- |
| Admin access restricted to the corporate egress ranges          | Removes credential replay from arbitrary networks           | `<name>` | 2026-04-02    |
| Admin session lifetime cut from 12 hours to 30 minutes          | Narrows the window on a stolen session                      | `<name>` | 2026-04-02    |
| Daily review of all admin actions against a named-approver list | Detection, not prevention; catches misuse next business day | `<name>` | 2026-03-28    |
| Detection rule for authentication to this role from a new ASN   | Detection within minutes for the specific attack path       | `<name>` | 2026-02-19    |

Two disciplines make this table honest. Label each control as preventive or detective and do not let a table of four detective controls be presented as though the gap is closed, because detection means the thing happened. And record verification as a date, not a checkbox, so the reviewer at expiry can see that the last verification predates the exception's second renewal.

## Approval authority

Tier approval to the residual risk the exception leaves behind, scored on the same 5-by-5 rubric the [risk register](risk-register-pattern.md) uses. Using one scale across both artifacts is what lets the exception register be summarized on a board deck without a translation step.

| Residual score | Approval authority                                                             | Maximum duration | Renewal limit |
| -------------- | ------------------------------------------------------------------------------ | ---------------- | ------------- |
| 1 to 4         | Control owner's manager, with security notified                                | 12 months        | 3             |
| 5 to 9         | Security leadership                                                            | 12 months        | 2             |
| 10 to 15       | Executive owning the accountable function                                      | 6 months         | 2             |
| 16 to 25       | Executive plus written notification to the board committee at the next meeting | 3 months         | 1             |

The tiers above are criteria: they say which level of authority a given residual score requires. Who occupies each tier is an assignment, and it belongs in the [decision rights register](../governance/decision-rights-register.md) rather than being written into this table, because a name recorded here goes stale silently when the person changes roles.

The approver is the person who owns the consequence, not the person who owns the system. An engineering leader approving an exception whose realized cost lands on the finance or legal function is the structural failure that makes an exception register meaningless. Security reviews and records; security does not approve its own waivers, because an exception approved by security is a security decision rather than a business one, and the entire point of the instrument is to put the decision where the consequence lands.

## Expiry, and what happens at it

Expiry is mandatory. An exception without an end date is a policy change made by someone without the authority to change policy.

At expiry the exception lapses by default. It does not auto-renew, and the burden at the review sits with whoever wants it to continue, not with security to justify enforcement. Build the calendar so the review lands 30 days before expiry, which is the only way the outcome is a decision rather than an emergency renewal on the last day.

Four outcomes, and naming all four matters because programs that only recognize renew and close will renew:

- **Closed.** The requirement is now met. Record the date and how it was verified.
- **Renewed.** The constraint still holds, verifiably. Increment the renewal count and re-verify every compensating control before the signature, not after.
- **Converted to a risk acceptance.** The constraint is permanent, the policy is right, and the organization is choosing to carry this. This is a different instrument with a different approver and it belongs on the risk register as an Accept row.
- **Escalated.** The constraint still holds but the residual risk has grown past the approver's tier. Re-approve at the higher tier or enforce.

## The three-renewal rule

An exception on its third renewal is not an exception. It is the operating model, described in a document that pretends otherwise, and the register is now hiding the gap rather than tracking it.

At the third renewal, stop and pick one of three answers. The policy is wrong for this class of system and should be amended, in which case amend it and close the exception. The risk is genuinely acceptable in perpetuity, in which case convert it to a risk acceptance with executive sign-off and put it on the risk register where it is visible next to everything else the organization has chosen to carry. Or the remediation has simply never been funded, in which case that is a resourcing decision and it belongs in the [board update](../board-reporting/quarterly-security-update-template.md) as an explicit ask rather than in the exception register as a fourth signature.

The count of exceptions past their second renewal is a better indicator of program health than the total count, and it is one of the few security metrics a board reads correctly without training.

## What cannot be excepted

Publish a short list of requirements that are not available for exception, and keep it short enough to be credible. Three or four items, not thirty.

The test for the list is not severity, it is whether granting the exception would make something else the organization has said untrue. A control that a customer contract commits to, a control named in a regulatory filing or an audit assertion, and a control claimed in a completed customer security questionnaire are all in that category: the exception does not just carry risk, it makes an existing statement false, and that is a legal question rather than a risk question. Route those to counsel instead of to the exception process.

Knowing which controls are in that category requires knowing what the organization has committed to, which is not something to reconstruct while an exception request is waiting. The contractual and voluntary rows of the [regulatory applicability register](../compliance/regulatory-applicability-register.md) are that list.

Everything else should be exceptable in principle. A non-exceptable list that runs long gets routed around rather than obeyed, and undocumented gaps are strictly worse than documented ones.

## Relationship to the other artifacts

- The risk this exception leaves open lives on the [risk register](risk-register-pattern.md), scored with the exception's compensating controls counted as existing controls. The two documents cite each other by ID.
- Exceptions above the executive tier, and the count past their second renewal, appear in the compliance and audit section of the [quarterly board update](../board-reporting/quarterly-security-update-template.md).
- When an incident occurs inside the scope of an active exception, say so plainly in the [postmortem](../incident-response/blameless-postmortem-template.md). The exception is a contributing factor and often a root cause, and a postmortem that omits a live waiver covering the exact failure path is not a complete analysis. It is also the strongest available evidence at the next renewal review.
- An AI system running outside policy on purpose is an exception like any other, not an annotation on the [AI system register](../ai-governance/ai-system-register-pattern.md).
- A vendor onboarded despite failing the assessment its tier requires is an exception, with an expiry and a compensating control, rather than a note in the vendor file. See the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md). This is one of the most common routes by which an undocumented gap enters an otherwise disciplined program, because the commercial pressure arrives with a date attached and the security objection does not.

## Common failure modes

1. **Permanent exceptions.** No expiry, or an expiry that is renewed indefinitely without re-examining the constraint. Track the renewal count and act on the third.
2. **Intentions as compensating controls.** "Additional monitoring will be implemented." Nothing is compensating anything. Only controls that are operating and verified are listed, with the date.
3. **Detection presented as closure.** Four detective controls, no preventive ones, described as though the gap is handled. Label the type and read the table honestly.
4. **Security approving its own waivers.** The approver must own the consequence. Security reviews, records, and reports.
5. **Scope that grows.** An exception written against a vague system name expands to cover whatever people later mean by it. Name accounts, environments, and interfaces, and state what is out of scope.
6. **Exceptions that never reach the risk register.** The gap is documented in a register no one reads outside the security team, so it never appears in the risk picture the executives see.
7. **No lapse discipline.** Expiry passes, nobody reviews, the system keeps running and the record says Expired. That is an undocumented gap wearing the paperwork of a documented one.

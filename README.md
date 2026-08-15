# security-program-templates

Working templates and patterns for the documents a security program needs to actually run: a risk register that says something useful, a board update that earns time on the agenda, an incident postmortem that produces shipped changes, an AI system register that survives its second quarter, a vendor tiering pattern that puts the effort where the exposure is, and a control mapping that auditors trust.

Markdown only. No code. Each document is opinionated about the things that most organizations get wrong the first time they build one, and each ends with a short list of failure modes worth watching for.

## Contents

<!-- BEGIN CONTENTS (auto-generated, do not edit by hand) -->

- [ai-governance/](ai-governance/README.md): Patterns for the inventory of AI systems an organization actually runs, covering the use-case unit of registration, three classes of autonomy, discovery of unapproved tools, and the tested shutdown.
- [assets/](assets/README.md): Patterns for enumerating the business capabilities an organization cannot lose, tracing each to what it depends on, and selecting by tolerable outage rather than by importance.
- [board-reporting/](board-reporting/README.md): Templates for the quarterly security update delivered to a board or board committee, covering structure, indicators, and the discipline around what to ask from the board.
- [compliance/](compliance/README.md): Patterns for mapping security controls across multiple frameworks with honest coverage qualifiers, and for the register of obligations that actually bind an organization.
- [governance/](governance/README.md): Patterns for recording who holds each decision a security program depends on, separating the criteria for a decision from the assignment of it.
- [identity/](identity/README.md): Patterns for access review and entitlement certification, covering who can actually judge an entitlement and why revocation rather than decision is the deliverable.
- [incident-response/](incident-response/README.md): Templates for blameless security incident postmortems, tabletop exercise design and after-action, and the contemporaneous record of a cybersecurity materiality determination.
- [metrics/](metrics/README.md): Patterns for choosing security metrics that change decisions, with each metric's gaming mode and blind spot recorded alongside its definition.
- [risk-management/](risk-management/README.md): Patterns for an enterprise risk register and the security exception record, covering risk statement format, a likelihood and impact rubric anchored to time and money, and the bar for an Accept.
- [third-party-risk/](third-party-risk/README.md): Patterns for tiering third parties by what they can do to you rather than by what you pay them, and for scaling assessment depth, evidence review, and offboarding to that tier.
- [FUTURE_FEATURES.md](FUTURE_FEATURES.md): Candidate documents for this repository, with the reason each is a candidate and the bar it would have to clear to be written.

<!-- END CONTENTS -->

## How these fit together

The documents are individually useful and they are more useful as a set, because a security program is a loop rather than a shelf of artifacts. Each one hands something specific to the next.

The [crown jewel inventory](assets/crown-jewel-inventory-pattern.md) comes first, because most of what follows is scoped against it. It enumerates the business capabilities the organization cannot lose, selected by tolerable outage rather than by importance, and traces each one down to the systems, data, and third parties it actually depends on.

The [risk register](risk-management/risk-register-pattern.md) is the spine. It holds the things that could go wrong, scored honestly, with owners and treatment decisions, and it is seeded from that inventory rather than from a workshop.

When a risk is treated by deliberately not meeting a requirement, the [exception record](risk-management/security-exception-record.md) documents the departure, the compensating controls, the approver, and the expiry. It is the operational other half of the register's Accept treatment, and it cites the register row by ID.

When something goes wrong, the [postmortem](incident-response/blameless-postmortem-template.md) establishes what happened and what changes. If an exception covered the failure path, the postmortem says so, and that is the strongest available evidence at the exception's next renewal review.

Before something goes wrong, the [tabletop exercise pattern](incident-response/tabletop-exercise-pattern.md) is how the response gets examined without waiting for an incident to do the examining. It shares the postmortem's action-item format on purpose, so exercise findings and incident findings compete in one backlog rather than in two.

In parallel and on a different clock, the [materiality determination record](incident-response/materiality-determination-record.md) captures whether the incident was material and how that was decided. Different question, different owner, different audience from the postmortem.

The [AI system register](ai-governance/ai-system-register-pattern.md) inventories what AI is running and who is accountable for it. It is an asset inventory, not a risk list: risks about AI systems are written in the standard form and scored on the risk register like everything else.

The [vendor tiering pattern](third-party-risk/vendor-risk-tiering-pattern.md) decides how hard to look at each third party, so that assessment effort tracks what a vendor can actually do to you. It feeds the register in two directions: a specific vendor failure is a risk row, and concentration across the critical tier is a risk row that belongs to nobody in particular unless the program claims it.

The [access review pattern](identity/access-review-pattern.md) covers who can do what, scoped to the access that can do damage and reviewed by someone capable of judging it, which is rarely the people manager the tooling defaults to.

The [regulatory applicability register](compliance/regulatory-applicability-register.md) records the duties that actually bind the organization, one row per duty rather than per regulation, drawn from statute, contract, and public commitment alike. It is what the materiality record assumes exists when it tells you to list every clock. The [control mapping](compliance/control-framework-mapping-pattern.md) then makes one set of evidence serve several frameworks, so the program is not tested five times for the same control.

Every document above defines criteria for at least one decision: what constitutes it and at what threshold it escalates. None of them says who holds it, on purpose. That is the [decision rights register](governance/decision-rights-register.md), which carries the assignment for the whole set, and its real job is making the empty rows visible, because an authority nobody holds is an absence and absences cannot be seen from inside any single document.

Running underneath all of it, the [metrics catalog](metrics/security-metrics-catalog.md) is where the numbers these artifacts produce get chosen, each one recorded with how it can be made to look good without anything improving and what it stays silent about.

The [quarterly board update](board-reporting/quarterly-security-update-template.md) is where the loop surfaces: top risks from the register, exceptions past their second renewal, material incidents and their determinations, AI inventory coverage, third-party posture, and audit posture from the mapping. The board update is a view over the other artifacts, not a document written from scratch each quarter. When it has to be written from scratch, that is the diagnostic.

## The worked example

The documents share one fictional organization so the examples read as a single program rather than a folder of unrelated illustrations.

**Ambervale Systems, Inc.** is a mid-market, publicly traded provider of billing and customer-portal software to regional utilities. Roughly 2,300 employees. It sells into a regulated buyer base, so it carries SOC 2 Type II, holds ISO/IEC 27001 for its European customers, and runs NIST CSF internally. It is an SEC registrant, which is what makes the materiality question live.

The thread that runs through the templates:

| ID | Artifact | What it is |
| -- | -------- | ---------- |
| `CJ-003` | Crown jewel inventory | Issue and collect customer invoices. Its trace runs through the billing engine, the rate tables, a payment processor, and the identity provider fronting all of them. |
| `OPS-014` | Risk register | Weak authentication on AccountView Classic, the legacy customer portal holding about 200,000 end-customer records. |
| `EXC-2026-031` | Exception record | The waiver for phishing-resistant MFA on that portal's admin role, granted because the platform predates SAML support and the replacement ships in Q3. |
| `SEC-2026-Q1-014` | Postmortem | March 2026: credential stuffing against that exact admin role. The risk materializing through the excepted gap. |
| `MAT-2026-03` | Materiality record | The determination on that incident, concluded not material at confirmed scope, with the reassessment triggers named. |
| `AI-007`, `AI-012` | AI system register | A support-ticket drafting assistant and a billing-dispute agent that can issue account credits. Same vendor, same model, two very different rows. |
| `TP-041` | Vendor tiering | An observability vendor at four figures a month holding a write-scoped token into production. Tier 4, and roughly a hundredth the spend of the facilities contract sitting in Tier 1. |
| `TTX-2026-02` | Tabletop after-action | The exercise that postmortem action item AI-03 called for, run as a third-party compromise scenario. Two of its three forced decisions stalled on authority nobody in the room held. |
| `AR-2026-Q2` | Access review campaign | Standing privileged roles and non-human identities in production. It is where the observability vendor's service account gets its permissions read back for the first time. |
| `REG-018` | Regulatory applicability register | The 72-hour data protection notification duty, one row among several, sitting behind the tighter contractual clock that the materiality record warns about. |
| `DR-014` | Decision rights register | Authority to take the customer portal offline. It sits beside the row that was empty until the exercise found it: revoking a vendor's production credential. |

The [metrics catalog](metrics/security-metrics-catalog.md) has no row here on purpose. It is a reference the others draw from rather than an artifact an organization produces one instance of, so it has nothing to carry an Ambervale identifier.

Ambervale is invented, as is every name, figure, and system in the examples. Any resemblance to a real organization is coincidental.

## How to use these

Copy a template into your wiki, document store, or repo and adapt it. The text is intentionally written so the structure stays useful after you cut the parts that do not fit your organization. The footers on each document call out the things that most often break a first implementation, so they are worth keeping even after the rest is rewritten.

One housekeeping note for anyone reading the source: the `CONTENTS` blocks in this README and in each folder README are maintained by a script that lives outside this repository, which is why they are marked as generated. Edit the first line of a document rather than the manifest entry that quotes it, and the manifest will catch up on the next run.

## What this is, and what it is not

These are templates and patterns. They are not policies, not standards, not control libraries, not legal advice, and not a security program in a box. They are the shape of the artifacts a security program produces, and the judgment lives in how they get filled in for a specific context.

Their provenance differs, and it is worth being straight about which is which. The risk register, exception record, postmortem, board update, vendor tiering pattern, and control mapping are long-established artifacts, and what is opinionated about them is distilled from running them in real organizations and watching what works and what does not. The AI system register and the materiality determination record describe practices that are still forming: the disclosure obligation they answer to is recent, and almost nobody has a decade of watching an AI inventory succeed or fail. Those two are reasoned from current practice, current regulatory expectations, and the failure patterns visible so far, and they should be read as a considered starting position rather than as settled practice.

## License

CC BY 4.0. Use, adapt, redistribute. Attribution required. See `LICENSE`.

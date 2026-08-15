# Future features

Candidate documents for this repository, with the reason each is a candidate and the bar it would have to clear to be written.

This file exists so that candidates are recorded with their reasoning rather than rediscovered, and so that the ones deliberately not written stay deliberately not written. It does not restate what currently ships; the [Contents](README.md#contents) manifest does that and is kept current by script.

## The bar

A document belongs here only if it clears all four of these. Most candidate topics fail on the third.

1. **It is an artifact a security program produces**, not a checklist, a policy, a control library, or a tool evaluation. The repository is about the shape of documents that get written, reviewed, and argued over.
2. **It hands something to, or takes something from, the existing set.** The value of this collection is that it is a loop rather than a folder. A document with no edge into the others is a candidate for somewhere else.
3. **There is a defensible opinion to hold about it**, one that a reader could disagree with. Good content on the topic being abundant is not a reason to skip it, and being abundant and undifferentiated is not a reason to add to it. The test is whether there is a position worth taking that the freely available versions do not take.
4. **The opinion survives contact with a real organization.** A position that only works at a company with a mature program and a dedicated team is a blog post, not a template.

## Candidates

Topics that have cleared the bar above and are waiting to be written. Nothing sits here at present. Everything assessed so far has either shipped or is recorded below with the reason it did not.

## Assessed and held

Topics that were put through the bar and did not ship. The reasoning is recorded so the judgment is not repeated, and so a later session can see what would have to change for the verdict to change. Everything else that was assessed is in the shipped list below.

Nothing sits here at present.

## Notes on vintage

The demand signals behind these candidates were gathered in August 2026 from published survey and practitioner reporting. Signals age. Anything still sitting in this file in a year should be re-checked against current reporting before it is written, rather than written on the strength of a note left here.

## Shipped

Items are annotated here when they ship, rather than deleted, so the reasoning stays with the outcome.

- **AI system register** — shipped, [ai-governance/](ai-governance/README.md).
- **Materiality determination record** — shipped, [incident-response/](incident-response/README.md).
- **Security exception and risk acceptance record** — shipped, [risk-management/](risk-management/README.md).
- **Vendor risk tiering** — shipped, [third-party-risk/](third-party-risk/README.md).
- **Exposure prioritization** — shipped, [vulnerability-management/](vulnerability-management/README.md). Held originally as a remediation SLA, which failed the first test because an SLA is a standard and this repository holds none. Reopened 2026-08-15 and shipped as the reframe recorded here, rather than by changing the repository's scope statement. The reframe turned out to be the stronger document anyway: the useful content is the arithmetic nobody does out loud, comparing finding arrival against closure to establish whether prioritization allocates a shortfall or solves one, and it almost always allocates. Also that the tail below the cut line is a decision rather than an oversight, and that grouping findings by root cause is the only move that reduces arrival rather than closure.
- **Customer assurance package** — shipped, [customer-assurance/](customer-assurance/README.md). Held originally as a weak pass, on the grounds that the topic drags toward tooling comparison and the surrounding advice is saturated. Reopened 2026-08-15. Built around the one angle that made it a security artifact rather than a sales one: every answer is a durable commitment that constrains the exception process afterwards, so answers are generated from the control mapping and the other registers rather than authored under deal pressure, and a question with no source artifact is a finding rather than something to answer from memory.
- **Crown jewel inventory** — shipped, [assets/](assets/README.md). Not from the research pass. Found by walking the set for artifacts that are assumed but never defined, the same method that promoted the regulatory applicability register. Five documents leaned on a crown-jewel list, and two of them pointed at each other for it: the access review pattern sourced its populations from the risk register, while the risk register treated the list as an input it already had. Neither defined it. The positions it takes are that the unit is a business capability rather than a machine, that the selection test is tolerable outage rather than importance because importance is not decidable, and that crown jewels are usually a shared dependency one level below the system anyone would name.
- **Security program charter** — shipped as something else, [governance/decision-rights-register.md](governance/decision-rights-register.md). Held originally because its decision-rights framing appeared to duplicate the exception record's approval tiers. Checking the set showed a larger problem: decision rights are partially specified in eight documents, and two of them independently report finding an authority nobody held. The resolution is to split criteria from assignment. Each domain document keeps the criteria, meaning what constitutes the decision and at what threshold it escalates, because that is domain knowledge and is useless out of context. One document carries the assignment, because that is an organizational fact that changes when people change and must be consistent across domains. The decisive argument for a single assignment table is not tidiness: an authority nobody holds is an absence, and absences are invisible from inside any one document. That also renamed the candidate. A charter as a genre is mostly decoration; the part that clears the bar is the decision-rights register alone.
- **Access review and entitlement certification** — shipped, [identity/](identity/README.md). Cleared the third test on the position that the reviewer is usually the wrong person: a people manager cannot tell what an entitlement grants and pays a cost for revoking but none for approving, so approve-all is the rational response to a badly framed question.
- **Regulatory applicability register** — shipped, [compliance/](compliance/README.md). Cleared the second test decisively by closing a hole this repository had created: the materiality record instructs you to list every applicable notification clock without saying how you would know them.
- **Security metrics catalog** — shipped, [metrics/](metrics/README.md). The recorded concern was that it would become a longer version of the board update's indicator table. Resolved by splitting the two jobs: the catalog is the source and carries every metric's gaming mode and blind spot, and the board section is the selection problem, which is the harder half. The board template now links into the catalog instead of restating it.
- **Tabletop exercise design and after-action** — shipped, [incident-response/](incident-response/README.md). The open question recorded here before writing was whether the after-action duplicated the postmortem enough to be a section of it instead. Resolved as a separate document: the two differ in whether the facts were discovered or authored, and half of the exercise document is design work that has no postmortem analogue, since reality does the design. What they share is the destination, so the after-action reuses the postmortem's action-item table verbatim rather than inventing a second format. One format, two documents, one backlog.

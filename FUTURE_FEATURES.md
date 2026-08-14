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

### Tabletop exercise design and after-action

The stronger of the two remaining candidates. It has a clean edge into the existing set: the exercise produces an after-action item list that flows into the same backlog the [postmortem](incident-response/blameless-postmortem-template.md) feeds, and a drilled but never exercised runbook is one of the failure modes that postmortem template already names.

The position worth taking, and the one most published scenario packs do not: injects should target decisions rather than technical steps. Who authorizes a ransom payment, who decides to take the product offline, who calls the regulator and on what threshold, who tells the customer. The technical response is usually the part an organization is best at, and the decision path is where an exercise finds real gaps. A second position: the exercise's deliverable is the after-action list with owners and dates, so an exercise that produces a satisfied feeling and no assigned items did not happen.

Open question before writing it: whether the after-action format is distinct enough from the postmortem's action-item table to justify its own document, or whether this should be a section of the postmortem template instead. Resolve that first, because writing it as a separate document and discovering it duplicates half of another one is the likelier failure than writing it badly.

### Security metrics catalog

Weaker as a standalone, and it needs care. The [board update](board-reporting/quarterly-security-update-template.md) already carries an indicator section with nine examples, and a catalog risks becoming a longer version of a table that is already there.

What would justify it: every metric shipping with two fields nobody publishes, its gaming mode and its blind spot. How the number is made to look good without the underlying thing improving, and what it is silent about. A patch-latency metric improves when the asset inventory shrinks. A phishing click rate improves when the simulations get easier. A mean-time-to-detect that counts only incidents that were detected is silent about the ones that were not, which is the population that matters. A catalog of forty metrics with those two fields filled in honestly would be genuinely scarce; the same catalog without them exists a hundred times over.

If written, it should be structured so the board template's indicator section links into it rather than duplicating it.

## Recorded, not assessed

These came out of the same research pass and no decision has been taken on any of them. Listed so the reasoning is not rediscovered from scratch.

- **Vulnerability and exposure management SLA.** The standard that exceptions get written against, so it has an edge into the exception record. Risk of being a policy rather than an artifact, which would fail the first test.
- **Access review and entitlement certification.** Authorization logic scattered across applications, with no central view of who can access what, is a recurring audit finding. Needs an opinion beyond "do access reviews" to clear the third test.
- **Regulatory applicability register.** Which regimes bind us, when they bite, and who owns each. Genuinely useful and it dates fast, so it would have to be a pattern for building one rather than a calendar of deadlines.
- **Customer assurance package.** The questionnaire response burden is real and large. It is closer to a sales enablement artifact than to a security program artifact, so it is the weakest fit against the first test.
- **Security program charter, or a first-90-days plan.** An artifact a new security leader produces, with a clean opinion available about what belongs in it. No edge into the current loop, which is the open question.

## Notes on vintage

The demand signals behind these candidates were gathered in August 2026 from published survey and practitioner reporting. Signals age. Anything still sitting in this file in a year should be re-checked against current reporting before it is written, rather than written on the strength of a note left here.

## Shipped

Items are annotated here when they ship, rather than deleted, so the reasoning stays with the outcome.

- **AI system register** — shipped, [ai-governance/](ai-governance/README.md).
- **Materiality determination record** — shipped, [incident-response/](incident-response/README.md).
- **Security exception and risk acceptance record** — shipped, [risk-management/](risk-management/README.md).
- **Vendor risk tiering** — shipped, [third-party-risk/](third-party-risk/README.md).

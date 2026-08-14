# Quarterly board security update

A working template for the quarterly security update that goes to a board or board committee (often Audit, sometimes Risk, occasionally a standing Technology committee). Designed for a director-level audience that has 15 to 30 minutes and wants to leave the meeting with three things: an honest read on the program, a short list of decisions you need from them, and confidence that the operator is not surprised by what is in the data.

## Length and structure

Aim for an 8 to 12 page deck plus a 1-page executive summary. Anything longer competes for attention you do not have; anything shorter looks underbaked for a quarterly cadence. The order below puts the items that change every quarter at the front and the items that change rarely at the back.

## Section 1: Executive summary (one page)

A single page. Three short paragraphs.

- **Posture.** One sentence on whether the overall risk posture is improving, holding, or declining, and why.
- **Top three risks right now.** Bullet list, one line each, with movement indicator (new, increased, decreased, holding). These are tied to specific rows in the risk register.
- **What we need from the board this quarter.** Bullet list of explicit asks: decisions, funding, policy approvals, escalations. Three at most. If there are none, say so explicitly; do not pad.

The executive summary is the page that gets forwarded. Write it last, after the rest of the deck is settled.

## Section 2: Key risk and performance indicators

A small dashboard, no more than 6 to 10 indicators, mixing leading and lagging measures. Avoid vanity metrics ("number of emails scanned"). Each indicator has:

- A definition that fits on one line.
- A current value and the value from the prior two quarters, so trend is visible.
- A target or threshold, and a color (green / yellow / red) tied to that threshold, not to gut feel.
- An owner.

Examples of indicators that survive board scrutiny:

| Indicator                      | Definition                                                                | Type    |
| ------------------------------ | ------------------------------------------------------------------------- | ------- |
| Critical patch latency         | Days from vendor release to 95% deployment for severity-critical patches  | Lagging |
| Phishing simulation click rate | Percent of simulated phishing emails clicked, by population               | Leading |
| MFA coverage                   | Percent of in-scope identities with phishing-resistant MFA                | Leading |
| Mean time to detect (MTTD)     | Median time from initial event to SOC alert, security incidents only      | Lagging |
| Third-party risk-review SLA    | Percent of in-scope vendors reviewed within their cadence                 | Leading |
| Crown-jewel control coverage   | Percent of named-asset controls passing most recent test                  | Leading |
| Aged security exceptions       | Count of exceptions past their second renewal, by approval tier           | Leading |
| Agentic AI shutdown readiness  | Percent of agentic AI systems with a shutdown drilled in the last quarter | Leading |
| Vendor tier reductions         | Count of vendors moved to a lower tier by narrowing their access          | Leading |

Indicators that look like security work but rarely belong on a board deck: number of vulnerabilities, number of blocked emails, number of EDR alerts. Volume is not signal.

The table above is an illustrative selection, not a menu. The fuller set, with each metric's gaming mode and its blind spot recorded next to its definition, is the [security metrics catalog](../metrics/security-metrics-catalog.md). The catalog is the source; this section is the selection problem, and the selection is the harder half. Never put the catalog itself in front of a board.

The last three rows are worth explaining, because they are unusual and each one reads correctly to a director without training.

Aged exceptions is a direct read on whether the program is closing its known gaps or accumulating them, and it is the number that moves when remediation goes unfunded. Shutdown readiness answers the question a board will eventually ask about AI, which is not how the models are governed on paper but how fast the company could stop one, and it is answerable only from drills that were actually run. Vendor tier reductions is the one that will look strange at first and is the most honest of the three: it counts vendors whose access was narrowed, which is exposure actually removed, as opposed to the review-SLA row above it, which counts work performed. Showing an activity measure next to an effect measure for the same program area is a quiet way to teach a board the difference, and the pair is more persuasive than either alone.

All three come from artifacts the program already maintains: the [exception register](../risk-management/security-exception-record.md), the [AI system register](../ai-governance/ai-system-register-pattern.md), and the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md).

## Section 3: Significant incidents and near misses

For each material incident or near miss in the quarter, one slide or one half-page. No client names, no individual names, no system identifiers that would compromise an open investigation.

- **What happened, in one paragraph.** Plain language.
- **Customer / regulatory / financial impact.** Quantified where possible.
- **Detection path.** How did we find it. This is often more diagnostic of program health than the incident itself.
- **Containment timeline.** Time to detect, time to contain, time to communicate.
- **Disclosure determination.** For a registrant, whether the incident entered the materiality process and what was determined, referencing the [determination record](../incident-response/materiality-determination-record.md) by ID. Report the count of incidents that entered the process alongside the count determined material; a quarter with disclosures and no visible process behind them is the thing an audit committee will ask about.
- **Action items and dates.** Tied to remediation backlog with owners.

If there are no material incidents, say so in one sentence and move on. Do not invent narrative.

## Section 4: Program updates

One slide per program area in scope, no more than 4 to 6 areas total. Suggested areas:

- Identity and access
- Vulnerability and patch management
- Third-party / vendor risk
- Detection and response
- Data protection
- AI governance, where the organization operates AI systems of consequence: inventory coverage from the [AI system register](../ai-governance/ai-system-register-pattern.md), how many agentic systems are live, and shutdown drill results
- Resilience (BCP / DR), including response readiness: which [tabletop scenario families](../incident-response/tabletop-exercise-pattern.md) have been exercised and who attended. Executive participation is the number worth showing a board, because unlike most security metrics it is one they can personally change

Each slide:

- Status against the published quarterly objectives (green / yellow / red, with reason).
- Notable shipped or delivered work this quarter.
- Notable in-flight work for next quarter.
- Open blockers, if any.

A board does not want a tour of every project. Show the shape, name the headwinds, move on.

## Section 5: Compliance and audit posture

A short status of regulatory and audit obligations:

- Open audit findings and aging.
- Compliance certifications status (in-scope frameworks, renewal dates).
- Regulatory engagements active this quarter.
- Material policy or framework changes since last meeting.
- Security exceptions at the executive approval tier, and the count past their second renewal. A board does not need the register; it needs the two or three waivers that were approved because remediation is unfunded, since those are decisions the board can actually change.

Tie back to the risk register where applicable; do not list the same item twice in different sections.

## Section 6: Strategic initiatives and roadmap

The longer-horizon view. One slide.

- Next 12 months on the security roadmap, framed by business outcome (not project name).
- Funding posture for the year against plan.
- Hiring posture against plan.

Strategic initiatives are the place to surface multi-quarter bets that need board awareness and continued support. Resist using this slide for a project list.

## Section 7: Forward look

The next 6 to 12 months. One slide.

- Emerging risks the program is watching, with rationale.
- External events on the horizon that may force program changes (regulation, M&A, major releases, geopolitical).
- Decisions or asks that will come to the board in the next two meetings, so the board can prepare.

## Section 8: Asks

Restate the asks from the executive summary, with the context needed to decide. Each ask:

- The decision needed, framed as a yes / no or A / B / C.
- The recommendation, with rationale.
- The cost of doing nothing.
- What changes if the decision waits a quarter.

If a board ends a meeting without any decisions being asked of them, they often feel they were given a status update rather than engaged in governance. Three good asks per quarter, with real options, is a healthy cadence.

## Backup material

Carry as appendices, not as part of the main flow:

- Detailed risk register extract (top 20 rows).
- Detailed incident chronology.
- Threat intelligence brief, sanitized.
- Glossary of terms used in the deck.

Reference the appendix when a director asks for detail. Do not lead with it.

## Where this connects

This deck should be a view over artifacts the program already maintains, not a document written from scratch each quarter. Where each section comes from:

| Section                                   | Source artifact                                                                                                                                                                               |
| ----------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Top three risks, and movement             | [Risk register](../risk-management/risk-register-pattern.md), sorted by residual score with the prior quarter's score alongside                                                               |
| Significant incidents                     | [Postmortems](../incident-response/blameless-postmortem-template.md) closed in the quarter, plus their [materiality determinations](../incident-response/materiality-determination-record.md) |
| AI governance                             | [AI system register](../ai-governance/ai-system-register-pattern.md): discovery coverage, agentic count, drill results                                                                        |
| Compliance and audit posture              | [Control mapping](../compliance/control-framework-mapping-pattern.md) and the audit finding backlog                                                                                           |
| Third-party posture                       | [Vendor tiering](../third-party-risk/vendor-risk-tiering-pattern.md): critical-tier count, review SLA, tier reductions achieved, and concentration across the critical tier                   |
| Response readiness                        | [Tabletop exercises](../incident-response/tabletop-exercise-pattern.md) run this year: scenario families covered, executive participation, and findings still open                            |
| The indicator set itself                  | [Security metrics catalog](../metrics/security-metrics-catalog.md), selecting six to ten and carrying each one's denominator                                                                  |
| Aged exceptions, and the asks behind them | [Exception register](../risk-management/security-exception-record.md), filtered to executive tier and to anything past a second renewal                                                       |

If assembling the deck requires original research rather than a query across those artifacts, the deck is not the problem. The underlying instruments are not being maintained at the cadence the board's questions assume, and the quarterly scramble is the symptom.

## Common failure modes

1. **Volume over signal.** Pages of metrics that no director knows how to interpret. Pick a small set and explain each one.
2. **No asks.** A polished status report with no decisions requested teaches the board that security is a reporting function rather than a governance function.
3. **Surprise incidents.** A material incident appears for the first time on the board deck. Brief committee chairs by phone the week the incident closes; the board meeting is not a discovery channel.
4. **Color-without-criteria.** A red indicator with no threshold definition is a feeling, not a signal. Tie every color to a published threshold.
5. **Reused decks.** The same slides quarter over quarter with minor updates train directors to skim. Refresh the structure annually, even if the underlying program is stable.

# Tabletop exercise design and after-action

A working pattern for designing a security tabletop exercise and recording what it found, opinionated about the thing an exercise should be testing and the reason most of them test something else.

An exercise is the only instrument a security program has for examining its response before an incident supplies the examination. That makes the design question, what exactly are we testing, the whole game. Run badly, a tabletop is two hours of people describing what they would do, followed by agreement that it went well. Run well, it produces a short list of specific things that do not work, each with a name and a date against it.

## Why this is a separate document from the postmortem

The two overlap enough that the question is worth answering directly.

A [postmortem](blameless-postmortem-template.md) is retrospective analysis of a real event, where the facts are discovered and the findings are about a system that failed. An exercise is a constructed event, where the facts were authored to provoke something, and the findings are about a response capability rather than about a system. They also differ in shape: half of this document is design work, choosing the decisions to test and writing the injects that force them, and the postmortem has no analogue for that because reality does the design.

What they share is the destination. Both produce action items that compete for the same engineering and program capacity, and both are worthless if those items do not ship. So the after-action section below deliberately reuses the postmortem's action-item table rather than inventing a second format. One format, two documents, one backlog. Where an organization keeps a single register of remediation work, exercise findings enter it on equal footing with incident findings, and it is worth being explicit that they do, because findings from an event that did not really happen are the first ones to get deprioritized.

## Design backward from the decisions

The most common design mistake is picking a scenario first. Someone proposes ransomware, the scenario gets written, the exercise runs, and afterward nobody can say what was learned because nothing in particular was being tested.

Invert it. Choose three or four decisions the organization has never actually had to make, then write the scenario that forces them. The scenario is a delivery mechanism, not the point.

Decisions worth testing, most of which no runbook answers:

- Who authorizes paying a ransom, and what is the approval path at 2am on a Sunday.
- Who decides to take a revenue-generating product offline, and against what threshold.
- Who declares an incident material, on what facts, and how fast can that group be assembled.
- Who talks to the regulator, and who has read the notification clock that applies.
- Who tells the affected customer, what do they say before the facts are confirmed, and who signs it off.
- At what point does the organization stop trusting its own identity provider.
- Who has authority to disconnect a third party that is actively integrated into production.

Notice how few of these belong to the security team. That is the finding most exercises are built to avoid.

## Injects target decisions, not technical steps

The technical response is usually the part an organization is best at, because it is the part that gets practiced incidentally every time something breaks. Writing injects about which log to check produces an exercise where the responders perform well and nothing is learned.

Write injects that force a decision under incomplete information, then take away the option that makes it easy.

> **Inject 2, T+40.** A journalist emails the general press inbox saying they have been shown a sample of customer records and are running the story in four hours. Legal counsel is on a flight and unreachable for three of them. What do you send, who approves it, and what do you say about scope you have not yet confirmed?

That inject tests a decision path, an approval authority, a communications capability, and the organization's tolerance for saying something imprecise under a deadline. An inject asking which SIEM query to run tests none of them.

## The scenario has to diverge from the playbook

Gaps in incident response appear where the scenario departs from what the plan anticipated. If the scenario is the one the runbook was written for, the exercise is testing reading comprehension.

Three cheap ways to force divergence, each of which reliably finds something:

- **Unexpected notification source.** The incident arrives from a customer, a journalist, a regulator, or an extortion email, rather than from the detection stack. Almost every plan is written assuming the SOC finds it first.
- **A missing dependency.** The person who owns the decision is unreachable, the ticketing system is part of the outage, the vendor's support line is a 24-hour queue, the backups are intact but the restore has never been timed.
- **Two things at once.** A routine second event, unrelated and low severity, competing for the same three people. This is how real weeks work and no plan accounts for it.

## Who is in the room

The single most consequential design decision, and the one usually made by whoever is free that afternoon.

An all-technical room tests half the response and the less fragile half. Every decision listed above is made by legal, communications, finance, or an executive, and an exercise without them produces confident answers about what those functions would probably do, which is precisely the assumption worth testing. Invite the people who own the decisions, and treat a decision-owner's absence as a finding in its own right rather than as a scheduling problem to work around.

Roles for the exercise itself:

- **Facilitator.** Runs the clock, delivers injects, and does not participate in solving. Ideally not the person who wrote the incident response plan, because that person cannot hear its gaps.
- **Scribe.** Captures decisions, the time each was reached, and what stalled. The scribe's notes are the after-action; nobody reconstructs this afterward.
- **Participants.** The decision owners, playing themselves.
- **Observers.** Silent. Useful for people who need the context but would otherwise dominate the room.

## Facilitation: the two rules that do the work

**No magic capabilities.** Participants may only use things that actually exist today. When someone says the team would restore from backup, the facilitator's job is to ask when a restore of that system was last performed end to end and how long it took. The answer is often the most valuable output of the exercise. This rule is what separates an exercise from a group description of best practices.

**Keep the clock hostile.** Real incidents apply time pressure and a relaxed room will design an ideal response nobody could execute. Deliver injects on schedule regardless of whether the previous discussion resolved, because unresolved discussion under a new inject is exactly the condition being tested.

A two-hour shape that works: 15 minutes of setup and ground rules, 90 minutes of exercise carrying three or four injects, 15 minutes of immediate reactions while people are still in the room, and a final block to write down the gaps before anyone leaves. The last block is not optional; a gap that is not written down in the room is not going to be written down later.

## Rotate the scenario family

Running ransomware every year trains one muscle and quietly reassures everyone. Rotate across families that test different decisions and different people.

| Family                             | The decision it exists to test                                                                         |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Ransomware with exfiltration       | Payment authority, and the separate decision about disclosure when data was taken as well as encrypted |
| Third-party compromise             | Acting when the affected environment is not yours and the vendor controls the information              |
| Insider, malicious or negligent    | Coordinating with HR and legal while preserving evidence and not tipping off                           |
| Disclosure-forcing incident        | Assembling the group that determines materiality, and doing it inside the clock                        |
| Agentic AI acting wrongly at scale | Who can stop it, how fast, and whether the actions it already took can be reversed                     |
| Extended platform outage           | Resilience and continuity, where the security team is not the lead and has to hand over                |

The AI and third-party families are the ones organizations most often skip and the ones where the response is least rehearsed.

## After-action record

```text
Exercise ID:        TTX-2026-02
Title:              <the decisions tested, not the scenario name>
Date:               2026-04-22
Origin:             Action item AI-03 from postmortem SEC-2026-Q1-014
Scenario family:    Third-party compromise
Facilitator:        <name>
Participants:       <names and functions>
Decision owners absent: <names and functions, with the decision each owns>
Scribe:             <name>
```

Naming the exercise for the decisions rather than the scenario is a small discipline with a real effect: it stops the annual exercise from being remembered as "the ransomware one" and makes it obvious when three consecutive exercises tested the same authority.

### Decision log

The section unique to an exercise, and the one that carries the value. For each decision the scenario forced:

| Decision                                | Reached at  | Stalled on                                                                     | What unblocked it                                 |
| --------------------------------------- | ----------- | ------------------------------------------------------------------------------ | ------------------------------------------------- |
| Take the customer portal offline        | T+35        | No published threshold; group debated revenue impact without a figure          | An executive in the room made the call personally |
| Notify the affected enterprise customer | Not reached | Nobody could say which contractual notification clock applied                  | Unresolved at exercise end                        |
| Disconnect the integrated vendor        | T+70        | No one present held the authority to revoke the vendor's production credential | Deferred to a name who was not in the room        |

"Not reached" is a legitimate and valuable entry. An exercise where every decision was reached comfortably was either too easy or too polite.

### What to measure

Time to first decision, not time to resolution. Resolution in a tabletop is fictional; the interval before a group commits to a course of action is not, and it is the number that predicts real performance.

Alongside it, count the times the group needed someone who was not in the room. That count is the most honest readiness measure an exercise produces, and it maps directly to what happens at 2am.

### Findings and action items

Use the [postmortem](blameless-postmortem-template.md) action-item table format exactly, with the same categories and the same requirement that every item carries an owner, a date, and a tracking ID in the real backlog. Add one column, whether the finding is a gap in capability, in authority, in documentation, or in awareness, because that split tells you over time whether the program keeps building tools for problems that are actually about who decides.

Cap the list. An exercise producing twenty findings has produced a list nobody will work; pick the ones that change the outcome and put the rest on a hardening backlog with a date.

## Where this connects

- Findings enter the same remediation backlog as [postmortem](blameless-postmortem-template.md) action items and use the same format. Exercises are frequently created by a postmortem action item, as in the worked example above.
- A finding that will not be fixed is a risk on the [risk register](../risk-management/risk-register-pattern.md), or an [exception](../risk-management/security-exception-record.md) if it means a stated requirement is knowingly unmet. It is not a note in an exercise report that gets filed.
- The materiality-decision family exercises the group and the clock in the [materiality determination record](materiality-determination-record.md). A disclosure committee that has never assembled under time pressure is the gap that document assumes away.
- The agentic AI family is the [AI system register](../ai-governance/ai-system-register-pattern.md) shutdown drill wrapped in a scenario, with the difference that the drill measures a procedure and the exercise tests whether anyone decides to invoke it.
- Third-party scenarios routinely find stale vendor contacts and an unclear revocation authority, which are properties of the relationship recorded in the [vendor tiering pattern](../third-party-risk/vendor-risk-tiering-pattern.md).
- Exercise cadence, scenario families covered, and executive participation belong in the program updates section of the [quarterly board update](../board-reporting/quarterly-security-update-template.md). Participation is the metric worth reporting, because it is the one the board can change.

## Common failure modes

1. **Scenario chosen first.** The exercise runs, everyone enjoys it, and no one can say what was tested. Choose the decisions, then build the scenario that forces them.
2. **Injects about technical steps.** The team performs well at the thing it is already good at. Target decisions made under incomplete information.
3. **An all-technical room.** Legal, communications, finance, and the executives who own the decisions are absent, so their behavior is assumed rather than observed.
4. **The playbook scenario.** The exercise follows the case the plan was written for and finds nothing. Force divergence with an unexpected notification source, a missing dependency, or a competing second event.
5. **Magic capabilities.** "We would restore from backup." Ask when that was last done end to end and how long it took.
6. **No gaps written in the room.** The hot wash produces good discussion and no document. Reserve the last block for writing, before anyone leaves.
7. **Findings that go nowhere.** Items without an owner, a date, and a backlog ID. Findings from a simulated event are the first to be deprioritized, so they need the same tracking discipline as incident findings, not less.
8. **The same exercise annually.** One scenario family, repeated, trains one muscle and produces steadily improving results that mean nothing. Rotate.

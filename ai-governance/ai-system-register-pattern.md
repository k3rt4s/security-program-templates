# AI system register pattern

A working pattern for the inventory of AI systems an organization actually runs, opinionated about the decisions that make the difference between a register that governs and a spreadsheet that ages.

Most AI governance effort in the last two years went into policy documents. The binding constraint is not policy, it is inventory: a policy that no one can apply to a system no one has enumerated does not govern anything. Regulators, auditors, and incident responders all start from the same question, which is what AI is running here and who is accountable for it. This is the artifact that answers it.

## What an AI system register is, and what it is not

An AI system register is a curated inventory of the AI use cases the organization operates or consumes, each with an accountable human owner, a declared data-classification ceiling, a stated class of autonomy, and a tested procedure for turning it off.

It is not a model catalog, not a vendor list, not a list of AI risks, and not an approval workflow. A model catalog tracks artifacts and versions and belongs to the ML platform team. Risks about AI systems belong on the [risk register](../risk-management/risk-register-pattern.md), scored the same way every other risk is scored. Approvals belong to the intake gate below, and the register is what the gate writes into. Collapsing these into one document produces a spreadsheet that serves none of them.

## The unit of registration is the use case, not the model or the tool

This is the single decision that determines whether the register is useful. Most first attempts register the wrong object.

Registering the model is useless because one model serves many purposes with wildly different exposure. Knowing that a general-purpose commercial model is "in use" tells you nothing about blast radius. Registering the vendor is worse, because a single vendor relationship can cover a harmless drafting assistant and an agent with write access to production.

Register the use case: a specific purpose, over specific data, producing a specific effect, owned by a specific person.

> **AI-007** — Draft first-response replies to inbound customer support tickets, for review by a human agent before sending.
>
> **AI-012** — Resolve billing disputes under $500 end to end: read the ticket, query the billing API, apply or deny the credit, notify the customer.

Same vendor, same underlying model, same quarter. One is a drafting aid and one can move money. They are two rows and they are governed differently.

## Three classes, three depths of registration

Registering everything at maximum depth is how a register dies in its second quarter. Scale the depth to what the system can do without a human.

| Class     | Definition                                                                                                                            | Registration depth                                                                              |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- |
| Assistive | Produces output a human reads and acts on. The system cannot change any system of record.                                             | Light: owner, purpose, data ceiling, vendor, review date.                                       |
| Advisory  | Output feeds a decision or a workflow, with human review that is sampled rather than complete.                                        | Full: all columns below, plus the sampling rate and who reviews.                                |
| Agentic   | Takes actions through tools or APIs without per-action human approval. Holds credentials. May hold persistent memory across sessions. | Full, plus non-human identity, action reversibility, blast-radius limit, and a tested shutdown. |

The line that matters is not how sophisticated the system is, it is whether a wrong output becomes a wrong action without a human in between. A simple script with an API key and a scheduler is agentic. A frontier model in a chat window that a human reads is assistive.

Reclassification is an event, not a background change. When a team adds tool access to an assistive system, it becomes agentic and re-enters the intake gate. Say this explicitly in the policy or it will happen quietly, which is the most common way an ungoverned agent reaches production.

## Columns

| Column                         | Purpose                                                          | Notes                                                                                                                                |
| ------------------------------ | ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| System ID                      | Stable identifier                                                | Never reuse. `AI-007`, `AI-012`.                                                                                                     |
| Use case                       | One sentence: purpose, data, effect                              | The single most important column. Written as what it does, not what it is.                                                           |
| Class                          | Assistive / Advisory / Agentic                                   | Drives registration depth and review cadence.                                                                                        |
| Accountable owner              | One person, not a team                                           | Same convention as the risk register. The owner is accountable for the system's behavior, not for building it.                       |
| Business sponsor               | The person whose outcome it serves                               | Separate from the owner. When these are the same person, the review is not independent.                                              |
| Data classification ceiling    | Highest class the system may process                             | Enforced at the integration, not asserted in the row. See below.                                                                     |
| Provider and model             | Vendor, model family, hosting mode                               | Hosting mode matters more than model name: vendor API, private endpoint, or self-hosted.                                             |
| Training and retention posture | Whether inputs train the provider's models, and retention period | Pull from the contract, not from the marketing page. Re-check on renewal.                                                            |
| Human oversight                | How a wrong output is caught                                     | Complete review, sampled review at a stated rate, or none. "None" is a legitimate answer for low-stakes assistive systems; write it. |
| Action scope                   | What the system can change                                       | Agentic only. Name the systems and the limits, e.g. "issue account credits up to $500; cannot alter service state".                  |
| Reversibility                  | Can an action be undone, by whom, how fast                       | Agentic only. An irreversible action taken at machine speed is the defining AI operational risk.                                     |
| Non-human identity             | The credential the system authenticates with                     | Agentic only. See below.                                                                                                             |
| Time to shutdown (tested)      | Measured minutes from decision to stopped, with drill date       | Not an estimate. See below.                                                                                                          |
| Linked risk IDs                | Rows on the enterprise risk register                             | The register inventories; the risk register scores.                                                                                  |
| Status                         | Proposed / Approved / Live / Suspended / Retired                 | Retired rows stay in the register with a retirement date.                                                                            |
| Next review date               | Calendar date                                                    | Cadence by class, below.                                                                                                             |

## The four fields most registers omit

Everything above the following four is inventory hygiene. These four are where a register earns its cost.

**Data classification ceiling, enforced rather than asserted.** A row that says "may not process customer PII" is a statement of intent. The useful question is what stops it. Name the enforcement point: a redaction proxy, a scoped service account, a filtered retrieval index, a separate tenant. If nothing enforces the ceiling, the honest entry is the highest class the system can reach, not the highest class it is supposed to reach.

**One accountable human owner.** Teams do not own AI systems any more than they own risks. When the system produces a wrong outcome, someone has to answer for it, decide whether to keep it running, and hold the shutdown authority. Name that person. When they leave, the row is reassigned before their last day or the system is suspended.

**Tested time to shutdown.** A recurring finding across the industry is that a majority of organizations do not know how quickly they could stop an AI system after an incident. Every agentic row carries a measured number and the date it was measured. The drill is described below. An untested shutdown is not a control, and it should not be reported as one.

**Reversibility of the action.** For each action an agentic system can take, record whether it can be undone, by whom, and inside what window. This is the field that determines whether an incident is a cleanup or a disclosure. Systems whose actions are irreversible and whose blast radius is wide should carry a hard rate limit, and the rate limit belongs in the action-scope column where an auditor can see it.

## Discovery: you cannot register what you cannot see

A register built from a survey enumerates the compliant. Adoption of AI tools outside formal approval is now the normal case, not the exception, so the register has to start from discovery and use the survey only to fill gaps.

Four channels, run on a schedule, each finding things the others miss:

1. **Spend.** Expense reports, corporate cards, and SaaS management data. Individual and team-level subscriptions surface here first and nowhere else.
2. **Identity.** OAuth grants and third-party app consents against the corporate identity provider. This is the highest-yield channel for tools that read corporate data, because the grant is the access.
3. **Network.** Egress to known AI service endpoints, from managed endpoints and from server subnets. Server-subnet egress is the interesting half: it usually means an integration rather than a person.
4. **Code.** Provider SDK imports, API key patterns, and agent framework dependencies across the source repositories and CI configuration. This is where agentic systems appear before anyone calls them that.

Run all four before concluding the register is complete. Report discovery coverage, not just row count, in the [board update](../board-reporting/quarterly-security-update-template.md); a register of 40 rows built from a survey is weaker evidence than one of 12 built from four discovery channels.

## The intake gate: five questions

A long intake form guarantees shadow AI, because the cost of asking exceeds the cost of not asking. Keep the gate short enough that a team routes through it by default and reserve depth for the classes that warrant it.

1. What decision or action does this system produce, and what happens if it is wrong?
2. What data does it see, and what is the highest classification in that set?
3. Can it change anything without a human approving that specific change?
4. Who is the accountable owner?
5. How do we turn it off?

Questions 3 and 5 do the classification. If the answer to 3 is yes, it is agentic, and the full depth applies. If there is no clean answer to 5, it does not go live.

## The shutdown drill

Once per quarter for agentic systems, twice per year for advisory, on a schedule that the owner does not choose in advance.

Someone with shutdown authority who is not the system's builder executes the documented procedure in a real environment. Measure from decision to confirmed stopped, including the time to find the right person and the right runbook, because that is where the minutes actually go. Record the measured number and the date in the register.

Two failure conditions worth naming, because both are common and neither shows up in a paper review: the shutdown depends on a single person who was not reachable, and stopping the system leaves in-flight actions half-applied with no defined state. Both are findings, and both belong in the [postmortem](../incident-response/blameless-postmortem-template.md) format if a real incident exposes them first.

## Non-human identity for agentic systems

An agent that calls tools is an authenticated principal, and it is usually a badly governed one: a long-lived key, over-scoped, issued to a shared service account, rotated never. Register the identity alongside the system.

- The credential type and where it is stored.
- The scope, expressed as the permissions it actually holds rather than the permissions it was intended to hold. Read them back from the target system.
- The rotation interval and the last rotation date.
- Whether the identity is unique to this system. Shared identities across two agentic systems make attribution impossible during an incident and should be treated as a finding.

## Relationship to the risk register

The two artifacts answer different questions and the split is worth defending. The AI register answers what is running and who owns it. The [risk register](../risk-management/risk-register-pattern.md) answers what could go wrong and what we are doing about it.

A row in the AI register is not a risk. The risk is written in the standard form and lives on the risk register with a link back:

> An external attacker may exploit prompt injection in the billing-dispute agent (AI-012) to issue unauthorized account credits, causing direct financial loss and a control-failure finding in the next SOC 2 examination.

Where an AI system is operating outside policy on purpose, with a deadline and a compensating control, that is an [exception](../risk-management/security-exception-record.md), not a register annotation.

## Review cadence and minimum viable register

Agentic systems review quarterly, advisory semi-annually, assistive annually or on material change. Any reclassification, any change to action scope, and any provider change to training or retention terms triggers an immediate review regardless of the calendar.

Starting from nothing, run the four discovery channels, then register every agentic system without exception, every advisory system touching regulated or customer data, and the assistive systems as a summary count by tool until they can be enumerated properly. A complete agentic inventory with a partial assistive one is a working register. The reverse, which is what most survey-built registers produce, is not.

## Where this sits against the external frameworks

The register is the precondition for all of them rather than a deliverable under any one of them. Inventory is the first practical step of the NIST AI Risk Management Framework's Map function, the operational core of the ISO/IEC 42001 requirement to identify AI systems and assign responsibility, and the prerequisite for EU AI Act classification work, since an organization cannot determine whether it operates a high-risk system without knowing what it operates.

Build the register first and map afterward. Building the register to satisfy a specific framework produces a register shaped like that framework's evidence request, which is the pattern described in the [control mapping](../compliance/control-framework-mapping-pattern.md) document and it fails the same way here.

## Common failure modes

1. **Registering models instead of use cases.** The row says "commercial LLM, approved". It governs nothing, because the exposure lives in what the system is pointed at and what it can change.
2. **Survey-built registers.** The rows all come from teams that volunteered. Everything found through the four discovery channels is missing, which is most of it.
3. **Untested shutdown.** A procedure exists in a document and has never been executed. The number in the register is an estimate someone wrote down, and it is optimistic.
4. **Silent reclassification.** An assistive system gets tool access in a sprint and becomes agentic without re-entering the gate. Make reclassification an explicit event with a named trigger.
5. **Asserted data ceilings.** The row claims a limit that nothing enforces. Record what the system can reach, not what the policy says it should.
6. **Owner equals builder.** The person who built the system also decides whether it keeps running. Separate accountable owner from business sponsor and from builder.
7. **The register as the whole program.** An inventory is not governance. It is the artifact that makes governance possible, and it is worth exactly as much as the review cadence and the gate behind it.

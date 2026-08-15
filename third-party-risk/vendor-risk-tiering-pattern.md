# Vendor risk tiering and assessment depth

A working pattern for deciding how hard to look at each third party, opinionated about the tiering dimension most programs leave out and the assessment work tiering should make proportionate.

Third-party exposure is consistently named as the hardest part of becoming resilient, and the standard response is a questionnaire. The questionnaire is not the problem. Sending the same questionnaire to every vendor is the problem, because it makes the assessment cost constant while the exposure varies by three orders of magnitude, and a program that costs the same for a facilities contract as for a vendor with write access to production will run out of capacity before it runs out of vendors.

## What this pattern covers

How to tier a third party by what it can do to you, how deep to assess at each tier, what a questionnaire can and cannot establish, and what to do at the end of the relationship.

It is not a questionnaire, and deliberately so. Good questionnaire content is abundant and freely available, from the standardized industry sets to the ones bundled with every TPRM platform. What is scarce is the judgment about which vendors deserve one, what to read in the answers, and what evidence to require instead when the answers are not enough.

## Tier by what the vendor can do to you

Most tiering models are driven by contract value, sometimes with a criticality label bolted on. Spend is a proxy for how much the business cares, not for how much damage the vendor can do, and the two come apart constantly. A facilities management contract at seven figures cannot reach a customer record. An observability vendor at four figures a month holds a write-scoped token into production and receives a copy of every application log. Tiering on spend puts the assessment effort on the wrong one.

Score three dimensions independently. Each is a property of the relationship, not an opinion about the vendor.

**Data reach.** The highest classification of data the vendor can access, and roughly how much of it. Access includes what the vendor can pull, not only what is deliberately sent: a support tool with an impersonation feature reaches everything the impersonated user reaches, and log streams routinely carry more than the data map says they do.

**Access mode.** What the vendor can do inside your environment. This is the dimension most models omit entirely and the one that dominates real incidents.

| Level | Access mode                                                                                            |
| ----- | ------------------------------------------------------------------------------------------------------ |
| 0     | No access. Data is exchanged in a defined artifact, or not at all.                                     |
| 1     | Read via scheduled export or file transfer. Latency between your systems and theirs.                   |
| 2     | Read via live integration. A credential exists that reads on demand.                                   |
| 3     | Write into a system of record. The vendor can change your data.                                        |
| 4     | Privileged or administrative access into your environment, including the ability to grant itself more. |

**Operational dependency.** What breaks if the vendor is unavailable, and how long you can run without them. Answer it as a duration, not as a label. A vendor whose absence is invisible for a month is different from one whose absence stops invoicing tomorrow, and the difference is a number the business owner can give you in one conversation.

## Composition: take the highest, never the average

The tier is the highest of the three dimensions, not a blended score.

Averaging is how a vendor with level-4 access, no data, and no operational dependency lands in the middle of the range and gets a light-touch review. That vendor is the one that shows up in breach reports. Any scoring model that lets a maximum be diluted by two minimums is producing a comfortable number rather than a useful one. If a weighted score is required for a governance committee that expects one, compute it, show it alongside the tier, and let the tier drive the work.

| Tier           | Trigger                                                                                         | Typical shape                                                                   |
| -------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| 4, Critical    | Access mode 3 or 4, or regulated data at volume, or an operational dependency measured in hours | Infrastructure, payment processing, identity, anything with a token that writes |
| 3, Significant | Access mode 2, or regulated data at low volume, or a dependency measured in days                | Live-integrated SaaS, subprocessors handling customer data                      |
| 2, Standard    | Access mode 1, or internal-only data, or a dependency measured in weeks                         | Exported reporting feeds, most business tooling                                 |
| 1, Minimal     | Access mode 0 and no sensitive data and no near-term dependency                                 | Facilities, professional services with no system access, commodity supply       |

## Assessment depth per tier

The point of tiering is that the work is proportionate. Publish this table, because the argument you will have is with an internal team who wants a Tier 4 vendor onboarded on Tier 2 evidence, and the argument goes better against a published standard than against a judgment call.

| Tier | Assessment                                                                                                                                                                                                                                                         | Cadence                          | Reassessment triggers                                         |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------- | ------------------------------------------------------------- |
| 4    | Independent evidence reviewed in full: audit report with scope and exceptions read, penetration test summary, architecture and data-flow review, named-control verification for the controls you actually depend on, contract security terms, documented exit plan | Annual, plus on trigger          | All of the triggers below                                     |
| 3    | Standardized questionnaire, plus the vendor's current audit report or certification with the scope section read                                                                                                                                                    | Every two years, plus on trigger | Breach, acquisition, subprocessor change, certification lapse |
| 2    | Attestation and a certificate check. No custom questionnaire                                                                                                                                                                                                       | On renewal                       | Breach, acquisition                                           |
| 1    | Registration only: owner, purpose, and what data it does not touch                                                                                                                                                                                                 | None                             | Change in access mode                                         |

The last column matters more than the cadence column. A calendar review is a snapshot of a vendor that keeps changing between snapshots, and the changes that matter arrive on their own schedule: the vendor is breached, is acquired, changes its subprocessors, loses a certification, or quietly adds an AI subprocessor in a data-processing amendment that arrives as an email nobody reads. Trigger-based reassessment catches those; annual review catches them up to twelve months late. Build the trigger list first and treat the cadence as the backstop.

## What a questionnaire establishes, and what it does not

Reliance on questionnaires is near-universal and confidence that they reflect reality is not. Both facts are true at once and neither means the questionnaire is worthless. It means it is being asked to do a job it cannot do.

A questionnaire reliably establishes three things. It creates a dated record of what the vendor asserted, which matters contractually and matters more after an incident. It reveals the vendor's own maturity through how they answer, since a security team that responds precisely, flags the questions that do not apply, and volunteers a scope boundary is a different organization from one that returns a hundred green checkboxes in a day. And it surfaces the specific gaps worth asking a follow-up question about.

It cannot tell you what is actually running. For that, at Tier 4, require evidence and read it properly:

- **The audit report's scope, not its cover page.** Which systems and which trust criteria are in scope, what period is covered, and which subservice organizations are carved out. A report whose scope excludes the product you are buying is a common and easily missed outcome.
- **The exceptions section.** This is where the signal is. A Type II report with a clean opinion and three testing exceptions describes a different vendor from one with none, and most reviews never reach that section.
- **The gap between report period and today.** A report covering a period that ended ten months ago says little about the vendor you are onboarding now, especially through an acquisition.
- **A bridge letter** where the gap is long, and note that a bridge letter is an assertion by management rather than tested evidence.

## Access is the control, not the questionnaire

The strongest thing a third-party program does is not assessment, it is constraining the access mode, and that reframing is worth making explicit because it changes who does the work.

Moving a vendor from write access to read, or from a live integration to a scheduled export, or from a shared administrative account to a scoped service identity, lowers the tier permanently. It reduces the exposure and it reduces the assessment cost every year thereafter. Assessment, by contrast, buys information about exposure without reducing it.

The practical consequence is that the highest-leverage third-party work is architectural and belongs in design review, not in the vendor questionnaire queue. A program that reports how many assessments it completed and not how many vendors it moved down a tier is measuring its own throughput rather than its effect.

## Fourth parties and concentration

Enumerating your vendors' vendors in full is not achievable and chasing it consumes the capacity that should go into the top tier. Two bounded questions get most of the value.

For each Tier 4 vendor, which of their dependencies underpins the specific thing you depend on them for. Not their whole supplier list, the one or two that carry your critical path. This is a question a Tier 4 vendor can answer, and their willingness to answer it is itself informative.

Then look across the answers for concentration. How many of your Tier 4 vendors run in the same cloud region, sit behind the same identity provider, or depend on the same upstream service. Concentration is invisible in a per-vendor assessment because every individual row looks fine; it only appears when the rows are read together, and it is the mechanism by which several independent vendors fail at the same time. Record it as a risk on the [risk register](../risk-management/risk-register-pattern.md), because it is not attributable to any single vendor and will otherwise belong to nobody.

## Offboarding

The least performed step in third-party risk management, and the one that leaves standing exposure behind.

Commercial offboarding and technical offboarding are different events, and the first happens reliably because someone stops paying an invoice. The second requires someone to revoke the credential, remove the integration, disable the federation, close the network path, and obtain the data deletion attestation the contract entitles you to. A vendor relationship that ended in procurement and not in the identity provider is an active credential belonging to an organization that no longer has any reason to protect it.

Make the offboarding checklist a required artifact for Tier 3 and above, owned by the same person who owned the relationship, and audit it by sampling terminated vendors against the identity provider once a year. That sample is one of the cheaper and more reliably alarming exercises a program can run.

Third-party accounts also fall inside the scope of the recurring campaign in the [access review pattern](../identity/access-review-pattern.md), which is where a live credential belonging to a terminated vendor usually surfaces. When it does, the finding is an offboarding failure rather than a review finding, and it should be fixed here rather than closed there.

## Where this connects

- Tier and access mode are properties of the vendor relationship; the risk of a specific vendor failure is written in the standard form on the [risk register](../risk-management/risk-register-pattern.md). Concentration across Tier 4 vendors is its own row and belongs to the program rather than to a vendor owner.
- A vendor onboarded despite failing its tier's assessment is an [exception](../risk-management/security-exception-record.md), with an expiry and a compensating control, not a note in the vendor file. This is one of the most common places an undocumented gap enters a program.
- A vendor that processes your data through an AI subprocessor introduces a row on the [AI system register](../ai-governance/ai-system-register-pattern.md), and a change in that subprocessor is a reassessment trigger here. Data-processing amendments are the channel this arrives through.
- Third-party review SLA and the count of Tier 4 vendors are already indicators in the [quarterly board update](../board-reporting/quarterly-security-update-template.md). The more useful number to add is how many vendors moved down a tier through an access change.
- Where a vendor's certification is the evidence behind a control you claim, the [control mapping](../compliance/control-framework-mapping-pattern.md) should say so, so that a lapsed vendor certification surfaces as a mapping defect rather than as a surprise during an audit.
- Run the third-party scenario family in the [tabletop exercise pattern](../incident-response/tabletop-exercise-pattern.md) against a Tier 4 vendor. It finds two things reliably and cheaply: the vendor contact list is stale, and nobody in the room holds the authority to revoke a live production credential belonging to a vendor. Both are properties of the relationship rather than of the vendor, which means both are yours to fix. That second one is a missing row in the [decision rights register](../governance/decision-rights-register.md), and it can be found there by inspection rather than waiting for an exercise to surface it.

## Common failure modes

1. **Tiering on spend.** The most expensive vendor gets the deepest review and the cheap one with an admin token gets a checkbox. Tier on data reach, access mode, and dependency.
2. **Averaging the dimensions.** A blended score lets level-4 access be diluted by two low scores. Take the highest.
3. **One questionnaire for everyone.** Assessment cost stays constant while exposure varies enormously, and the program runs out of capacity in the tier that matters.
4. **Reading the cover page.** The audit report is accepted on its opinion, and the scope, the carve-outs, the period, and the exceptions section go unread. The exceptions section is the reason the report exists.
5. **Calendar-only reassessment.** The review cycle is annual and the vendor was acquired in March. Build the trigger list and treat the cadence as a backstop.
6. **Assessment mistaken for reduction.** The program reports assessments completed. Nothing about the exposure changed. Count tier reductions achieved through access changes.
7. **No technical offboarding.** The contract ended, the credential did not. Sample terminated vendors against the identity provider annually.
8. **Concentration invisible.** Every vendor row is individually acceptable and six of them share one region. Read the rows together and put the result on the risk register.

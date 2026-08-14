# Control framework mapping pattern

A working pattern for mapping security controls across multiple frameworks (NIST SP 800-53, ISO/IEC 27001 Annex A, SOC 2 Trust Services Criteria, CIS Critical Security Controls, and house standards) without losing the meaning of any of them. Used for compliance scope analysis, certification preparation, and answering "are we already covered" questions during contract or audit response.

## Why mapping is harder than it looks

Mappings between security control frameworks look like a translation problem and are actually a semantic alignment problem. A NIST 800-53 control like `AC-2 Account Management` overlaps with ISO 27001:2022 `A.5.16 Identity management` and SOC 2 `CC6.1` but none of those three say exactly the same thing, require exactly the same evidence, or cover exactly the same scope. Treating the mapping as a 1:1 table will produce a document that audit teams quietly distrust.

Two principles keep mappings honest:

1. **Direction matters.** Mapping "framework A to framework B" implies a question: are we using A to satisfy B, or vice versa. The mapping looks different depending on which framework is the source of truth for evidence and which is the target for certification.
2. **Coverage is partial by default.** A mapped pair almost always has overlap, residual, and remainder. Document each of the three; do not collapse them into "yes / no covered".

## The unit of mapping is the control, not the framework

Build the mapping at the level of individual controls, not whole frameworks. The artifact is a table where each row is one source control and each row records the targets it touches, with a coverage qualifier.

| Source control | Source framework  | Target control | Target framework   | Coverage | Notes                                                  |
| -------------- | ----------------- | -------------- | ------------------ | -------- | ------------------------------------------------------ |
| AC-2           | NIST 800-53 rev 5 | A.5.16         | ISO/IEC 27001:2022 | Full     | Both require provisioning, review, and deprovisioning. |
| AC-2           | NIST 800-53 rev 5 | CC6.1          | SOC 2 TSC 2017     | Partial  | SOC 2 narrower; covers logical access only.            |
| AC-2           | NIST 800-53 rev 5 | CC6.2          | SOC 2 TSC 2017     | Partial  | Together with CC6.1 covers most of AC-2.               |
| AC-2           | NIST 800-53 rev 5 | Control 5      | CIS Controls v8    | Partial  | CIS focuses on account inventory, less on lifecycle.   |

## Coverage qualifiers

Use a short, fixed set of qualifiers. Free-text descriptions produce inconsistent mappings across teams.

- **Full:** the source control fully satisfies the target control. Evidence used to demonstrate the source can be reused for the target without modification.
- **Partial:** the source control satisfies some elements of the target control. Either the scope is narrower, the depth is lighter, or specific sub-requirements are not covered. Notes explain which.
- **Compensating:** the source control does not directly satisfy the target, but the combined effect of the source and one or more named other controls does. List the other controls.
- **Reference only:** the source and target are topically related but the source does not contribute to satisfying the target. Useful for documentation but not for evidence reuse.
- **None:** no meaningful relationship.

If a row reads "Full" and there is more than one sentence in Notes, the row is probably "Partial" and the author is being generous.

## The 80/20 mapping for a multi-framework program

For an organization that holds (or is pursuing) certifications across SOC 2 plus ISO 27001 plus a sector framework, build the mapping in this order:

1. **Pick the deepest framework as the canonical source.** Usually NIST 800-53 (rev 5) for organizations doing federal work, or ISO/IEC 27001/27002 for organizations centered on international certification. The canonical source is where evidence lives.
2. **Map each canonical control outward to the certification targets.** This is the table above.
3. **Identify gaps in the canonical source against any target.** Controls in the target with no full or partial coverage from the canonical source are real work, not paperwork.
4. **Adopt a small house-standard layer for things the canonical framework underspecifies.** Examples: AI and model governance, privacy-by-design requirements, customer-data-handling specifics. The house-standard layer maps both ways.

AI governance is currently the clearest case for that fourth layer. The AI management-system standard (ISO/IEC 42001) and the NIST AI Risk Management Framework are both real targets, but the crosswalks between them and the established security frameworks are considerably less mature than the 800-53 to 27001 lineage, and the control language does not decompose cleanly onto existing security controls. The honest posture is to carry AI controls as a house-standard layer with explicit partial mappings outward, rather than to claim coverage from security controls that were not written for the problem. What makes any of it tractable is the inventory: see the [AI system register](../ai-governance/ai-system-register-pattern.md), which is the precondition for all three targets rather than a deliverable under any one of them.

This stops the common antipattern of running parallel control programs per certification, which doubles evidence work and produces contradictions.

## Authoritative crosswalks to use as a starting point

Do not build a mapping from scratch when an authoritative crosswalk exists.

- NIST SP 800-53 rev 5 publishes machine-readable mappings to ISO/IEC 27001:2013 (and is being updated for 27001:2022). Start there.
- AICPA publishes a SOC 2 TSC mapping to COSO and to other frameworks.
- CIS Controls v8 publishes mappings to NIST CSF, NIST 800-53, ISO 27001, and PCI DSS.
- The Secure Controls Framework (SCF) maintains a community crosswalk across dozens of frameworks and is useful as a sanity check.

Treat each crosswalk as a starting point, not as an output. The official crosswalk does not know your scope, your evidence, or your architecture; the mapping that ships to an auditor reflects all three.

## Evidence reuse, the actual point

The reason a mapping exists is to let one piece of evidence satisfy multiple frameworks. The mapping is valuable only to the extent that auditors accept the reused evidence. Two practices that hold up under audit:

- **Tag the evidence to the source control, not to each target.** When the canonical source changes (rev 5 to rev 6, ISO 2022 to a future revision), only the source-to-target table changes; the evidence index does not.
- **Maintain a per-target evidence index that walks through the mapping.** This index is what gets handed to an audit team. It says: target control X is satisfied by source controls A, B, C, which produce evidence items 1, 2, 3.

## Maintenance cadence

A control mapping is not a one-time artifact. The cadence:

- **On framework revision** (rev 4 -> rev 5, ISO 2013 -> 2022): full mapping refresh, treated as a discrete project with a named owner.
- **On scope change** (new business unit, new product line, new regulated data type): refresh the affected slice; do not let scope drift silently.
- **Annually:** spot-audit a sample of mappings against current evidence. Mappings rot when controls evolve and the table does not.

## Where this connects

- A control that a mapping claims as covered, but which a live [security exception](../risk-management/security-exception-record.md) waives for some part of the scope, is a mapping defect and an audit exposure. Reconcile the exception register against the mapping at the annual spot-audit, and treat any control named in an audit assertion or a customer commitment as ineligible for exception in the first place.
- Gaps found in step 3 above are remediation work, not risks. They belong on a remediation backlog. The risk is the consequence of the gap, written in the standard form on the [risk register](../risk-management/risk-register-pattern.md). Keeping these separate is the same discipline that keeps a register from filling with control gaps.
- Open findings, certification status, and framework changes are section 5 of the [quarterly board update](../board-reporting/quarterly-security-update-template.md).

## Common failure modes

1. **Spreadsheet sprawl.** Every team has its own mapping spreadsheet, slightly different from every other team's. Establish one canonical source and prohibit shadow copies.
2. **Mapping as wish.** Rows marked "Full" because the team wishes the source covered the target. Coverage qualifiers must be honest; auditors will read the notes.
3. **No remainder.** A row that does not document what is *not* covered hides risk. Even fully-mapped controls have edge cases worth noting.
4. **Evidence orphaned from the mapping.** The mapping exists, the evidence exists, but no one can show which evidence backs which target control. Build the per-target evidence index from day one.
5. **One-shot project.** Mapping treated as a project that delivers and ends. Without an owner and a cadence, the mapping is a snapshot of a single moment, increasingly wrong over time.

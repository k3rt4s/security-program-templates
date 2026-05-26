# security-program-templates

Working templates and patterns for the documents a security program needs to actually run: a risk register that says something useful, a board update that earns time on the agenda, an incident postmortem that produces shipped changes, and a control mapping that auditors trust.

Markdown only. No code. Each document is opinionated about the things that most organizations get wrong the first time they build one, and each ends with a short list of failure modes worth watching for.

## Contents

| File                                                                                                             | What it is                                                                                                                                                                                                       |
| ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`risk-management/risk-register-pattern.md`](risk-management/risk-register-pattern.md)                           | Risk register column set, 5-point likelihood and impact rubrics anchored to time horizon and money, treatment options, and the bar for an "Accept" decision.                                                     |
| [`board-reporting/quarterly-security-update-template.md`](board-reporting/quarterly-security-update-template.md) | Eight-section structure for the quarterly board or board-committee security update, including a one-page executive summary, an indicators dashboard with criteria for color, and a discipline around board asks. |
| [`incident-response/blameless-postmortem-template.md`](incident-response/blameless-postmortem-template.md)       | Header through action items for a blameless security-incident postmortem, with explicit notes on when blameless is and is not the right frame, and how to separate causes from contributing factors.             |
| [`compliance/control-framework-mapping-pattern.md`](compliance/control-framework-mapping-pattern.md)             | Pattern for mapping controls across NIST 800-53, ISO/IEC 27001, SOC 2 TSC, CIS Controls, and house standards, with honest coverage qualifiers and an evidence-reuse model that survives audit.                   |

## How to use these

Copy a template into your wiki, document store, or repo and adapt it. The text is intentionally written so the structure stays useful after you cut the parts that do not fit your organization. The footers on each document call out the things that most often break a first implementation, so they are worth keeping even after the rest is rewritten.

## What this is, and what it is not

These are templates and patterns. They are not policies, not standards, not control libraries, and not a security program in a box. They are the shape of the artifacts a security program produces, distilled from running them in real organizations and watching what works and what does not. The judgment lives in how the templates get filled in for a specific context.

## License

CC BY 4.0. Use, adapt, redistribute. Attribution required. See `LICENSE`.

# security-program-templates

Working templates and patterns for the documents a security program needs to actually run: a risk register that says something useful, a board update that earns time on the agenda, an incident postmortem that produces shipped changes, and a control mapping that auditors trust.

Markdown only. No code. Each document is opinionated about the things that most organizations get wrong the first time they build one, and each ends with a short list of failure modes worth watching for.

## Contents

<!-- BEGIN CONTENTS (auto-generated, do not edit by hand) -->

- [board-reporting/](board-reporting/README.md): Templates for the quarterly security update delivered to a board or board committee, covering structure, indicators, and the discipline around what to ask from the board.
- [compliance/](compliance/README.md): Patterns for mapping security controls across multiple frameworks (NIST 800-53, ISO/IEC 27001, SOC 2, CIS Controls) with honest coverage qualifiers that survive audit scrutiny.
- [incident-response/](incident-response/README.md): Templates for blameless security incident postmortems, structured to separate root-cause analysis from accountability and to feed action items into the regular engineering backlog.
- [risk-management/](risk-management/README.md): Patterns for an enterprise risk register, covering risk statement format, a 5-point likelihood and impact rubric anchored to time and money, treatment options, and the bar for an Accept decision.

<!-- END CONTENTS -->

## How to use these

Copy a template into your wiki, document store, or repo and adapt it. The text is intentionally written so the structure stays useful after you cut the parts that do not fit your organization. The footers on each document call out the things that most often break a first implementation, so they are worth keeping even after the rest is rewritten.

## What this is, and what it is not

These are templates and patterns. They are not policies, not standards, not control libraries, and not a security program in a box. They are the shape of the artifacts a security program produces, distilled from running them in real organizations and watching what works and what does not. The judgment lives in how the templates get filled in for a specific context.

## License

CC BY 4.0. Use, adapt, redistribute. Attribution required. See `LICENSE`.

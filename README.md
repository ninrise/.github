# Ninrise • Organization Health Repository

Welcome to the `.github` organization health repository for [Ninrise](https://github.com/ninrise).

This special repository houses default community health files, issue and pull request templates, and profile configurations that automatically apply across all repositories within the `ninrise` organization unless explicitly overridden at the repository level.

---

## Repository Structure

```text
.github/
├── profile/
│   └── README.md                # Public organization profile for github.com/ninrise
├── ISSUE_TEMPLATE/
│   ├── config.yml               # Issue template configuration & security redirect
│   ├── bug_report.md            # Standard template for bug and defect reports
│   └── feature_request.md       # Standard template for feature proposals
├── PULL_REQUEST_TEMPLATE.md     # Default PR template across Ninrise repositories
├── CONTRIBUTING.md              # Organization-wide contribution guidelines
├── CODING_CONVENTIONS.md        # Technical standards (C++, Rust, Python, TypeScript)
├── CODE_OF_CONDUCT.md           # Community standards and conduct expectations
├── SECURITY.md                  # Coordinated vulnerability disclosure policy
└── README.md                    # Overview of this health repository
```

---

## File Overview

- **[`profile/README.md`](profile/README.md)**: Displayed on the front page of [github.com/ninrise](https://github.com/ninrise), showcasing Ninrise’s brand identity, mission, core values, domains, and divisions.
- **[`ISSUE_TEMPLATE/`](ISSUE_TEMPLATE/)**: Defines issue templates with structured prompts for reproducible bug reports and well-reasoned feature requests, routing security concerns away from public trackers.
- **[`PULL_REQUEST_TEMPLATE.md`](PULL_REQUEST_TEMPLATE.md)**: Guides contributors through describing changes, categorizing scope, and completing a quality checklist before code review.
- **[`CONTRIBUTING.md`](CONTRIBUTING.md)**: Outlines collaborative expectations, fork-and-pull-request workflows, commit conventions, and best practices aligned with our values.
- **[`CODING_CONVENTIONS.md`](CODING_CONVENTIONS.md)**: Establishes rigorous engineering and language-specific conventions for **C++** (Games), **Rust** (Backend), **Python** (Data/AI), and **TypeScript** (Frontend).
- **[`CODE_OF_CONDUCT.md`](CODE_OF_CONDUCT.md)**: Sets our standards for an inclusive, respectful, and safe community grounded in *Integrity* and *Humanity*.
- **[`SECURITY.md`](SECURITY.md)**: Details our vulnerability reporting procedures, safe harbor provisions, and maintainer response commitments.

---

## Maintenance

Updates to files in this repository instantly propagate default behavior to all public and private repositories in the `ninrise` organization that do not define their own local `.github/` counterparts.

*Resolve right. Build with heart.*

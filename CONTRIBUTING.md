# Contributing to Ninrise

Thank you for your interest in contributing to **Ninrise** (`N∞`)! We warmly welcome contributions from the community. Whether you are reporting a bug, improving documentation, designing a feature, or writing code, your involvement helps us build software that lasts.

Our motto is simple: **Resolve right. Build with heart.**

---

## Core Principles

When contributing to any Ninrise project, keep our core values in mind:

1. **Integrity:** Write honest code. Take responsibility for your changes, be candid in discussions, and ensure transparent, verifiable implementation.
2. **Humanity:** Treat every collaborator with empathy and respect. We welcome questions, constructive feedback, and diverse technical perspectives.
3. **Innovation:** Think critically and creatively. If you see a cleaner, more resilient way to solve a problem, bring it forward.
4. **Dedication:** Sweat the details. Write clean tests, follow project formatting, and ensure your work is complete and ready for production.

---

## Technical Standards & Conventions

To maintain high architectural consistency across all repositories in the `ninrise` organization, please review our detailed **[Coding Conventions Guide](CODING_CONVENTIONS.md)** and adhere to the following standards:

### 1. Technology Stacks
Ninrise projects are built across four primary languages:
- **C++ (C++20+):** Game development, engine core, and performance-critical systems (**Ninrise Studio**).
- **Rust:** High-performance, memory-safe backend microservices, APIs, and foundational infrastructure.
- **Python (3.10+):** Data pipelines, telemetry processing, machine learning models, and analytics.
- **TypeScript:** Frontend user interfaces, design systems, and web applications.

### 2. Standard Repository Naming
All repositories under `github.com/ninrise` strictly follow `kebab-case` with standardized functional prefixes:

```text
[prefix-]<domain/service-name>[-type]
```

| Prefix Group | Description & Target Purpose | Examples |
| :--- | :--- | :--- |
| *(None / Root)* | Flagship products, core documentation, or organization repos | `ninrise-web`, `.github` |
| `service-` / `api-` | Backend services, microservices, and API gateways | `service-auth`, `api-billing` |
| `app-` / `web-` | User-facing web dashboards, client apps, and mobile frontends | `web-dashboard`, `app-mobile-ios` |
| `infra-` | Cloud infrastructure, Terraform modules, Docker, and K8s configs | `infra-k8s-cluster`, `infra-terraform-aws` |
| `lib-` / `sdk-` | Shared libraries, utility packages, and client SDKs | `sdk-python`, `lib-common-logger` |
| `tool-` | Internal developer utilities, CLI tools, and automation scripts | `tool-db-migrator`, `tool-agent-orchestrator` |
| `template-` | Project starter templates and boilerplate repositories | `template-rust-service`, `template-nextjs-app` |

*For games developed under **Ninrise Studio**, repositories use clean project codenames (e.g. `blade-odyssey`, `chronos-runner`).*

### 3. Package Scopes & Internal Project Codes
- **Package Managers:** All shared packages, UI component kits, and tooling published to npm, PyPI, or Docker Hub use the official `@ninrise` organization scope (e.g., `@ninrise/ui`, `@ninrise/eslint-config`, `@ninrise/logger`).
- **Internal Ticket & Project Codes:** Projects are linked to the standardized legal/internal prefix `NNR` (e.g., `NNR-CORE`, `NNR-OPS`, `NNR-102`).

### 4. Branch Naming Convention
Branches should clearly indicate their purpose and associate with an issue or ticket:
- `feat/<short-description>` or `feat/NNR-<id>-<short-description>` (e.g., `feat/NNR-102-player-inventory-sync`)
- `fix/<short-description>` or `fix/NNR-<id>-<short-description>` (e.g., `fix/NNR-45-session-token-leak`)
- `refactor/<short-description>` (e.g., `refactor/clean-query-builder`)
- `docs/<short-description>` (e.g., `docs/api-getting-started`)
- `chore/<short-description>` (e.g., `chore/bump-deps`)

### 5. Commit Message Convention
We strictly follow the [Conventional Commits](https://www.conventionalcommits.org/) specification (`<type>(<scope>): <subject>`):
- `feat(auth): add OAuth2 refresh token rotation`
- `fix(gameplay): prevent character clipping during dash animation`
- `docs(readme): add local development setup guide`
- `refactor(db): streamline transaction retry handler`
- `perf(render): optimize tilemap batching in level 2`
- `test(core): add edge-case unit tests for payload parser`

### 6. Versioning & Releases
All libraries, services, and products follow **Semantic Versioning (SemVer 2.0.0)** (`vMAJOR.MINOR.PATCH[-PRERELEASE]`):
- Git tags are annotated directly from the `main` branch (e.g., `git tag -a v1.2.0 -m "Release v1.2.0"`).
- Changelogs are generated automatically from Conventional Commits.

---

## How to Contribute

### 1. Discuss Major Changes First
For significant features, architectural modifications, or breaking changes, please open an issue first to discuss the design with project maintainers.

### 2. Keep Pull Requests Focused
Keep PRs small, cohesive, and focused on a single concern. Smaller pull requests are easier to review, test, and merge safely.

### 3. Verify Locally
Before submitting a pull request:
- Run all existing test suites and linters.
- Ensure zero compiler, linter, and type-check warnings (`Clang-Tidy`, `Clippy`, `Ruff`, `ESLint`).
- Verify adherence to [CODING_CONVENTIONS.md](CODING_CONVENTIONS.md).
- Confirm documentation is updated to reflect any user-facing changes.

---

## Standard Workflow (Fork & Pull Request)

1. **Fork the Repository:** Create a fork of the target repository under your GitHub account.
2. **Clone Locally:**
   ```bash
   git clone https://github.com/<your-username>/<repo-name>.git
   cd <repo-name>
   ```
3. **Create a Feature Branch:**
   ```bash
   git checkout -b feat/NNR-102-my-new-feature
   # or
   git checkout -b fix/issue-description
   ```
4. **Make Your Changes:** Write clean code, add tests, and follow our conventions.
5. **Commit & Push:**
   ```bash
   git add .
   git commit -m "feat(module): describe change cleanly"
   git push origin feat/NNR-102-my-new-feature
   ```
6. **Open a Pull Request:** Navigate to the original repository on GitHub, open a PR against the default branch, complete the Pull Request template, and submit it for review.

---

## Code of Conduct

All contributors and community members are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

---

*Built with heart by Ninrise.*

# Ninrise • Engineering & Coding Conventions

> *"Resolve right. Build with heart."*

This document outlines the official organization-wide coding standards, design philosophies, and language-specific conventions for **Ninrise** (`github.com/ninrise`).

Every line of code authored under the Ninrise banner should embody our foundational values: **Integrity** (honest, robust code without hidden shortcuts), **Humanity** (accessible, empathetic UX and DX), **Innovation** (clean, modern, flexible architecture), and **Dedication** (sweating the small details, high test coverage, and craftsmanship).

---

## 1. Universal Engineering Philosophies

1. **Clarity Over Cleverness:** Write code for the engineer who maintains it next. Favor self-documenting naming and straightforward control flow over dense, obfuscated "one-liners."
2. **Fail Fast & Explicit Error Handling:** Validate input at system boundaries. Never swallow exceptions or silently ignore errors (e.g., empty `catch` blocks or unhandled `Result::Err`).
3. **Immutability by Default:** Prefer immutable data structures, `const`, and read-only references unless mutation is explicitly required for performance or state management.
4. **Zero Compiler & Linter Warnings:** Code must compile cleanly without warnings before opening a pull request. Treat warnings as errors in CI pipelines.
5. **Security & Secrets Hygiene:** Never hardcode passwords, API tokens, internal endpoints, or private keys. Always retrieve configuration from environment variables or secure secret vaults.

---

## 2. Universal Naming & Style Rules

| Category | Convention | Examples | Notes |
| :--- | :--- | :--- | :--- |
| **Files & Directories** | `kebab-case` | `user-session.ts`, `game-physics.cpp` | Component files in React/Vue may follow `PascalCase` if required by framework convention |
| **Constants & Enums** | `UPPER_SNAKE_CASE` | `MAX_RETRY_COUNT`, `DEFAULT_TIMEOUT_MS` | Global or module-level constants |
| **Booleans** | Prefixed (`is`, `has`, `can`, `should`) | `isActive`, `hasPermission`, `canJump` | Avoid ambiguous names like `open` or `status` |
| **Indentation** | Spaces | 2 spaces (TypeScript, YAML, JSON); 4 spaces (C++, Rust, Python) | Enforced via `.editorconfig` |
| **Line Endings** | `LF` (`\n`) | UTF-8 encoding without BOM | Mandatory across all repositories |

---

## 3. Language-Specific Standards

### 3.1. C++ (Game Development — Ninrise Studio)

Targeting modern standards (**C++20** or higher) for engine core, game mechanics, and performance-critical systems.

* **Memory & RAII:**
  * Strictly avoid raw `new` and `delete`. Use RAII and smart pointers (`std::unique_ptr` by default, `std::shared_ptr` only when ownership is genuinely shared).
  * Beware of cache line misses and heap allocations inside tight render or physics loops. Prefer value semantics and contiguous containers (`std::vector`).
* **Type Safety & Modern Syntax:**
  * Use `nullptr` instead of `NULL` or `0`.
  * Mark non-modifying member functions as `const`.
  * Use `auto` judiciously when the type is obvious from context or redundant (e.g., iterators), but avoid it when it obscures critical types.
  * Prefer `std::string_view` and `std::span` for non-owning read access to buffers and text.
* **Naming Conventions:**
  * Classes, Structs, Enums: `PascalCase` (e.g., `CharacterController`, `TransformComponent`).
  * Member Variables: `m_` prefix or trailing underscore (e.g., `m_health`, `m_meshRenderer`).
  * Methods & Functions: `PascalCase` or `camelCase` (consistent per engine/project convention, e.g., Unreal Engine uses `PascalCase`, custom engines use `camelCase`).

```cpp
// Good: Modern RAII and const-correctness
class PlayerController {
public:
    explicit PlayerController(std::string_view playerName)
        : m_name(playerName) {}

    [[nodiscard]] bool IsAlive() const noexcept {
        return m_health > 0.0f;
    }

    void ApplyDamage(float amount) noexcept {
        m_health = std::max(0.0f, m_health - amount);
    }

private:
    std::string m_name;
    float m_health{100.0f};
};
```

---

### 3.2. Rust (Backend & Infrastructure)

Powering highly concurrent, low-latency microservices, APIs, and critical backend systems.

* **Safety & Idiomatic Rust:**
  * **Zero `unsafe` policy** in application code unless justified with a formal safety audit and documented invariants.
  * Leverage the type system: use the newtype pattern to enforce domain invariants (e.g., `struct UserId(Uuid)` instead of raw `Uuid`).
  * Never use `.unwrap()` or `.expect()` in production paths. Propagate errors using `?` and explicit error types (via `thiserror` or `anyhow`).
* **Concurrency & Async:**
  * Prefer async runtimes (Tokio). Keep lock contention minimal; use message passing (`tokio::sync::mpsc`) or lock-free atomics where appropriate.
  * Avoid long-held synchronous locks across `.await` points.
* **Tooling:**
  * Must pass `cargo fmt --check` and `cargo clippy -- -D warnings`.

```rust
// Good: Domain newtype, explicit error propagation
use thiserror::Error;

#[derive(Debug, Error)]
pub enum AccountError {
    #[error("insufficient funds for withdrawal: required {required}, available {available}")]
    InsufficientFunds { required: u64, available: u64 },
    #[error("account is locked")]
    AccountLocked,
}

pub struct Account {
    balance: u64,
    is_locked: bool,
}

impl Account {
    pub fn withdraw(&mut self, amount: u64) -> Result<(), AccountError> {
        if self.is_locked {
            return Err(AccountError::AccountLocked);
        }
        if self.balance < amount {
            return Err(AccountError::InsufficientFunds {
                required: amount,
                available: self.balance,
            });
        }
        self.balance -= amount;
        Ok(())
    }
}
```

---

### 3.3. Python (Data Engineering, Analytics & AI/ML)

Utilized for data ingestion pipelines, analytical processing, telemetry, and machine learning models.

* **Code Formatting & Type Hints:**
  * **Mandatory Type Hints:** All function signatures and return types must be explicitly typed using `typing` / Python 3.10+ native types (`list[str]`, `str | None`).
  * Adhere strictly to **PEP 8**. Formatting is enforced using `ruff` or `black` (line length: 88 or 100 characters).
* **Performance & Data Handling:**
  * Use vectorized operations (NumPy, Polars, Pandas) rather than explicit Python `for` loops for large datasets.
  * Use generators (`yield`) and streaming readers for processing large log files or datasets to prevent out-of-memory crashes.
  * Use dataclasses or Pydantic models for structured schema validation instead of raw dictionaries.
* **Virtual Environments & Dependencies:**
  * Manage dependencies via `uv` or `poetry`. Never install packages globally.

```python
# Good: Type-annotated, structured schema with dataclass/Pydantic
from dataclasses import dataclass
from typing import Sequence

@dataclass(frozen=True)
class TelemetryEvent:
    event_id: str
    user_id: str
    timestamp: int
    payload: dict[str, float]

def aggregate_metric(events: Sequence[TelemetryEvent], metric_name: str) -> float:
    """Computes the mean value for a given telemetry metric."""
    values = [e.payload[metric_name] for e in events if metric_name in e.payload]
    if not values:
        return 0.0
    return sum(values) / len(values)
```

---

### 3.4. TypeScript (Frontend, UI & Web Applications)

Used for user-facing web applications, administrative consoles, and web tooling.

* **Strictness & Type Integrity:**
  * Strict mode enabled 100% (`"strict": true` in `tsconfig.json`).
  * **No `any` rule:** Never use `any`. Use `unknown` when input is unpredictable and parse/validate at runtime using schema validators like `zod`.
  * Prefer `type` for unions/tuples and `interface` for extendable public APIs and object contracts.
* **Component & State Architecture:**
  * Separate business logic from presentational rendering (hooks/stores vs. pure components).
  * Use immutable state updates. Do not mutate objects or arrays directly.
  * Favor functional components and React Hooks (or framework equivalent).
* **Linting & Formatting:**
  * Enforced with ESLint and Prettier. Run `eslint --max-warnings=0`.

```typescript
// Good: Strict validation, readonly properties, typed async
import { z } from 'zod';

export const UserProfileSchema = z.object({
  id: z.string().uuid(),
  displayName: z.string().min(2),
  email: z.string().email(),
  isActive: z.boolean(),
});

export type UserProfile = z.infer<typeof UserProfileSchema>;

export async function fetchUserProfile(userId: string): Promise<UserProfile> {
  const response = await fetch(`/api/v1/users/${encodeURIComponent(userId)}`);
  if (!response.ok) {
    throw new Error(`Failed to fetch user profile: ${response.statusText}`);
  }
  const rawData: unknown = await response.json();
  return UserProfileSchema.parse(rawData);
}
```

---

## 4. Testing & Quality Assurance

1. **Unit Tests:** Every core algorithm, utility function, and domain logic must be covered by unit tests.
2. **Integration Tests:** Public API routes and critical user workflows (auth, data pipeline, payment) require automated integration tests.
3. **Determinism:** Tests must be deterministic and isolated. Avoid relying on network calls, shared global mutable state, or arbitrary sleeps. Use test doubles or mocks for external services.

---

## 5. Review & Enforcement

All pull requests must undergo automated CI verification and manual peer review:
- CI passes all linters, type checks, and automated test suites.
- Reviewers verify architectural integrity, readability, and adherence to the [Code of Conduct](CODE_OF_CONDUCT.md).

*Resolve right. Build with heart.*

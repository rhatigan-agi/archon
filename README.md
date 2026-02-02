<div align="center">

# Archon

### Reference Implementation of the Anvil Specification

**The AI is not trusted. The compiler is.**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Status](https://img.shields.io/badge/status-in%20development-orange.svg)](#project-status)
[![Anvil](https://img.shields.io/badge/implements-Anvil%20v0.2-purple.svg)](https://github.com/rhatigan-agi/anvil)

[The Problem](#the-problem) • [Why Archon](#why-archon) • [How It Works](#how-it-works) • [Examples](#example-outputs) • [Status](#project-status)

</div>

---

> ⭐ **Star this repo** to get notified when the CLI is released.
>
> 💬 **[Join the Discussions](https://github.com/rhatigan-agi/archon/discussions)** — feedback on philosophy and design welcome now.

---

## The Problem

AI coding tools are remarkable but fundamentally ungoverned:

- They hallucinate file paths that don't exist
- They invent model relationships that aren't in code
- They create duplicate domains when canonical ones exist
- They make security changes without authorization
- They delete tests to make code "pass"
- They have no memory of what was agreed upon

The result: teams don't trust AI output. Every change requires manual review of *everything*.

**Archon doesn't replace these tools—it governs them.**

---

## Why Archon?

Claude Code, Cursor, and Copilot are remarkable tools. Archon adds the governance layer they lack—think of it as a compiler pipeline for AI workflows.

| AI Coding Tool | Limitation | Archon Solution |
|----------------|------------|-----------------|
| **Claude Code** | No file grounding—hallucinates paths | Discovery phase grounds all references |
| **Cursor** | Changes leak outside scope | Phase-based scope enforcement |
| **GitHub Copilot** | Completion-only, no workflow | Full Intent → Commit pipeline |
| **v0 / bolt** | Frontend-only, no backend awareness | Backend-first sequencing validation |
| **Aider** | Git-aware but ungoverned | L0-L3 governance with fixability contracts |

**Archon orchestrates these tools with compiler guarantees.**

---

## The Core Insight

A traditional compiler doesn't have "vibes." It has invariants.

| Compiler Concept | Archon Equivalent |
|------------------|-------------------|
| Source code | Intent |
| Lexer/Parser | Intent → Issue → Plan transformation |
| **Linker** | **4-pass Discovery** |
| Type checker | Governance engine |
| Symbol table | Domain registry |
| Code generation | Execution packets |

> The AI generates. The system verifies. Governance is structural, not behavioral.

**A prompt can be ignored. A compiler invariant cannot.**

---

## About Anvil

Archon is the reference implementation of **[Anvil](https://github.com/rhatigan-agi/anvil)**, an open specification for governed AI-assisted development.

**Anvil** defines:
- Standard phases: Intent → Discovery → Issue → Plan → Execute → Govern
- Governance rule schema (L0-L3 tiers)
- Discovery contract format
- Fixability classification (AUTO / HUMAN / NEVER)
- Event logging schema

**Archon** implements these specifications with:
- Production CLI workflow
- Multi-LLM support (Anthropic, Ollama)
- Language-specific AST scanners (Python, TypeScript)
- Domain registry enforcement
- Immutable audit trails

> Other implementations welcome. See the [Anvil spec](https://github.com/rhatigan-agi/anvil) to build your own.

---

## How It Works

```
Intent → Discovery → Issue → Plan → Validate → Execute → Govern → Commit
                                                  ↑          ↓
                                                  └── Repair ←┘
```

### Discovery — The Linker

Before any AI involvement, Archon runs **4-pass deterministic reconnaissance**:

| Pass | Purpose | Method |
|------|---------|--------|
| **Pass 1** | Framework Detection | Signal-based detection with confidence scores |
| **Pass 2** | Surface Discovery | AST-scanned models, views, components (AUTHORITATIVE) |
| **Pass 3** | Ownership Binding | Which file *executes* vs *renders* the intent |
| **Pass 4** | Schema Sufficiency | Does the schema support this intent? (DETERMINISTIC, non-LLM) |

**Key guarantees:**
- Pass 2 is authoritative—AST-scanned, no LLM interpretation
- Pass 4 is deterministic—pure code analysis, no inference
- If `migration_required: false`, the planner is blocked from proposing migrations
- The AI cannot reference files that weren't discovered

### User Resolution

When genuine ambiguity exists, Archon stops and asks:

```
## Frontend Placement Required

Your intent introduces new UI work, but multiple valid targets exist.

1. apps/frontend/components/ProjectDetailModal.tsx
2. apps/frontend/views/ProjectListView.tsx

Your choice: 1

Resolution recorded.
Frontend owner := ProjectDetailModal.tsx
```

This isn't feedback—it's a **type cast**. The user's choice becomes the only valid option for all subsequent phases.

### Governance Rules

Archon enforces deterministic, machine-checkable rules:

| Rule | Description | Fixability |
|------|-------------|------------|
| **GOV-001** | No new models without explicit intent | 🟡 HUMAN |
| **GOV-002** | No auth/security changes without security intent | 🟡 HUMAN |
| **GOV-003** | No production infra mutation from non-infra intent | 🟡 HUMAN |
| **GOV-004** | No test deletion without justification | 🟡 HUMAN |
| **GOV-005** | Phase writes files outside declared scope | 🔴 NEVER |
| **GOV-006** | No modifications to generated files | 🔴 NEVER |
| **GOV-007** | No file creation outside phase scope | 🔴 NEVER |
| **GOV-008** | No duplicate domain definitions | 🔴 NEVER |
| **GOV-009** | Canonical domain names enforced | 🔴 NEVER |

### The Fixability Contract

| Classification | Behavior |
|----------------|----------|
| 🟢 **AUTO** | System repairs automatically |
| 🟡 **HUMAN** | Workflow pauses for decision |
| 🔴 **NEVER** | Abort—fundamental violation |

**NEVER means NEVER.** The system cannot proceed. This prevents the AI from "fixing" migrations, editing lock files, or inventing domain synonyms.

---

## Example Outputs

The CLI is under development, but the design is concrete. See [`/examples`](./examples) for real artifacts generated during development:

| Artifact | Description |
|----------|-------------|
| [`01_intent/`](./examples/01_intent/) | Structured intent representation |
| [`02_discovery/`](./examples/02_discovery/) | 4-pass discovery with grounding facts |
| [`03_issue/`](./examples/03_issue/) | Generated issue with acceptance criteria |
| [`04_plan/`](./examples/04_plan/) | Multi-phase execution plan with dependencies |
| [`05_packets/`](./examples/05_packets/) | Handoff packets for Claude Code / Cursor |
| [`06_governance/`](./examples/06_governance/) | Pass/fail verdicts with fixability |
| [`07_timeline/`](./examples/07_timeline/) | Immutable event log |
| [`config/`](./examples/config/) | Example configuration files |

### Example: Discovery Output

```json
{
  "intent_id": "INTENT_007",
  "passes": {
    "pass_2_surface": {
      "source": "deterministic",
      "confidence": "absolute"
    },
    "pass_4_schema": {
      "method": "ast_comparison",
      "llm_used": false
    }
  },
  "schema_sufficiency": {
    "ProjectTask": {"migration_required": false}
  },
  "user_resolution": "apps/frontend/components/ProjectDetailModal.tsx"
}
```

### Example: Governance Failure

```json
{
  "rule": "GOV-009",
  "status": "FAIL",
  "fixability": "NEVER",
  "violation": {
    "file": "apps/frontend/hooks/useProjectTasks.ts",
    "issue": "Interface 'TodoItem' uses alias 'Todo'. Canonical domain: 'Task'"
  },
  "remediation": "Rename to 'TaskItem' to match canonical domain"
}
```

---

## Domain Registry

AI tools fragment codebases with synonyms. Archon enforces a symbol table:

```yaml
# .archon/domains.yaml

domains:
  Task:
    canonical: true
    presentation_aliases:
      - TODO
      - Activity
    backend:
      naming_rule: canonical_only  # Must use "Task"
    frontend:
      allowed_aliases:
        - TODO  # UI can say "TODO"
```

**Enforcement:**

```
✓ Backend: class Task(Model)         → ALLOWED
✗ Backend: class Todo(Model)         → GOV-009 BLOCK (NEVER)
✓ Frontend: <TodoList>               → ALLOWED (alias)
```

---

## Configuration

```
.archon/
├── domains.yaml        # Terminology registry
├── engine.yaml         # AI engine settings
└── governance/
    ├── l0.yaml         # Structural invariants (NEVER)
    ├── l1.yaml         # Project policies (HUMAN)
    └── l2.yaml         # Advisory rules (warnings)
```

See [`/examples/config`](./examples/config/) for complete configuration examples.

---

## Project Status

**Archon** — In Development (Design Preview)

This repo contains:
- ✅ Complete design documentation
- ✅ Example outputs from development builds
- ✅ Configuration schemas
- ✅ Anvil specification alignment
- ⏳ CLI source code (coming soon)

**What's working internally:**
- 4-pass discovery with AST scanning
- Intent → Issue → Plan pipeline
- Governance engine (GOV-001 through GOV-009)
- Domain registry enforcement
- Immutable event timeline
- Packet generation for handoff

**Star this repo** to get notified when the CLI is released.

---

## Anvil Specification

Archon is the reference implementation of **[Anvil](https://github.com/rhatigan-agi/anvil)**, an open specification for deterministic AI-assisted development.

| | Link |
|---|------|
| **Spec** | [github.com/rhatigan-agi/anvil](https://github.com/rhatigan-agi/anvil) |
| **Implementation** | This repo |
| **Versions** | Anvil v0.2 (stable) • Archon (in development) |

Want to build your own Anvil implementation? See the [spec docs](https://github.com/rhatigan-agi/anvil).

---

## Contributing

Feedback welcome now, code contributions when CLI is released:

- **Discussions**: Philosophy, architecture, feature ideas
- **Issues**: Edge cases, governance gaps, design questions

---

## License

MIT

---

<div align="center">

**Built by [Jeff Rhatigan](https://github.com/rhatigan-agi) at [Rhatigan Labs](https://rhatigan.ai)**

*A compiler doesn't have vibes. Neither should your AI coding workflow.*

</div>

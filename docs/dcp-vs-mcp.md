# Decision Context Protocol (DCP) vs Model Context Protocol (MCP)

<div align="center">

[Home](index.md) ·
[Specification](specification.md) ·
[Architecture](architecture.md) ·
[DCP vs MCP](dcp-vs-mcp.md) ·
[Examples](examples.md) ·
[Governance](governance.md)

</div>

---

## Overview

Decision Context Protocol (DCP) and Model Context Protocol (MCP) address
different but complementary concerns in agentic systems.

They are designed to work together, not to compete.

- **DCP** governs *decision authority*
- **MCP** governs *invocation mechanics*

Together, they enable scalable autonomy with enterprise-grade governance.

---

## Core Distinction

| Dimension | DCP | MCP |
|---------|-----|-----|
| Primary concern | Decision authority | Tool invocation |
| Governs | Whether an action may occur | How an action is invoked |
| Evaluates | Autonomous intent | Execution mechanics |
| Enforcement point | Pre-execution | During execution |
| Ownership | System of record | Agent / tool interface |
| Audit artifact | Decision record | Invocation trace |

---

## What DCP Solves

DCP addresses a gap that emerges as agents become autonomous:

- Agents dynamically generate intent
- Tool access implicitly grants execution power
- Decision authority becomes embedded in orchestration logic
- Auditability and accountability degrade at scale

DCP introduces an explicit, protocolized decision boundary where systems
evaluate whether an autonomous action should be permitted in context,
prior to execution.

---

## What MCP Solves

MCP standardizes how agents:

- Discover tools and services
- Exchange structured context
- Invoke capabilities consistently across environments

MCP intentionally assumes that authorization decisions have already been made.

This design choice keeps MCP focused on interoperability and execution
rather than governance.

---

## How DCP and MCP Work Together

A typical enterprise agent flow:

1. Agent forms intent
2. Agent submits proposed action to DCP
3. System evaluates policy, context, risk, and delegation
4. DCP returns a declarative decision (ALLOW / DENY / ESCALATE / CONSTRAIN)
5. If permitted, agent invokes tools via MCP

This separation preserves autonomy while keeping decision authority
explicit, auditable, and system-owned.

---

## Why DCP Is Not "Policy-as-Code"

Traditional policy engines evaluate static access requests.

DCP evaluates **proposed autonomous intent**, which may include:

- Uncertainty and confidence signals
- Situational context
- Temporal validity
- Delegation semantics

DCP returns a contextual commitment, not a standing permission.

---

## Architectural Positioning

DCP and MCP occupy distinct layers:

- **Agent reasoning layer** — intent formation
- **Decision layer (DCP)** — authorization and governance
- **Invocation layer (MCP)** — execution mechanics
- **System of record** — state change

This layering enables what DCP defines as *systems of intelligence*:
distributed autonomy governed by local decision boundaries.

---

## Summary

DCP and MCP are complementary protocols.

- MCP makes agent execution portable
- DCP makes agent autonomy governable

Together, they form a foundation for production-grade agentic systems.

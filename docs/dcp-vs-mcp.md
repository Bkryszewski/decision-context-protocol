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

# DCP vs MCP

This document distinguishes the **Decision Context Protocol (DCP)** from the **Model Context Protocol (MCP)**. Both play roles in governed autonomous systems, but they serve distinct architectural purposes.

## Purpose

- **DCP** defines a standardized decision authority boundary that evaluates autonomous intent against system policies prior to execution.
- **MCP** defines a standardized interface for model-mediated tool invocation and context propagation.

DCP is a **decision authority**.  
MCP is an **execution interface**.

These protocols are complementary, not interchangeable.

---

## Definitions

### Decision Context Protocol (DCP)

DCP governs whether an action *may* be executed.

DCP evaluates:
- Proposed action semantics
- Actor identity
- Execution identity
- Contextual signals (risk, confidence, constraints)

DCP returns a decision outcome that determines whether execution proceeds.

Supported decision outcomes include:
- **ALLOW** — execution may proceed
- **DENY** — execution must not proceed
- **ESCALATE** — requires human or secondary system approval
- **CONSTRAIN** — execution may proceed with specific limits

DCP is independent of how execution is implemented.

### Model Context Protocol (MCP)

MCP standardizes how models or agents:
- Invoke tools or services
- Pass structured context to execution interfaces
- Receive structured results

MCP does not make authorization decisions, nor does it determine whether execution should occur.

---

## Contractual Responsibilities

| Concern | DCP | MCP |
|---------|-----|-----|
| Authorization decision | **Yes** | No |
| Execution interface specification | No | **Yes** |
| Context propagation for execution | Optional | **Yes** |
| Policy evaluation | **Yes** | No |
| Audit record of decision | **Yes** | Optional |

---

## Sequence Semantics

### DCP First

1. Agent or system proposes an action.
2. DCP evaluates intent and returns a decision.
3. Only if decision is `ALLOW` or `CONSTRAIN` does execution proceed.
4. Execution is mediated either via MCP or other mechanisms (API gateway, direct service call).

### MCP Only (Insufficient for Authorization)

1. Agent invokes an execution interface via MCP.
2. Execution may occur without explicit authorization.
3. No standardized decision boundary is enforced.
4. This pattern is insufficient for governance in regulated contexts.

**Rule:** DCP must always precede execution. MCP may be used after authorization.

---

## Integration Patterns

### Pattern: DCP + MCP

- DCP enforces the decision boundary.
- MCP provides a standardized tool invocation interface.
- Decision identifiers and constraints from DCP are propagated into MCP calls.

This pattern is appropriate where:
- Multiple models or agents require consistent execution semantics
- Tool schema standardization is valuable
- Auditability across agents and execution interfaces is required

### Pattern: DCP + API Management

- DCP governs decision authority.
- API management layers enforce execution constraints (e.g., rate limits, routing).
- DCP decision metadata may be passed as request headers.

This pattern is appropriate where:
- MCP is not available
- Existing enterprise API gateways are in place
- Enforcement and telemetry are owned by existing infrastructure

---

## Security and Enforcement Boundaries

- **DCP is a decision enforcement point** and must be owned by a system with the appropriate security context, identity, and policy authority.
- **MCP is an execution interface** and inherits enforcement requirements from upstream decisions.
- Authorization decisions must not be bypassed by direct MCP calls.

---

## Non-Goals

- DCP does not specify a policy language, policy engine, or policy store.
- MCP does not make authorization decisions.
- Neither protocol defines agent planning or reasoning semantics.

---

## Summary

- DCP is the canonical decision authority that evaluates autonomous intent before execution.
- MCP is a standardized execution interface that models or systems use once authorization is granted.
- DCP and MCP serve different purposes and may be used in concert.

---


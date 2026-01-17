# Implementer’s Guide (Non-Normative)

<div align="center">

[Specification](specification.md) ·
[Architecture](architecture.md) ·
[DCP vs MCP](dcp-vs-mcp.md) ·
[Implementers Guide](implementers-guide.md) ·
[Examples](examples.md) ·
[Governance](governance.md)

</div>

---

## Purpose

This document provides **implementation guidance** for Decision Context Protocol (DCP).
It is **non-normative** and does not introduce new protocol requirements.

The goal is to help engineers and architects integrate DCP into existing systems
in a consistent, scalable, and auditable way.

---

## Where DCP Fits in a System

DCP is invoked **after intent formation** and **before execution**.

A typical placement:

1. Agent forms intent  
2. Agent proposes an action  
3. DCP evaluates authorization  
4. Execution proceeds only if authorized  

DCP SHOULD be treated as a **decision boundary**, not as business logic
embedded inside orchestration flows.

---

## Minimal Integration Pattern

A minimal DCP integration requires:

- A **DCP evaluation endpoint** owned by the system of record  
- A **policy evaluation mechanism** (OPA, ABAC, rules engine, or custom logic)  
- A **decision record store** (for auditability)  

### Recommended Responsibility Split

| Component | Responsibility |
|----------|----------------|
| Agent | Propose intent |
| DCP Interface | Evaluate authorization |
| Policy Engine | Apply policy logic |
| System of Record | Own execution and compliance |

---

## Example DCP Request Flow

An agent proposes a transfer action:

```json
{
  "action": {
    "verb": "transfer",
    "resource": "account",
    "parameters": {
      "amount": 5000,
      "currency": "USD"
    }
  },
  "actor_identity": "agent.cashflow.optimizer",
  "execution_identity": "svc.finance.transfer",
  "context": {
    "signals": {
      "confidence": 0.92,
      "risk_score": "medium"
    }
  }
}

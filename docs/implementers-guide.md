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
```

### Summary

The system evaluates this request **before invoking any execution APIs**.  
No execution-side effects should occur until a DCP decision is returned.

---

## Handling Decision Outcomes

Implementations SHOULD handle DCP outcomes explicitly and deterministically.

### ALLOW
- Proceed to execution
- Invoke tools or services as proposed
- Record the decision identifier alongside execution telemetry

### DENY
- Do not execute the proposed action
- Surface the denial reason to the agent or operator
- Do not retry without a material change in context

### ESCALATE
- Route the request to a human reviewer or secondary control system
- Preserve the full decision context and rationale
- Resume execution only after explicit approval

### CONSTRAIN
- Enforce constraints programmatically at execution time
- Reject any execution attempt that violates the imposed limits
- Record both the constraints and enforcement outcome

---

## Policy Engine Integration

DCP does not mandate a specific policy engine.

Common integration patterns include:
- Attribute-Based Access Control (ABAC)
- Open Policy Agent (OPA)
- Rules engines
- Domain-specific evaluators

The policy engine SHOULD:
- Receive the complete DCP request
- Evaluate policy against current context
- Return a decision outcome and rationale
- Be versioned and auditable

---

## Decision Record Storage

Every DCP evaluation SHOULD produce a **decision record**.

Decision records SHOULD include:
- Full request payload
- Decision outcome
- Applied policy version
- Timestamp
- Validity window

Decision records SHOULD be:
- Immutable
- Replay-protected
- Retained according to regulatory and organizational requirements

---

## Latency and Failure Handling

Recommended practices include:
- Use synchronous evaluation for high-risk or regulated actions
- Allow asynchronous evaluation for low-risk actions
- Default to **fail-closed** behavior for safety-critical systems
- Cache decisions only within their defined validity windows

---

## Relationship to MCP

When used with Model Context Protocol (MCP):

- DCP evaluates **whether** an action may occur
- MCP governs **how** the action is invoked

A compliant system MUST NOT invoke tools via MCP
without first receiving an **ALLOW** or **CONSTRAIN** decision from DCP.

---

## Testing and Validation

Implementers SHOULD:
- Validate all DCP messages against published schemas
- Unit test policy outcomes independently of agents
- Simulate failure modes (timeouts, partial context, policy unavailability)

---

## Common Implementation Mistakes

Avoid the following:
- Treating DCP as a logging or observability layer
- Evaluating decisions after execution
- Embedding policy logic inside agent prompts
- Allowing tool access to imply authorization

---

## Final Summary

DCP enables scalable autonomy by making decision authority explicit,
auditable, and system-owned.

Successful implementations treat DCP as a **first-class architectural
boundary**, not an afterthought.


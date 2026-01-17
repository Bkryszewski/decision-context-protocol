# Decision Context Protocol (DCP) v1 — Core Specification

<div align="center">

**Status:** Draft v1.0  
**Normative**

</div>

---

## 1. Scope

This document defines the **normative protocol semantics** for
Decision Context Protocol (DCP).

DCP specifies how autonomous intent MUST be evaluated prior to execution
by systems that own the effects of an action.

Architectural context for this specification is defined in
[Figure 1–3](../architecture.md).

---

## 2. Protocol Role

DCP operates at the decision boundary between:

- Agent reasoning systems
- Systems of record that own execution authority

DCP does not define:
- Agent planning algorithms
- Tool invocation mechanics
- Model architectures

---

## 3. Conformance Language

The key words **MUST**, **MUST NOT**, **SHOULD**, and **MAY** are to be
interpreted as described in RFC 2119.

---

## 4. Decision Context Request

A DCP request MUST include:

1. Proposed action descriptor
2. Originating actor identity
3. Execution identity
4. Context bundle
5. Temporal validity

---

## 5. Decision Outcomes

A DCP evaluation MUST return one of:

- **ALLOW**
- **DENY**
- **ESCALATE**
- **CONSTRAIN**

Each outcome MUST include:
- Decision identifier
- Rationale reference
- Validity window

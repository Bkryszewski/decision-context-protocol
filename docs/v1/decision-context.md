# Decision Context Protocol (DCP) v1 — Core Specification

<div align="center">

**Status:** Draft v1.0  
**Normative**

</div>

---

## 1. Scope

This specification defines the **normative protocol semantics** for
Decision Context Protocol (DCP).

DCP defines a pre-execution decision evaluation interaction by which
a system **MUST** determine whether a proposed autonomous action
is authorized to execute under current policy and context.

Architectural context for this specification is defined in
[Figure 1–3](../architecture.md).

---

## 2. Protocol Role

DCP operates at the decision boundary between agent reasoning systems
and systems that own execution authority.

DCP **MUST NOT** define:
- Agent planning algorithms
- Tool invocation mechanics
- Model architectures or training

DCP **MUST** be invoked prior to execution when an agent proposes
an action that would mutate system state.

---

## 3. Conformance Language

The key words **MUST**, **MUST NOT**, **SHOULD**, and **MAY**
are to be interpreted as described in RFC 2119.

All requirements expressed using these terms are normative
unless explicitly labeled as informative.

---

## 4. Decision Context Request

A DCP request **MUST** represent a single proposed action.

A DCP request **MUST** include:

1. A proposed action descriptor
2. An originating actor identity
3. An execution identity under which the action would occur
4. A context bundle sufficient for evaluation
5. A temporal scope defining validity expectations

A DCP request **MAY** include additional contextual signals
such as confidence, uncertainty, or risk indicators.

---

## 5. Decision Outcomes

A DCP evaluation **MUST** return exactly one of the following outcomes:

- **ALLOW** — execution is authorized as proposed
- **DENY** — execution is not authorized
- **ESCALATE** — execution requires secondary review
- **CONSTRAIN** — execution is authorized within explicit limits

A DCP response **MUST** include:
- A unique decision identifier
- A reference to the applied rationale or policy
- A validity window defining how long the decision applies

Execution **MUST NOT** occur outside the bounds of the returned decision.

---

## 6. Informative Notes (Non-Normative)

The following guidance is informative and does not define protocol
requirements.

- DCP may be implemented synchronously or asynchronously
- Decision caching strategies are implementation-specific
- Policy engines may vary across domains and organizations

## Schema Artifacts

The normative JSON schemas for this specification are defined in:

- [DCP Request Schema](dcp-request.schema.json)
- [DCP Response Schema](dcp-response.schema.json)
- [Decision Record Schema](decision-record.schema.json)

Conforming implementations **MUST** produce and consume messages that
validate against these schemas.


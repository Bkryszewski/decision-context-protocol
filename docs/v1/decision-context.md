# Decision Context Protocol (DCP) v1 — Core Specification

<div align="center">

**Status:** Draft v1.0  
**Normative**

</div>

---

## 1. Scope

This specification defines the **normative protocol semantics** for the
Decision Context Protocol (DCP).

DCP defines a pre-execution decision evaluation interaction by which
a system **MUST** determine whether a proposed autonomous action
is authorized to execute under current policy, context, and delegation
constraints.

This specification defines protocol semantics only. Architectural
context and deployment considerations are described in
[`architecture.md`](architecture.md).

---

## 2. Protocol Role

DCP operates at the **decision boundary** between systems that generate
autonomous intent and systems that own execution authority.

DCP **MUST** be invoked prior to execution whenever an agent or automated
system proposes an action that would mutate system state, initiate
external effects, or exercise delegated authority.

DCP **MUST NOT** define or prescribe:
- Agent planning or reasoning algorithms
- Tool invocation mechanics
- Transport protocols
- Model architectures or training methodologies

DCP evaluates **whether** an action may occur. It does not define
**how** execution is performed.

---

## 3. Conformance Language

The key words **MUST**, **MUST NOT**, **REQUIRED**, **SHALL**, **SHALL NOT**,
**SHOULD**, **SHOULD NOT**, **RECOMMENDED**, **MAY**, and **OPTIONAL**
are to be interpreted as described in RFC 2119.

All requirements expressed using these terms are normative unless
explicitly labeled as informative.

---

## 4. Decision Context Request

A DCP request **MUST** represent exactly one proposed action.

A DCP request **MUST** include the following elements:

1. **Proposed Action Descriptor**  
   A structured description of the action being proposed, including
   the operation to be performed and the target resource.

2. **Actor Identity**  
   The identity of the agent, system, or delegated entity proposing
   the action.

3. **Execution Identity**  
   The identity under which the action would execute if authorized.
   This identity represents the authority exercised during execution.

4. **Context Bundle**  
   Contextual information sufficient for authorization evaluation,
   including relevant signals, evidence, or environmental state.

5. **Temporal Scope**  
   A declaration of validity expectations for the proposed action,
   including time sensitivity or expiration semantics.

A DCP request **MAY** include additional contextual signals such as:
- Confidence or uncertainty indicators
- Risk classification or blast-radius estimates
- References to supporting evidence or prior decisions

---

## 5. Identity and Delegation Semantics

DCP distinguishes between **actor identity** and **execution identity**.

- The **actor identity** identifies the entity proposing intent.
- The **execution identity** identifies the authority under which
  execution would occur.

These identities **MAY** differ. Such differences represent delegation
and **MUST** be evaluated as part of the authorization decision.

DCP **MUST NOT** assume trust relationships implicitly. Authorization
decisions **MUST** be made explicitly based on supplied identities,
context, and policy.

Identity authentication mechanisms and credential verification are
out of scope for this specification and are implementation-defined.

---

## 6. Decision Outcomes

A DCP evaluation **MUST** return exactly one of the following outcomes:

- **ALLOW**  
  Execution is authorized as proposed.

- **DENY**  
  Execution is not authorized and **MUST NOT** occur.

- **ESCALATE**  
  Execution requires secondary review or explicit approval before
  proceeding.

- **CONSTRAIN**  
  Execution is authorized only within explicitly defined limits.

A DCP response **MUST** include:
- A globally unique decision identifier
- A reference to the applied policy or rationale
- A validity window defining the temporal bounds of the decision

Execution **MUST NOT** occur:
- Without a decision
- After a decision’s validity window has expired
- Outside the bounds defined by a constrained decision

---

## 7. Escalation Semantics

When a decision outcome is **ESCALATE**:

- Execution **MUST NOT** proceed until escalation is resolved
- The full decision context **MUST** be preserved
- Resolution **MUST** result in a subsequent authorization decision
  (ALLOW, DENY, or CONSTRAIN)

Escalation resolution mechanisms are implementation-defined but
**MUST** preserve auditability and decision traceability.

---

## 8. Decision Records

Each DCP evaluation **MUST** result in the creation of a decision record.

Decision records **MUST** capture:
- The full decision context request
- The decision outcome
- The applied policy or rationale reference
- The decision identifier
- The validity window
- A timestamp of evaluation

Decision records **MUST** be immutable and replay-protected.
They serve as the authoritative source for audit, investigation,
and post-incident reconstruction.

---

## 9. Temporal Semantics

Decision validity **MUST** be explicitly bounded.

- Decisions **MUST NOT** be reused beyond their validity window
- Expired decisions **MUST** be treated as non-authorizations
- Cached decisions **MUST** respect validity constraints

The representation of temporal scope is schema-defined.

---

## 10. Failure Semantics

If a DCP evaluation cannot be completed due to error, timeout, or
indeterminate state:

- Execution **MUST NOT** proceed by default
- Implementations **SHOULD** treat indeterminate outcomes as DENY
  for safety-critical or regulated actions

Failure handling policies are implementation-defined but **MUST**
be deterministic and auditable.

---

## 11. Informative Notes (Non-Normative)

The following guidance is informative and does not define protocol
requirements:

- DCP may be implemented synchronously or asynchronously
- Policy engines may vary across domains and organizations
- Decision caching strategies are implementation-specific

---

## 12. Schema Artifacts

The normative JSON schemas for this specification are defined in:

- [`dcp-request.schema.json`](dcp-request.schema.json)
- [`dcp-response.schema.json`](dcp-response.schema.json)
- [`decision-record.schema.json`](decision-record.schema.json)

Conforming implementations **MUST** produce and consume messages that
validate against these schemas.

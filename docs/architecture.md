<div align="center">

[Home](index.md) ·
[Specification](specification.md) ·
[Architecture](architecture.md) ·
[DCP vs MCP](dcp-vs-mcp.md) ·
[Examples](examples.md) ·
[Governance](governance.md)

</div>

## Layered Architecture

<img width="956" height="506" alt="image" src="https://github.com/user-attachments/assets/804329f9-1685-470c-9503-86788d23d966" />


**Figure 1** illustrates the architectural position of Decision Context Protocol (DCP)
within an enterprise agentic system.

+-----------------------------------------------------+
|                 Agent Reasoning Layer                |
|  (goal formation, planning, reflection, intent gen) |
+---------------------------+-------------------------+
                            |
                            | Proposed Action
                            v
+-----------------------------------------------------+
|            Decision Context Protocol (DCP)           |
|  (policy evaluation, context, risk, delegation)     |
+---------------------------+-------------------------+
                            |
                            | Authorized Decision
                            v
+-----------------------------------------------------+
|        Invocation Layer (MCP or equivalent)          |
|  (tool discovery, invocation, execution mechanics) |
+---------------------------+-------------------------+
                            |
                            v
+-----------------------------------------------------+
|              System of Record / Execution            |
|  (state mutation, compliance ownership)             |
+-----------------------------------------------------+

**Figure 1 — DCP within a layered enterprise architecture.**  
Agents generate autonomous intent but do not directly execute actions.
DCP evaluates whether execution is permitted in context, prior to invocation.
Tool invocation protocols such as MCP are only engaged after authorization
has been granted by the system that owns execution authority.

## Decision Evaluation Sequence

**Figure 2** shows the runtime interaction between an agent and a system
using DCP prior to execution.
Agent            DCP Interface          System of Record
  |                     |                        |
  | Form intent          |                        |
  |-------------------->|                        |
  |                     | Evaluate context       |
  |                     | policy, risk, identity |
  |                     |---------------------->|
  |                     |                        |
  |                     | Decision outcome       |
  |                     |<----------------------|
  | Receive decision    |                        |
  |<--------------------|                        |
  |                     |                        |
  | If allowed, invoke tool via MCP               |

  **Figure 2 — Pre-execution decision evaluation flow.**  
DCP introduces an explicit evaluation step prior to execution.
Authorization is determined before invocation occurs, preserving
system-of-record ownership and enabling auditable decision records.

## Decision Outcome Model

**Figure 3** represents the possible outcomes of a DCP evaluation.
              +-----------+
              |  PROPOSE  |
              +-----------+
                     |
                     v
        +---------+  +-----------+  +------------+  +-------------+
        |  ALLOW  |  |   DENY    |  |  ESCALATE  |  |  CONSTRAIN  |
        +---------+  +-----------+  +------------+  +-------------+
            |              |              |                 |
            v              v              v                 v
       Execute        No execution   Secondary review   Execute within limits

**Figure 3 — DCP decision outcomes.**  
DCP evaluations return declarative outcomes that determine execution
behavior. Outcomes may permit execution, deny execution, require escalation,
or constrain execution within explicit limits.
      







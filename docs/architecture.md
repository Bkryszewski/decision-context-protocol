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


## Decision Evaluation Sequence

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/ec83a994-608b-47ec-8d83-5116592ef783" />


  **Figure 2 — Pre-execution decision evaluation flow.**  
DCP introduces an explicit evaluation step prior to execution.
Authorization is determined before invocation occurs, preserving
system-of-record ownership and enabling auditable decision records.

## Decision Outcome Model

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/d62c3590-428c-4916-9cae-8cfc45fc937c" />

**Figure 3 — DCP decision outcomes.**  
DCP evaluations return declarative outcomes that determine execution
behavior. Outcomes may permit execution, deny execution, require escalation,
or constrain execution within explicit limits.

## DCP Failure and Degradation Paths

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/cbeb2967-317c-4e1d-912d-1320c6a05764" />

**Figure 4 — Failure & Degradation Semantics.**  
DCP failure handling ensures deterministic behavior under degraded conditions, preserving system-of-record authority and auditability.   

## Organizational Ownership & Control Plane View

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/eb605ad3-1d3f-497a-b434-e3f4c7cf93d7" />

**Figure 5 - Organizational Ownership & Control Plane View
DCP establishes a system-owned decision control plane that governs autonomous action across enterprise execution surfaces.






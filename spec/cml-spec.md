# Controlled Mutation Layer (CML) Specification

Version: 0.1  
Status: Draft

---

# 1. Purpose

The Controlled Mutation Layer (CML) defines a structured boundary for authoritative state mutation.

In traditional software systems, state mutations often occur implicitly within application logic. These mutations may not record the reasoning context that produced the change.

CML introduces a formal mutation boundary where:

- contextual signals are evaluated
- policy is applied
- authorization is declared
- state mutation is recorded

Every mutation passing through the CML produces a **Turn**.

A Turn provides a replayable record describing **why a state change occurred**.

---

# 2. Architectural Role

CML operates as the mutation boundary of an **Emergent State Machine (ESM)** system.

Within an ESM architecture:

Signals
↓
State Construction
↓
Projection
↓
Policy Evaluation
↓
Controlled Mutation Layer
↓
Authoritative State Mutation

The CML ensures that all authoritative state changes are emitted as **structured Turn records**.

---

# 3. Design Principles

CML follows several core principles:

### Explicit Mutation

All authoritative state mutations must pass through the CML boundary.

### Structured Decision Records

Every mutation produces a Turn describing the reasoning context of the change.

### Replayability

Turns must contain sufficient information to reconstruct the reasoning that produced the mutation.

### Policy Transparency

Mutations must reference the governing policy or decision framework that authorized the change.

---

# 4. The Turn

A **Turn** is the atomic record emitted by the Controlled Mutation Layer.

It represents a single decision resulting in an authoritative state mutation.

## Required Fields

| Field            | Description                                   |
| ---------------- | --------------------------------------------- |
| `turn_id`        | Unique identifier for the mutation event      |
| `timestamp`      | UTC timestamp (ISO8601)                       |
| `pre_state`      | Authoritative state before mutation           |
| `signals`        | Bounded contextual inputs used for evaluation |
| `policy_version` | Identifier of the governing policy version    |
| `decision`       | Structured decision label                     |
| `post_state`     | Resulting authoritative state                 |

## Example Turn

```json
{
  "turn_id": "7d9e3c9f",
  "timestamp": "2026-03-13T18:02:11Z",
  "pre_state": "ticket_open",
  "signals": {
    "priority": "high",
    "sla_hours_elapsed": 23,
    "customer_tier": "enterprise"
  },
  "policy_version": "support_policy_v2",
  "decision": "escalate_ticket",
  "post_state": "ticket_escalated"
}
5. Turn Requirements

A valid Turn must satisfy the following requirements.

Deterministic Context

Signals included in the Turn must be sufficient to explain the decision.

Immutable Record

Turns are append-only records and must not be modified after emission.

Bounded Signals

Signals should include only the contextual inputs relevant to the decision.

6. Implementation Requirements

An implementation of the Controlled Mutation Layer must:

provide a Turn envelope structure

enforce emission of Turns for authoritative mutations

ensure Turn immutability

ensure globally unique Turn identifiers

support Turn storage or transmission

Language-specific SDKs may implement these requirements.

7. Relationship to Observability

CML differs from traditional observability systems.

Observability systems capture events and metrics.

CML captures decisions that mutate authoritative state.

The Turn record represents a decision boundary, not a log event.

8. Non-Goals

CML does not specify:

internal signal derivation methods

policy design frameworks

projection models

storage implementations

Those concerns belong to the broader ESM architecture.

9. Future Extensions

Possible extensions to this specification include:

distributed Turn coordination

Turn signing / cryptographic verification

Turn replay protocols

multi-system mutation boundaries

10. Summary

The Controlled Mutation Layer introduces a structured mutation boundary for software systems.

By emitting Turn records, systems gain:

replayable decision history

explicit policy boundaries

auditable state transitions

deterministic mutation control
```

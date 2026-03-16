# Controlled Mutation Layer (CML)

**Version:** 0.1.0  
**Status:** Draft specification

**A decision boundary architecture for safe, replayable state mutation.**

The Controlled Mutation Layer (CML) is an architectural boundary for **authoritative state mutation**.

Instead of allowing application code to mutate state implicitly, CML requires every mutation to pass through a structured decision boundary.

Each mutation becomes a **Turn** — a replayable record containing the signals, policy, and decision that produced the change.

CML is the mutation boundary used by the **Emergent State Machine (ESM)** architecture.

## Getting Started

New to CML?

➡️ Getting Started (30 Minutes)
(link once the guide is ready)

This walkthrough demonstrates how to instrument a mutation boundary and emit structured Turn objects.

## Reference SDKs

CML is specification-first. SDKs provide reference implementations of the Turn envelope and mutation boundary instrumentation.

- Python SDK (reference implementation)
  https://github.com/controlled-mutation-layer/sdk-python

Additional language SDKs may be added over time.

## What is the Controlled Mutation Layer?

The Controlled Mutation Layer is the architectural boundary where:

- Signals are evaluated
- Policy is applied
- Authorization is declared
- State mutation is recorded
- The decision becomes replayable

CML is not logging and it is not observability.

It is the structured decision boundary before authoritative state changes.

In an ESM system, CML is the place where a Turn is emitted.

## Atomic Unit: The Turn

Every mutation passing through CML is captured as a structured object called a Turn.

A Turn records the reasoning context for a state mutation.

Typical fields include:

- turn_id — unique identifier
- timestamp — UTC ISO8601 string
- pre_state — state before mutation
- signals — bounded contextual inputs
- policy_version — governing policy version
- decision — structured decision label
- post_state — resulting state after mutation

Turns provide a replayable decision record.

If a system cannot answer:

“Why did this state change?”

then the system does not have a Turn.

## Relationship to the Emergent State Machine

CML is the mutation boundary of the Emergent State Machine architecture.

Within the ESM model:

```text
Signals
↓
State Construction
↓
Projection
↓
Policy Evaluation
↓
CML Boundary
↓
State Mutation (Turn emitted)
```

The CML ensures that every authoritative state change is instrumented as a Turn.

This enables deterministic replay, auditability, and controlled system evolution.

## Repositories

- ESM Specification (architecture and theory)
  https://github.com/controlled-mutation-layer/esm-spec

- Python SDK (reference implementation)
  https://github.com/controlled-mutation-layer/sdk-python

## Design Principle

Mutation is inevitable.

Structure is optional.

CML makes mutation structured.

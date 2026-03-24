# Controlled Mutation Layer (CML)

**A decision boundary architecture for governed, replayable state mutation.**

**Version:** 1.1.0  
**Status:** Draft specification

The Controlled Mutation Layer (CML) is an architectural boundary for **authoritative state mutation**.

Instead of allowing application code to mutate state implicitly, CML requires every mutation to pass through a structured decision boundary.

Each mutation becomes a **turn** — a replayable record containing the signals, policy, and decision that produced the change.

CML is the mutation boundary used by the **Emergent State Machine (ESM)** architecture.

## Positioning

CML is to **authoritative state mutation** what OpenTelemetry is to **observability**.

OpenTelemetry standardizes how systems emit traces, metrics, and logs.

CML standardizes how systems emit **Turn records** when authoritative state changes.

Where observability answers:

- What happened?
- Where did it happen?
- How long did it take?

CML answers:

- Why did authoritative state change?
- What policy governed the change?
- What decision produced the mutation?

## Getting Started

New to CML?

➡️ [Getting Started (30 Minutes)](guides/getting-started.md)

This walkthrough demonstrates how to instrument a mutation boundary and emit structured Turn objects.

## Reference SDKs

CML is specification-first. SDKs provide reference implementations of the Turn envelope and mutation boundary instrumentation.

- Python SDK (reference implementation)
  https://github.com/controlled-mutation-layer/sdk-python

Additional language SDKs may be added over time.

## What is the Controlled Mutation Layer?

The Controlled Mutation Layer is the architectural boundary where:

Signals are evaluated
Policy is applied
Authorization is declared
State mutation is executed
The decision is recorded as a Turn

CML is not logging and it is not observability.

It is the structured decision boundary preceding authoritative state change.

In an ESM system, CML is the point at which a Turn is emitted.

## Atomic Unit: The Turn

Every mutation passing through CML is captured as a structured object called a turn.

A turn records the full decision context for a state mutation.

Typical fields include:

turn_id — unique identifier
timestamp — UTC ISO8601 string
pre_state — state before mutation
signals — bounded contextual inputs
policy_version — governing policy version
decision — structured decision label
post_state — resulting state after mutation

Turns provide a replayable and auditable decision record.

If a system cannot answer:

“Why did this state change?”

then the system does not have a turn.

## Relationship to the Emergent State Machine

CML is the mutation boundary of the Emergent State Machine (ESM) architecture.

Within the ESM model:

Signals
↓
State Construction
↓
Projection
↓
Relevance Determination
↓
Policy Evaluation
↓
CML Boundary
↓
State Mutation (Turn emitted)

CML ensures that every authoritative state change is:

explicitly authorized
structurally recorded
deterministically replayable

This enables auditability, governance, and controlled system evolution.

## Repositories

- ESM Specification (architecture)
  https://github.com/emergent-state-machine/esm-spec

- Python SDK (reference implementation of the CML specificationn; demonstrates Turn emission and mutation boundary enforcement)
  https://github.com/controlled-mutation-layer/sdk-python

## Design Principle

Mutation is inevitable.

Structure is optional.

CML makes mutation governed, explicit, and replayable.

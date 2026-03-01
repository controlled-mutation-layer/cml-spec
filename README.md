# Controlled Mutation Layer (CML)

Modern AI systems mutate authoritative state continuously.

Refunds are issued.
Accounts are frozen.
Tickets are escalated.
Medical cases are triaged.

Every state mutation passes through a boundary — whether that boundary is explicit or implicit.

The **Controlled Mutation Layer (CML)** makes that boundary explicit.

---

## Getting Started

New to CML?

➡️ [Getting Started (30 Minutes)](guides/getting-started.md)

---

## What is the Controlled Mutation Layer?

The Controlled Mutation Layer is the architectural boundary where:

- Signals are evaluated
- Policy is applied
- Authorization is declared
- State mutation is recorded
- The mutation becomes replayable

It is not logging.
It is not observability.
It is the structured decision boundary before authoritative state changes.

---

## Atomic Unit: The Turn

Every mutation passing through CML is captured as a structured object called a **Turn**.

A Turn includes:

- `turn_id` — unique identifier
- `timestamp` — UTC ISO8601 string
- `pre_state` — state before mutation
- `signals` — bounded metadata describing context
- `policy_version` — governing policy version
- `decision` — structured decision label
- `post_state` — state after mutation

If you can’t answer “Why did this change?” — you don’t have a Turn.

---

## Repositories

- **Spec / Philosophy**  
  https://github.com/controlled-mutation-layer/esm-spec

- **Python SDK (Reference Implementation)**  
  https://github.com/controlled-mutation-layer/sdk-python

---

## Design Principle

Mutation is inevitable.  
Structure is optional.

CML makes mutation structured.

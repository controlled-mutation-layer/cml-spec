# Controlled Mutation Layer (CML)

Modern AI systems mutate authoritative state continuously.

Refunds are issued.
Accounts are frozen.
Tickets are escalated.
Medical cases are triaged.

Every state mutation passes through a boundary — whether that boundary is explicit or implicit.

The **Controlled Mutation Layer (CML)** makes that boundary explicit.

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

- Pre-state
- Signals
- Policy version
- Confidence / volatility (optional)
- Authorized mutation
- Post-state

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

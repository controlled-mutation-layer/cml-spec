# Getting Started with CML (30 Minutes)

This guide shows how to adopt the **Controlled Mutation Layer (CML)** in an existing system.

You do not need to rewrite your architecture.

You only need to instrument one boundary.

---

## What is CML?

CML instruments the moment your system commits an authoritative state change.

It turns that mutation into a structured object called a **Turn**.

If your system cannot answer:

> “Why did this change?”

Then you do not yet have a Turn.

---

## Step 1 (5 min): Identify Your First Mutation Boundary

Write one sentence:

> “The mutation boundary is where we commit changes to **\_\_**.”

Examples:

- “refund policy record in Postgres”
- “account status transition”
- “case escalation event publish”
- “threshold update in config store”

Pick one boundary only. Start small.

---

## Step 2 (5 min): Define `pre_state` and `post_state`

You do not need your entire domain object.

Capture only the portion of state that explains the mutation.

Example:

```json
{
  "refund_policy": "v1",
  "max_refund": 50
}

After mutation:

{
  "refund_policy": "v2",
  "max_refund": 75
}

Your Turn should make drift debuggable in a single query.

Step 3 (5 min): Define signals

signals is bounded metadata that describes context.

It answers:

“What context did we have when we mutated?”

Recommended starter fields:

actor — who or what initiated the change

entity — which logical target is being mutated

reason_codes — short, enumerable reason codes

correlation_id — request or trace ID (if available)

Example:

{
  "actor": "ops:alice",
  "entity": "refund_policy",
  "reason_codes": ["policy_update"],
  "correlation_id": "req_123"
}

Start small. Expand later.

Step 4 (5 min): Emit a Turn at the Boundary

At the commit point:

Capture pre_state

Compute post_state

Construct a Turn

Persist or emit it (log, file, DB, event stream)

Apply the mutation

Python Example
import datetime
from uuid import uuid4
from cml import Turn

pre_state = {"refund_policy": "v1", "max_refund": 50}
post_state = {"refund_policy": "v2", "max_refund": 75}

turn = Turn(
    turn_id=str(uuid4()),
    timestamp=datetime.datetime.now(datetime.timezone.utc).isoformat(),
    pre_state=pre_state,
    signals={
        "actor": "ops:test",
        "entity": "refund_policy",
        "reason_codes": ["policy_update"],
        "correlation_id": "req_123",
    },
    policy_version="v0",
    decision="update_refund_policy",
    post_state=post_state,
)

print(turn.to_json())

Store the Turn wherever your system stores audit events.

Step 5 (5 min): Add a Warn-Only Guard

Before blocking mutations, start by flagging suspicious ones.

Examples:

mutation diff exceeds expected range

invariant violated (refund > order total)

unexpected decision label

mutation rate spike

In v0, log warnings only.

Visibility comes before enforcement.

Step 6 (5 min): Create a Basic Debug Query

You are done when you can answer:

“Show the last 20 Turns for entity X.”

“Which decisions are increasing?”

“Which actors are driving mutations?”

Even grepping JSON logs is acceptable at first.

Success Criteria

CML is successfully installed when:

You can point to your first mutation boundary

Every mutation emits a Turn

Turn contains meaningful pre/post state

signals includes at least actor and reason_codes

You can retrieve Turns by entity and time window

Next Steps

After visibility exists, you can expand toward:

standardized signal schemas

persistent Turn storage

dashboards and anomaly detection

policy simulation and replay

escalation workflows

Do not prebuild.

Instrument one boundary first.
```

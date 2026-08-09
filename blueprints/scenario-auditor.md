# Northfield ticket router — Trick-task board blueprint

Blueprint for running the seven-row board on any message-routing bot. A stranger pastes their bot description, their stakes, and sample messages. The board returns Caught / Slips / Hold for each of seven trick tasks, names the defense that would flip each Slips, and applies the go-live gate.

---

## Intake paste shape

The stranger provides:

1. **Bot name and job** — what the bot does (e.g., routes customer messages to queues)
2. **Clear bar** — the standard the bot must meet (e.g., "A two-problem message opens two tickets.")
3. **Sample messages** — real messages the bot will face (minimum 5, ideally 10)
4. **Source** — where the messages came from (e.g., "Last week's live queue export")

---

## The seven trick tasks

Run each task against the stranger's sample messages. Mark each row:

| Mark | Meaning |
|------|---------|
| **Caught** | The bot's current setup handles this trick correctly |
| **Slips** | The bot fails this trick — needs a defense before ship |
| **Hold** | Cannot evaluate yet — blocked on missing info or config |

### p1 — Bundle split

Does the bot split multi-problem messages into separate tickets?

**Test:** Find a message with two distinct problems. Does the bot open two tickets?

*Worked example from Northfield router:*
> "Where's my order? Also the promo code never applied."

This message has two problems (order status + promo code). The bot must open two tickets. If it opens one, mark **Slips**.

### p2 — Messy harmless

Does the bot handle messy but harmless input without breaking?

**Test:** Find a message with typos, fragments, or casual tone. Does the bot route it correctly?

*Worked example from Northfield router:*
> "It broke again after you fixed it yesterday."

Informal, no explicit queue label. The bot must still route to the correct queue.

### p3 — Mind reader

Does the bot avoid guessing intent that isn't stated?

**Test:** Find a message where the real intent is ambiguous. Does the bot ask for clarification or guess?

*Worked example from Northfield router:*
> "Refund for wrong size — not a shipping question."

The customer explicitly says what it's not. The bot must not route to Shipping.

### p4 — Small quotable

Does the bot preserve the customer's exact words when summarizing?

**Test:** Find a short message. Does the bot's summary quote the customer or paraphrase into something different?

*Worked example from Northfield router:*
> "Store credit never showed; ticket said Refunds owns it."

A one-liner. The bot's summary must quote the customer line or stay blank — not invent details.

### p5 — Hidden library

Does the bot cite its source when answering policy questions?

**Test:** Find a message asking about policy. Does the bot name where it got the answer?

*Worked example from Northfield router:*
> "Can someone escalate? I've been in Billing for three days."

If the bot routes based on escalation policy, it must name that policy source.

### p6 — Goldfish

Does the bot remember context from earlier in the conversation?

**Test:** Find a message that references a prior exchange. Does the bot lose that context?

*Worked example from Northfield router:*
> "Password reset loop — agent told me to email support@."

The customer references what an agent said. The bot must not ignore that prior context.

### p7 — Your own trick task

> It verifies the customer from the call before opening a queue.

**Test:** Does the bot verify customer identity before routing to a queue?

*Worked example from Northfield router:*
> "Billing charged twice; chat said shipping had the tracking."

Before opening a Billing queue ticket, the bot must verify this is actually the account holder.

---

## Use defenses

When a task marks **Slips**, name the defense that would flip it to **Caught**:

| Defense ID | Label | What it catches |
|------------|-------|-----------------|
| `split_bundles` | Force a split when there are two jobs | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| `rewrite_mind_read` | Ban mind-reading verbs | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| `name_source` | Require a quoted source line | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

Current defense state for Northfield router:
- `split_bundles`: **off**
- `rewrite_mind_read`: **on**
- `name_source`: **off**

---

## Go-live gate

**Gate rule:** Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** Ship stops when **2** or more tasks mark Slips.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Board output shape

After running all seven tasks, return:

```
Task          | Mark    | Defense (if Slips)
--------------|---------|-------------------
p1_bundle     | Caught  | —
p2_messy      | Slips   | [defense_id]
p3_mind_read  | Slips   | [defense_id]
p4_quotable   | Slips   | [defense_id]
p5_library    | Slips   | [defense_id]
p6_goldfish   | Slips   | [defense_id]
p7_your_own   | Hold    | —

Slips count: X
Go-live: [SHIP / HOLD]
Re-run: Re-run after policy / FAQ change — plus a biweekly floor.
```

If Slips count ≥ 2, go-live = **HOLD**. Each Slips row must name its defense and an owner before ship.

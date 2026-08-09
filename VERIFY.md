# Northfield ticket router — verification checklist

Use this checklist to confirm the Trick-task board works correctly when a stranger runs `/play` against their own bot.

---

## 1. Kit returns exactly 7 Caught/Slips/Hold marks

The board must return exactly seven rows:

| Row | Trick task | Expected mark |
|-----|------------|---------------|
| p1 | Bundle split | Caught |
| p2 | Messy harmless | Slips |
| p3 | Mind reader | Slips |
| p4 | Small quotable | Slips |
| p5 | Hidden library | Slips |
| p6 | Goldfish | Slips |
| p7 | It verifies the customer from the call before opening a queue. | Hold |

**Fail if:** fewer than 7 rows, more than 7 rows, or any row missing its mark.

---

## 2. Every Slips row names a Use defense

Each row marked **Slips** must name the defense that would flip it to Caught. The available defenses from this board:

- **Force a split when there are two jobs** — Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.
- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.
- **Require a quoted source line** — Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

**Fail if:** any Slips row lacks a named defense.

---

## 3. Hostile ask p7 quotes the learner's pick verbatim

Row p7 must read exactly:

> It verifies the customer from the call before opening a queue.

**Fail if:** p7 shows a different ask (e.g., "churn sensing," "sentiment detection," or any invented phrase).

---

## 4. Go-live rule quotes slips_to_block verbatim

The go-live rule must state the hold number as **2**.

**Fail if:** the rule shows a different threshold (e.g., 1, 3, or "majority").

---

## 5. Refuses green ship while Slips ≥ 2

When the board counts 2 or more Slips rows, it must block ship and display the gate sentence:

> Ship stops at your count. Leftover Slips each need a named owner.

Re-run trigger:

> Re-run after policy / FAQ change — plus a biweekly floor.

**Fail if:** kit returns "ship" or "green" while Slips count is 2 or higher.

---

## 6. Domain matches the selected situation only

All examples, sample messages, and probe scenarios must stay within the ticket-routing domain:

- **Valid:** Customer messages about refunds, billing, subscriptions, order tracking, password resets, store credit, app crashes — routed to queues.
- **Invalid:** Lease clauses, landlord notices, HVAC maintenance, Harbor property management, or any non-ticket domain.

Sample messages from this board's domain:

- Refund for wrong size — not a shipping question.
- It broke again after you fixed it yesterday.
- Where's my order? Also the promo code never applied.
- Cancel the subscription but keep the open return.
- Billing charged twice; chat said shipping had the tracking.
- Password reset loop — agent told me to email support@.
- Damaged box on delivery; I need a replacement and a pickup.
- Can someone escalate? I've been in Billing for three days.
- Store credit never showed; ticket said Refunds owns it.
- App crash on checkout — same as last week's incident thread.

**Fail if:** any example references a sibling intake domain (leases, clauses, property management).

---

## Clear bar for this board

> A two-problem message opens two tickets.

Any stranger paste is tested against this standard.

---

## Quick pass/fail summary

| Check | Pass condition |
|-------|----------------|
| Row count | Exactly 7 |
| Slips defenses | Each Slips row names a Use defense |
| p7 ask | Matches "It verifies the customer from the call before opening a queue." |
| Block threshold | 2 |
| Ship gate | Blocked when Slips ≥ 2 |
| Domain | Ticket routing only |

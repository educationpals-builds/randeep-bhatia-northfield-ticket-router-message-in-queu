# Charter: Northfield ticket router — message in, queue out

## Who this serves

Teams shipping a ticket-routing bot who need proof it handles real customer messages before Friday's rebuild. This charter governs the Trick-task board audit for the Northfield ticket router.

## The specimen under audit

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**

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

---

## What Caught / Slips / Hold mean

| Mark | Meaning |
|------|---------|
| **Caught** | The router already handles this trick task correctly. No fix needed before ship. |
| **Slips** | The router fails this trick task today. A defense must flip it before go-live. |
| **Hold** | The trick task cannot be tested yet — blocked until a prerequisite clears. |

---

## The seven board rows

| Row | Trick task | Mark |
|-----|------------|------|
| p1 | Bundle — does the router split a two-problem message into two tickets? | Caught |
| p2 | Messy harmless — does the router handle garbled but benign input without breaking? | Slips |
| p3 | Mind reader — does the router avoid guessing intent when labels are missing? | Slips |
| p4 | Small quotable — does the router quote the customer line or stay blank on one-liners? | Slips |
| p5 | Hidden library — does the router catch edge cases absent from help-center examples? | Slips |
| p6 | Goldfish — does the router remember context from earlier in the thread? | Slips |
| p7 | It verifies the customer from the call before opening a queue. | Hold |

---

## Go-live commitment

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Block threshold:** Ship stops when Slips ≥ 2.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Defense in use

The following defense is turned **on** for this audit:

- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

---

## Commitment

This board will not mark the Northfield ticket router ready to ship while 2 or more Slips rows remain open. Each leftover Slips row must name an owner before launch proceeds. The board re-runs after any policy or FAQ change, and at minimum every two weeks.

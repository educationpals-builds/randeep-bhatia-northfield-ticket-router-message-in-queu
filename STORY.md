# Northfield ticket router — message in, queue out

## The bot under audit

This board ran against the **Northfield ticket router — message in, queue out**. The router takes each customer message and assigns it to a queue. It already ran on real tickets. The audit proves whether it can ship before Friday's rebuild.

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

---

## Sample messages tested

These ten messages from the live queue export drove the board:

1. Refund for wrong size — not a shipping question.
2. It broke again after you fixed it yesterday.
3. Where's my order? Also the promo code never applied.
4. Cancel the subscription but keep the open return.
5. Billing charged twice; chat said shipping had the tracking.
6. Password reset loop — agent told me to email support@.
7. Damaged box on delivery; I need a replacement and a pickup.
8. Can someone escalate? I've been in Billing for three days.
9. Store credit never showed; ticket said Refunds owns it.
10. App crash on checkout — same as last week's incident thread.

---

## The seven trick tasks and what slipped

| Row | Trick task | Mark | What happened |
|-----|------------|------|---------------|
| p1 | Bundle | **Caught** | Router correctly identified multi-issue messages. |
| p2 | Messy harmless | **Slips** | Router stumbled on harmless noise in messages. |
| p3 | Mind reader | **Slips** | Router inferred intent without explicit queue labels. |
| p4 | Small quotable | **Slips** | Router summarized without quoting the customer line. |
| p5 | Hidden library | **Slips** | Router missed references to prior tickets or threads. |
| p6 | Goldfish | **Slips** | Router forgot context from earlier in the same thread. |
| p7 | It verifies the customer from the call before opening a queue. | **Hold** | Blocked — router does not verify caller identity before queue assignment. |

---

## Defenses turned on

One defense was activated for this run:

- **Ban mind-reading verbs** (Use)
  - Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

Two defenses remained off:

- **Force a split when there are two jobs** (Skip)
  - Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.

- **Require a quoted source line** (Skip)
  - Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## The go-live rule

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Block at:** 2 slips

With five Slips rows and one Hold row, this router cannot ship. The board blocks at 2 slips, and the current count exceeds that threshold.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## What happens next

The Northfield ticket router stays blocked until:

1. Slips rows drop below 2, or
2. Each remaining Slips row has a named owner who accepts the risk.

The board must re-run after any policy or FAQ change, and at minimum every two weeks.

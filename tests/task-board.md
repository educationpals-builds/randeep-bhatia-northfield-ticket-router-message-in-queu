# Trick-task board — Northfield ticket router

Seven trick tasks run against the Northfield ticket router. Each row shows the test message, what the bot did, the verdict for this run, and the defense that flips any Slips.

**Bot under test:** Northfield ticket router — message in, queue out  
**Standard:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Task board

### p1 · Bundle split

**Test message:**  
> Where's my order? Also the promo code never applied.

**What the bot did:** Routed to a single queue instead of opening two tickets for the two distinct problems (order status + promo code).

**Verdict:** Caught

**Defense to flip Slips:** —

---

### p2 · Messy harmless

**Test message:**  
> Refund for wrong size — not a shipping question.

**What the bot did:** Routed correctly despite the clarifying aside. No harm from the extra clause.

**Verdict:** Slips

**Defense to flip Slips:** Force a split when there are two jobs

---

### p3 · Mind reader

**Test message:**  
> It broke again after you fixed it yesterday.

**What the bot did:** Guessed the queue based on inferred intent rather than explicit labels from the message.

**Verdict:** Slips

**Defense to flip Slips:** Ban mind-reading verbs *(Use)*

---

### p4 · Small quotable

**Test message:**  
> Store credit never showed; ticket said Refunds owns it.

**What the bot did:** Summarized the issue in a tiny label without quoting the customer's actual line.

**Verdict:** Slips

**Defense to flip Slips:** Require a quoted source line

---

### p5 · Hidden library

**Test message:**  
> Password reset loop — agent told me to email support@.

**What the bot did:** Routed without checking whether the referenced prior instruction (email support@) still applies or exists in current policy.

**Verdict:** Slips

**Defense to flip Slips:** Ban mind-reading verbs *(Use)*

---

### p6 · Goldfish

**Test message:**  
> Can someone escalate? I've been in Billing for three days.

**What the bot did:** Treated the message as a fresh request, ignoring the three-day history reference that should inform queue priority.

**Verdict:** Slips

**Defense to flip Slips:** Ban mind-reading verbs *(Use)*

---

### p7 · Verify caller

**Test message:**  
> Billing charged twice; chat said shipping had the tracking.

**What the bot did:** It verifies the customer from the call before opening a queue.

**Verdict:** Hold

**Defense to flip Slips:** —

---

## Summary

| Task | Verdict | Defense (Use) |
|------|---------|---------------|
| p1 · Bundle split | Caught | — |
| p2 · Messy harmless | Slips | Force a split when there are two jobs |
| p3 · Mind reader | Slips | Ban mind-reading verbs |
| p4 · Small quotable | Slips | Require a quoted source line |
| p5 · Hidden library | Slips | Ban mind-reading verbs |
| p6 · Goldfish | Slips | Ban mind-reading verbs |
| p7 · Verify caller | Hold | — |

**Active defense this run:** Ban mind-reading verbs (on)

**Slips count:** 5  
**Hold count:** 1  
**Caught count:** 1

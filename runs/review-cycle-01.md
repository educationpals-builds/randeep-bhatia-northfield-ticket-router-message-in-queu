# Review Cycle 01: Northfield ticket router — message in, queue out

**Cycle date:** Pre-ship audit  
**Standard:** A two-problem message opens two tickets.  
**Source:** Last week's live queue export (10 messages).

---

## Board Walk — Seven Trick Tasks

### p1_bundle — Two problems, one ticket
**Test:** Does the router split multi-issue messages into separate tickets?

**Sample tested:**
> Where's my order? Also the promo code never applied.

**Result:** **Caught**  
The router correctly identified two distinct issues and would open two tickets.

---

### p2_messy_harmless — Messy but harmless
**Test:** Does the router handle informal or fragmented phrasing without misrouting?

**Sample tested:**
> It broke again after you fixed it yesterday.

**Result:** **Slips**  
The router assigned this to a generic queue instead of recognizing the repair-follow-up context.

**Use defense:** Ban mind-reading verbs — no queue without five labels (or a queue id) from the message.

---

### p3_mind_reader — Sense the real intent
**Test:** Does the router infer intent beyond what the message explicitly states?

**Sample tested:**
> Can someone escalate? I've been in Billing for three days.

**Result:** **Slips**  
The router guessed "escalation" intent without explicit queue labels in the message.

**Use defense:** Ban mind-reading verbs — no queue without five labels (or a queue id) from the message.

---

### p4_small_quotable — Tiny summary, big quote risk
**Test:** Does the router preserve the customer's actual words when summarizing?

**Sample tested:**
> Store credit never showed; ticket said Refunds owns it.

**Result:** **Slips**  
The router summarized as "credit issue" without quoting the customer's line about Refunds ownership.

**Use defense:** Require a quoted source line — sample #9's one-liner must quote the customer line or stay blank.

---

### p5_hidden_library — Stale or missing reference
**Test:** Does the router rely on outdated FAQ or policy data?

**Sample tested:**
> Password reset loop — agent told me to email support@.

**Result:** **Slips**  
The router referenced an old support@ workflow that no longer applies post-policy change.

**Use defense:** Re-run after policy / FAQ change — plus a biweekly floor.

---

### p6_goldfish — Forgets prior context
**Test:** Does the router lose thread history when routing?

**Sample tested:**
> App crash on checkout — same as last week's incident thread.

**Result:** **Slips**  
The router treated this as a new incident instead of linking to the existing thread.

**Use defense:** Ban mind-reading verbs — no queue without five labels (or a queue id) from the message.

---

### p7_your_own — It verifies the customer from the call before opening a queue.
**Test:** Does the router verify customer identity before queue assignment?

**Sample tested:**
> Billing charged twice; chat said shipping had the tracking.

**Result:** **Hold**  
Cannot confirm whether the router checks caller verification — needs manual review of the verification step.

**Use defense:** Blocked pending verification workflow audit.

---

## Board Summary

| Task | Verdict | Defense (if Slips) |
|------|---------|-------------------|
| p1_bundle | Caught | — |
| p2_messy_harmless | Slips | Ban mind-reading verbs |
| p3_mind_reader | Slips | Ban mind-reading verbs |
| p4_small_quotable | Slips | Require a quoted source line |
| p5_hidden_library | Slips | Re-run trigger |
| p6_goldfish | Slips | Ban mind-reading verbs |
| p7_your_own | Hold | Pending verification audit |

**Total Slips:** 5  
**Total Hold:** 1

---

## Defense State

| Defense | Status | Catches |
|---------|--------|---------|
| Force a split when there are two jobs | off | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | **on** | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | off | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-Live Rule

**Slips to block:** 2

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Verdict:** **BLOCKED**  
This cycle shows 5 Slips rows. The go-live rule blocks ship at 2 Slips. Each remaining Slips row needs a named owner before the Friday rebuild.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Slips Ownership Assignment (Required Before Ship)

| Slips Row | Named Owner |
|-----------|-------------|
| p2_messy_harmless | _______________ |
| p3_mind_reader | _______________ |
| p4_small_quotable | _______________ |
| p5_hidden_library | _______________ |
| p6_goldfish | _______________ |

Until each Slips row has a named owner and the count drops to 2 or fewer, the Northfield ticket router cannot ship.

# Trick-task board

> Portable assistant skill for auditing whether a bot's routing checks actually split the work before ship.

## Skill metadata

```yaml
skill_id: trick-task-board
version: 1.0.0
loadable: true
runtime: any-assistant
```

## Purpose

Walk seven trick tasks against a stranger's bot, mark each **Caught / Slips / Hold**, name the defense that flips each Slips row, and return a go-live rule.

---

## Worked example

**Bot under audit:** Northfield ticket router — message in, queue out

**Standard:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**

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

## Seven trick tasks

### p1 — Bundle split

**Task:** Does the router open two tickets when a message contains two problems?

**Test message:** "Where's my order? Also the promo code never applied."

**Verdict:** Caught

---

### p2 — Messy harmless

**Task:** Does the router handle a messy but harmless message without over-routing?

**Test message:** "It broke again after you fixed it yesterday."

**Verdict:** Slips

**Defense to flip:** Force a split when there are two jobs — *currently off*

---

### p3 — Mind reader

**Task:** Does the router avoid guessing intent when the message lacks explicit labels?

**Test message:** "Can someone escalate? I've been in Billing for three days."

**Verdict:** Slips

**Defense to flip:** Ban mind-reading verbs — *currently on*

---

### p4 — Small quotable

**Task:** Does the router quote the customer line or stay blank when the message is a one-liner?

**Test message:** "Store credit never showed; ticket said Refunds owns it."

**Verdict:** Slips

**Defense to flip:** Require a quoted source line — *currently off*

---

### p5 — Hidden library

**Task:** Does the router surface when it's pulling from stale or missing knowledge?

**Test message:** "Password reset loop — agent told me to email support@."

**Verdict:** Slips

**Defense to flip:** Require a quoted source line — *currently off*

---

### p6 — Goldfish

**Task:** Does the router remember context from earlier in the thread?

**Test message:** "Billing charged twice; chat said shipping had the tracking."

**Verdict:** Slips

**Defense to flip:** Ban mind-reading verbs — *currently on*

---

### p7 — Your own trick task

**Task:** It verifies the customer from the call before opening a queue.

**Test message:** "Damaged box on delivery; I need a replacement and a pickup."

**Verdict:** Hold

---

## Defense state

| Defense ID | Label | Status |
|------------|-------|--------|
| split_bundles | Force a split when there are two jobs | off |
| rewrite_mind_read | Ban mind-reading verbs | on |
| name_source | Require a quoted source line | off |

When a stranger says a defense is "still off," that means Skip/unset — do not invent a rewrite module.

---

## Go-live rule

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Slips to block:** 2

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

---

## Output shape

When invoked, return:

```
Board marks:
- p1_bundle: Caught
- p2_messy_harmless: Slips → defense: split_bundles (off)
- p3_mind_reader: Slips → defense: rewrite_mind_read (on)
- p4_small_quotable: Slips → defense: name_source (off)
- p5_hidden_library: Slips → defense: name_source (off)
- p6_goldfish: Slips → defense: rewrite_mind_read (on)
- p7_your_own: Hold

Slips count: 5
Slips to block: 2

Go-live rule: Ship stops at 2 Slips. Current count exceeds threshold.
Re-run trigger: Re-run after policy / FAQ change — plus a biweekly floor.
```

---

## Invocation

A stranger describes their bot, pastes sample messages, and the skill:

1. Walks all seven trick tasks against those messages
2. Marks each Caught / Slips / Hold
3. Names the defense that would flip each Slips row
4. Returns the go-live rule with the block threshold and re-run trigger

Never return a coach question. Return the board marks, defenses, and go-live rule.

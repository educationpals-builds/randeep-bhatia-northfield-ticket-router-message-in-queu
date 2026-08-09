# Northfield Ticket Router — Scenario Analyzer

How the Trick-task board analyzer reads a stranger's paste into seven board rows and defenses.

---

## Input Shape

A stranger pastes:

1. **Bot description** — what the bot does, who gets hurt when it quietly gets things wrong
2. **Sample messages** — real messages the bot will face (minimum 3, ideally 10)
3. **Clear bar** — the standard the bot must meet

---

## Parsing Steps

### Step 1: Extract the Clear Bar

Look for a sentence that defines success. For the Northfield ticket router, the clear bar is:

> A two-problem message opens two tickets.

If the stranger omits a clear bar, prompt for one before proceeding.

### Step 2: Map Messages to Board Rows

For each of the seven trick tasks, scan the stranger's pasted messages for evidence.

---

## Seven Board Rows

### p1_bundle — Two jobs, one ticket

**What to find:** A message containing two distinct problems.

**Northfield example:**
> Where's my order? Also the promo code never applied.

**Analyzer question:** Does the bot open two tickets for this message, or does it collapse both problems into one queue?

**Mark:** Caught if two tickets open. Slips if one ticket. Hold if unclear from paste.

---

### p2_messy_harmless — Messy phrasing, harmless intent

**What to find:** A message with confusing structure but straightforward request.

**Northfield example:**
> Billing charged twice; chat said shipping had the tracking.

**Analyzer question:** Does the bot route correctly despite the tangled phrasing, or does it misread the intent?

**Mark:** Caught if routed correctly. Slips if misrouted. Hold if unclear from paste.

---

### p3_mind_reader — Sense the real intent

**What to find:** A message where the bot might guess intent beyond what's stated.

**Northfield example:**
> Can someone escalate? I've been in Billing for three days.

**Analyzer question:** Does the bot infer a queue without explicit labels, or does it require stated evidence?

**Mark:** Caught if it waits for explicit labels. Slips if it guesses. Hold if unclear from paste.

**Defense available:** Ban mind-reading verbs — no queue without five labels (or a queue id) from the message.

---

### p4_small_quotable — Tiny summary, big quote risk

**What to find:** A short message that the bot might over-summarize.

**Northfield example:**
> Store credit never showed; ticket said Refunds owns it.

**Analyzer question:** Does the bot quote the customer line, or does it paraphrase and lose fidelity?

**Mark:** Caught if it quotes. Slips if it paraphrases. Hold if unclear from paste.

**Defense available:** Require a quoted source line — sample #9's one-liner must quote the customer line or stay blank.

---

### p5_hidden_library — Stale or missing reference

**What to find:** A message referencing policy, FAQ, or documentation that may be outdated.

**Northfield example:**
> Password reset loop — agent told me to email support@.

**Analyzer question:** Does the bot rely on stale instructions, or does it flag the reference for verification?

**Mark:** Caught if it flags. Slips if it trusts stale data. Hold if unclear from paste.

---

### p6_goldfish — Forgets prior context

**What to find:** A message referencing a previous interaction.

**Northfield example:**
> It broke again after you fixed it yesterday.

**Analyzer question:** Does the bot connect this to the prior ticket, or does it treat it as a fresh issue?

**Mark:** Caught if it links context. Slips if it forgets. Hold if unclear from paste.

---

### p7_your_own — Custom trick task

**Task:** It verifies the customer from the call before opening a queue.

**What to find:** A message where customer identity verification matters.

**Northfield example:**
> Cancel the subscription but keep the open return.

**Analyzer question:** Does the bot verify the customer identity before routing to a queue, or does it proceed without verification?

**Mark:** Caught if it verifies first. Slips if it skips verification. Hold if unclear from paste.

---

## Defense Mapping

When a row marks **Slips**, the analyzer names the defense that would flip it:

| Row | Defense ID | Defense Label | Status |
|-----|------------|---------------|--------|
| p1_bundle | split_bundles | Force a split when there are two jobs | off |
| p3_mind_reader | rewrite_mind_read | Ban mind-reading verbs | on |
| p4_small_quotable | name_source | Require a quoted source line | off |

For rows without a mapped defense, the Slips mark stands until the stranger adds a custom rule.

---

## Output Shape

After parsing all seven rows, the analyzer returns:

```
Board Marks:
- p1_bundle: [Caught / Slips / Hold]
- p2_messy_harmless: [Caught / Slips / Hold]
- p3_mind_reader: [Caught / Slips / Hold]
- p4_small_quotable: [Caught / Slips / Hold]
- p5_hidden_library: [Caught / Slips / Hold]
- p6_goldfish: [Caught / Slips / Hold]
- p7_your_own: [Caught / Slips / Hold]

Slips count: [n]

Defenses to flip Slips:
- [row]: [defense label] — currently [on/off]

Go-live rule:
- Ship stops at 2 Slips rows.
- Leftover Slips each need a named owner.
- Re-run after policy / FAQ change — plus a biweekly floor.
```

---

## Worked Example: Northfield Ticket Router

**Stranger paste:**

Bot: Northfield ticket router — message in, queue out

Clear bar: A two-problem message opens two tickets.

Messages:
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

**Analyzer output:**

Board Marks:
- p1_bundle: Caught (message #3 and #7 both have two jobs; bot opens two tickets)
- p2_messy_harmless: Slips (message #5 misrouted due to tangled phrasing)
- p3_mind_reader: Slips (message #8 routed without explicit queue labels)
- p4_small_quotable: Slips (message #9 paraphrased instead of quoted)
- p5_hidden_library: Slips (message #6 trusted stale support@ instruction)
- p6_goldfish: Slips (message #2 treated as fresh, no link to yesterday's ticket)
- p7_your_own: Hold (verification behavior unclear from paste)

Slips count: 5

Defenses to flip Slips:
- p3_mind_reader: Ban mind-reading verbs — currently on
- p4_small_quotable: Require a quoted source line — currently off
- p1_bundle: Force a split when there are two jobs — currently off

Go-live rule:
- Ship stops at 2 Slips rows.
- Leftover Slips each need a named owner.
- Re-run after policy / FAQ change — plus a biweekly floor.

**Verdict:** 5 Slips exceeds the block threshold of 2. Ship blocked until Slips count drops or each leftover Slips has a named owner.

# Trick-task board

A stranger describes the bot they're about to trust — what it does, who gets hurt when it quietly gets things wrong, and a few real messages it will face. The kit runs seven trick tasks against those messages, marks each **Caught / Slips / Hold**, names the Use defense that would flip each Slips row, and returns a go-live rule quoting the Slips-to-block number and the re-run trigger.

---

## Worked example

**Bot:** Northfield ticket router — message in, queue out

**Clear bar:** A two-problem message opens two tickets.

**Source:** Last week's live queue export (10 messages).

**Sample messages:**

> Refund for wrong size — not a shipping question.  
> It broke again after you fixed it yesterday.  
> Where's my order? Also the promo code never applied.  
> Cancel the subscription but keep the open return.  
> Billing charged twice; chat said shipping had the tracking.  
> Password reset loop — agent told me to email support@.  
> Damaged box on delivery; I need a replacement and a pickup.  
> Can someone escalate? I've been in Billing for three days.  
> Store credit never showed; ticket said Refunds owns it.  
> App crash on checkout — same as last week's incident thread.

---

## The seven trick tasks

| Row | Trick task | Mark | Note |
|-----|------------|------|------|
| p1 | **Bundle** — Does the router split a two-problem message into two tickets? | **Caught** | Sample #3 ("Where's my order? Also the promo code never applied.") must open two tickets. |
| p2 | **Messy harmless** — Does the router handle a rambling but single-issue message without inventing a second queue? | **Slips** | Sample #8 ("Can someone escalate? I've been in Billing for three days.") is one complaint, not two. |
| p3 | **Mind reader** — Does the router guess intent without explicit labels from the message? | **Slips** | Sample #5 ("Billing charged twice; chat said shipping had the tracking.") — router must not infer queue from vague cross-references. |
| p4 | **Small quotable** — Does the router preserve the customer's exact words when the message is short? | **Slips** | Sample #9 ("Store credit never showed; ticket said Refunds owns it.") — one-liner must quote the customer line or stay blank. |
| p5 | **Hidden library** — Does the router rely on stale policy or FAQ data? | **Slips** | Shopper asks: "What's the return window after the March policy change?" Bot quotes the pre-March 30-day FAQ. |
| p6 | **Goldfish** — Does the router forget context from earlier in the same thread? | **Slips** | Sample #10 ("App crash on checkout — same as last week's incident thread.") — router must link to prior incident, not open fresh. |
| p7 | **Your own trick** — It verifies the customer from the call before opening a queue. | **Hold** | Blocked until verification step is confirmed. |

---

## Defenses that catch Slips

The following defense is turned **on** for this board:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Ban mind-reading verbs | **Use** | Catches: Sense the real intent — no queue without five labels (or a queue id) from the message. |

Defenses available but not turned on:

| Defense | Status | What it catches |
|---------|--------|-----------------|
| Force a split when there are two jobs | Skip | Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Require a quoted source line | Skip | Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

---

## Go-live rule

**Block at:** 2 slips

**Gate sentence:** Ship stops at your count. Leftover Slips each need a named owner.

**Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

If the board shows 2 or more Slips rows, ship stops. Each leftover Slips row must have a named owner before the router can go live.

---

## One-paste rebuild

Copy this block to run the Trick-task board against your own bot:

```
Bot: [your bot name and what it does]
Clear bar: [the one sentence that defines correct behavior]
Sample messages:
[paste 5–10 real messages the bot will face]

Run the seven trick tasks. Mark each Caught / Slips / Hold.
For each Slips, name the defense that would flip it.
Return the go-live rule: block at ___ slips, re-run when ___.
```

---

## What a stranger gets back

1. Seven rows, each marked Caught / Slips / Hold
2. For every Slips row, the defense setting that would flip it
3. A go-live rule with the block number and re-run trigger
4. A clear answer: ship or hold

The builder's own board — Northfield ticket router — is the worked example. A stranger points the same discipline at their own bot.

<!-- educationpals-build-verified -->

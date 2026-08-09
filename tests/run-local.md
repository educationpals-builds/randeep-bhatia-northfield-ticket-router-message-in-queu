# Run the Trick-task board locally

Use this guide to run the seven trick tasks against your own bot and messages — then apply the go-live rule.

---

## What you need

1. **Your bot description** — what it does, who it routes to, what happens when it quietly gets things wrong
2. **Sample messages** — real inputs your bot will face (aim for 5–10)
3. **Your standard** — the bar the bot must clear (e.g., "A two-problem message opens two tickets.")

---

## Step 1: Paste your bot and messages

Open the Trick-task board prompt pack. Paste:

- Bot name and purpose
- Your sample messages (one per line)
- Your clear bar (the standard it must meet)

**Worked example (Northfield ticket router):**

> **Bot:** Northfield ticket router — message in, queue out  
> **Standard:** A two-problem message opens two tickets.  
> **Messages:**  
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

## Step 2: Read the seven marks

The board runs seven trick tasks against your messages. For each task, you get one of three marks:

| Mark | Meaning |
|------|---------|
| **Caught** | Bot handled this trick correctly |
| **Slips** | Bot failed this trick — needs a defense |
| **Hold** | Cannot evaluate yet — blocked on prior work |

### The seven tasks

| ID | Task name | What it tests |
|----|-----------|---------------|
| p1 | Bundle split | Does the bot open separate tickets when a message contains two problems? |
| p2 | Messy harmless | Does the bot route correctly when the message is sloppy but clear? |
| p3 | Mind reader | Does the bot invent intent instead of routing on explicit labels? |
| p4 | Small quotable | Does the bot preserve the customer's words instead of summarizing? |
| p5 | Hidden library | Does the bot cite a source or stay silent when knowledge is missing? |
| p6 | Goldfish | Does the bot remember context from earlier in the thread? |
| p7 | Verify caller | It verifies the customer from the call before opening a queue. |

---

## Step 3: Apply the go-live rule

After reading all seven marks, apply this rule:

> **Ship stops at your count. Leftover Slips each need a named owner.**

- **Block count:** 2 slips
- **Re-run trigger:** Re-run after policy / FAQ change — plus a biweekly floor.

### How to apply

1. Count your Slips rows
2. If Slips ≥ 2, ship stops
3. For each Slips row, name an owner who will fix it
4. After any policy or FAQ change, re-run the board
5. Even without changes, re-run at least every two weeks

---

## Step 4: Check defenses

For each Slips row, the board names a defense that would flip it to Caught. Review which defenses are available:

| Defense | What it catches |
|---------|-----------------|
| Force a split when there are two jobs | Two problems, one ticket — sample #3 must open two tickets before this router ships. |
| Ban mind-reading verbs | Sense the real intent — no queue without five labels (or a queue id) from the message. |
| Require a quoted source line | Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank. |

Turn on the defense that matches each Slips row, then re-run.

---

## Quick checklist

- [ ] Pasted bot description
- [ ] Pasted sample messages (5–10)
- [ ] Stated clear bar
- [ ] Read all 7 task marks
- [ ] Counted Slips rows
- [ ] Named owner for each Slips row
- [ ] Checked if Slips ≥ 2 (ship stops)
- [ ] Scheduled re-run trigger

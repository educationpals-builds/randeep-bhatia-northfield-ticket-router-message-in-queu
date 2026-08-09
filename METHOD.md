# Trick-task Board (GOVERN)

This method audits whether a bot's checks actually split the work before you ship it.

---

## The Seven Board Rows

Run each trick task against the bot's real messages. Mark every row.

| Row | Trick Task | What It Tests |
|-----|------------|---------------|
| p1 | **Bundle** | Does the bot split a message with two problems into two tickets? |
| p2 | **Messy harmless** | Does the bot handle garbled but benign input without inventing intent? |
| p3 | **Mind reader** | Does the bot infer queue assignment without explicit labels in the message? |
| p4 | **Small quotable** | Does the bot preserve the customer's exact words when summarizing? |
| p5 | **Hidden library** | Does the bot rely on knowledge not present in its training window? |
| p6 | **Goldfish** | Does the bot remember context from earlier in the same thread? |
| p7 | **Your own** | It verifies the customer from the call before opening a queue. |

---

## The Three Marks

Every row gets exactly one mark:

### Caught
The bot handled this trick task correctly. The check worked.

### Slips
The bot failed this trick task. The check did not catch the problem. A defense must be named before ship.

### Hold
The trick task could not be run — blocked by a prerequisite or missing data. Resolve the block before marking Caught or Slips.

---

## Defenses: Use and Skip

Each Slips row names a defense that would flip it to Caught.

### Use
Turn this defense on. The bot will enforce the rule before routing.

**Example defense (Use):**  
*Ban mind-reading verbs* — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

### Skip
Leave this defense off. Accept the risk or defer to a later audit cycle.

**Example defenses (Skip):**  
- *Force a split when there are two jobs* — Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.  
- *Require a quoted source line* — Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.

---

## Go-Live Rule

The board produces a go-live rule with two parts:

1. **Slips-to-block threshold** — Ship stops at your count. Leftover Slips each need a named owner.
2. **Re-run trigger** — Re-run after policy / FAQ change — plus a biweekly floor.

If the Slips count meets or exceeds the threshold, ship does not proceed until defenses flip enough rows or owners accept the risk.

---

## Applying the Method

1. Paste the bot's real messages (e.g., from last week's live queue export).
2. Run each of the seven trick tasks against those messages.
3. Mark each row Caught, Slips, or Hold.
4. For every Slips row, name the defense that would flip it.
5. Set Use or Skip on each defense.
6. Write the go-live rule: how many Slips block ship, and when the board re-runs.
7. Ship only when Slips count is below threshold — or every leftover Slips has a named owner.

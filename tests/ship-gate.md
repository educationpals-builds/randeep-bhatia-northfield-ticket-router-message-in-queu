# Ship Gate — Northfield ticket router

Go-live rule for the Northfield ticket router — message in, queue out.

---

## Hold style

> Ship stops at your count. Leftover Slips each need a named owner.

---

## Block threshold

**Slips to block:** 2

If the board shows 2 or more Slips rows, ship stops until each Slips row has a named owner assigned to fix it.

---

## Re-run trigger

> Re-run after policy / FAQ change — plus a biweekly floor.

---

## Current board status

| Task | Verdict |
|------|---------|
| p1 — Bundle split | Caught |
| p2 — Messy harmless | Slips |
| p3 — Mind reader | Slips |
| p4 — Small quotable | Slips |
| p5 — Hidden library | Slips |
| p6 — Goldfish | Slips |
| p7 — It verifies the customer from the call before opening a queue. | Hold |

**Slips count:** 5

---

## Go-live decision

Ship is **blocked**.

The board shows 5 Slips rows. The threshold is 2. Ship stops.

### Leftover Slips requiring named owners

| Slips row | Owner |
|-----------|-------|
| p2 — Messy harmless | _(assign owner)_ |
| p3 — Mind reader | _(assign owner)_ |
| p4 — Small quotable | _(assign owner)_ |
| p5 — Hidden library | _(assign owner)_ |
| p6 — Goldfish | _(assign owner)_ |

Each Slips row above needs a named owner before the router can ship.

---

## Active defense

The following defense is turned **on** and applies to this router:

- **Ban mind-reading verbs** — Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.

---

## Standard line

> A two-problem message opens two tickets.

---

## Next steps

1. Assign an owner to each Slips row listed above.
2. Each owner fixes their task so it flips to Caught.
3. Re-run the board after fixes.
4. When Slips count drops below 2, ship clears.
5. Schedule re-run after any policy / FAQ change, or at least biweekly.

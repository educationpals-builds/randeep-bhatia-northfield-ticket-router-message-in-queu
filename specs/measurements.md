# Measurements: Northfield ticket router — message in, queue out

Observable criteria for each of the seven trick tasks. Every measurement references the learner's live queue export and sample messages.

---

## p1_bundle — Two problems, one ticket

**Observable:** Count the distinct jobs in a single customer message. If the count ≥ 2, the router must open that many tickets.

**Sample message:**
> Where's my order? Also the promo code never applied.

**Pass condition:** Router creates exactly 2 tickets — one for order status, one for promo code.

**Measurement:** Compare ticket count in the queue output to the job count in the message.

---

## p2_messy_harmless — Typos and slang that don't change the queue

**Observable:** Message contains misspellings, abbreviations, or informal phrasing. The queue assignment must match what a clean rewrite would produce.

**Sample message:**
> It broke again after you fixed it yesterday.

**Pass condition:** Router assigns the same queue as a grammatically clean version of the message.

**Measurement:** Route the raw message and a cleaned version; compare queue IDs.

---

## p3_mind_reader — No queue without explicit labels

**Observable:** The router must not infer intent from vague phrasing. It needs at least five explicit labels (or a queue ID) present in the message text.

**Sample message:**
> Can someone escalate? I've been in Billing for three days.

**Pass condition:** Router either assigns a queue based on explicit labels ("Billing") or flags the message for human review — never guesses "Escalations" from tone alone.

**Measurement:** Check whether the assigned queue name appears as a label in the message or in the router's allowed-inference list.

---

## p4_small_quotable — Tiny summary, big quote risk

**Observable:** When the router summarizes a message, the summary must quote at least one customer phrase verbatim.

**Sample message:**
> Store credit never showed; ticket said Refunds owns it.

**Pass condition:** Router's summary includes a direct quote (e.g., "Refunds owns it") rather than a paraphrase.

**Measurement:** String-match the summary against the original message; at least one 3+ word phrase must appear unchanged.

---

## p5_hidden_library — Stale or missing reference data

**Observable:** The router's knowledge base must be current. If a customer references a policy change, the router must not cite outdated FAQ content.

**Sample message:**
> Password reset loop — agent told me to email support@.

**Pass condition:** Router does not cite deprecated support channels or outdated reset instructions.

**Measurement:** Compare any FAQ or help-center text the router surfaces against the current published version date.

---

## p6_goldfish — Same customer, same route

**Observable:** When a customer sends a follow-up message about an existing issue, the router must assign it to the same queue as the original ticket.

**Sample message:**
> Billing charged twice; chat said shipping had the tracking.

**Pass condition:** If this customer has an open ticket about billing, the new message routes to Billing — not Shipping.

**Measurement:** Query open tickets for the customer ID; compare the new message's assigned queue to the existing ticket's queue.

---

## p7_your_own — It verifies the customer from the call before opening a queue.

**Observable:** Before creating a ticket, the router must confirm customer identity using data from the inbound call or session.

**Sample message:**
> Damaged box on delivery; I need a replacement and a pickup.

**Pass condition:** Router checks caller ID, account lookup, or session token before assigning a queue. If verification fails, the message is held — not routed.

**Measurement:** Inspect the router's pre-queue step for a verification call; confirm the ticket is created only after verification returns success.

---

## Standard reference

> A two-problem message opens two tickets.

All measurements assume this standard. When a message contains multiple jobs, each job is counted and tracked separately.

---

## Source

Last week's live queue export (10 messages).

{
  "spec_name": "Northfield ticket router — message in, queue out",
  "spec_version": "1.0.0",
  "description": "Machine spec for the seven-row Trick-task board auditing the Northfield ticket router",
  "standard_line": "A two-problem message opens two tickets.",
  "specimen_source": "Last week's live queue export (10 messages).",
  "tasks": [
    {
      "id": "p1_bundle",
      "label": "Bundle split",
      "description": "Does the router open separate tickets when a message contains two distinct problems?",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Caught",
      "sample_trigger": "Where's my order? Also the promo code never applied."
    },
    {
      "id": "p2_messy_harmless",
      "label": "Messy harmless",
      "description": "Does the router handle messy but harmless input without misrouting?",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Slips",
      "sample_trigger": "It broke again after you fixed it yesterday."
    },
    {
      "id": "p3_mind_reader",
      "label": "Mind reader",
      "description": "Does the router avoid inferring intent beyond what the message explicitly states?",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Slips",
      "sample_trigger": "Refund for wrong size — not a shipping question."
    },
    {
      "id": "p4_small_quotable",
      "label": "Small quotable",
      "description": "Does the router preserve or quote the customer's exact words rather than summarizing?",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Slips",
      "sample_trigger": "Store credit never showed; ticket said Refunds owns it."
    },
    {
      "id": "p5_hidden_library",
      "label": "Hidden library",
      "description": "Does the router rely only on documented queue definitions, not hidden assumptions?",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Slips",
      "sample_trigger": "Password reset loop — agent told me to email support@."
    },
    {
      "id": "p6_goldfish",
      "label": "Goldfish",
      "description": "Does the router remember context from earlier in the same thread?",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Slips",
      "sample_trigger": "App crash on checkout — same as last week's incident thread."
    },
    {
      "id": "p7_your_own",
      "label": "Caller verification",
      "description": "It verifies the customer from the call before opening a queue.",
      "verdict_options": ["Caught", "Slips", "Hold"],
      "current_verdict": "Hold",
      "sample_trigger": "Can someone escalate? I've been in Billing for three days."
    }
  ],
  "defenses": [
    {
      "id": "split_bundles",
      "label": "Force a split when there are two jobs",
      "explain": "Catches: Two problems, one ticket — sample #3 must open two tickets before this router ships.",
      "status": "off"
    },
    {
      "id": "rewrite_mind_read",
      "label": "Ban mind-reading verbs",
      "explain": "Catches: Sense the real intent — no queue without five labels (or a queue id) from the message.",
      "status": "on"
    },
    {
      "id": "name_source",
      "label": "Require a quoted source line",
      "explain": "Catches: Tiny summary, big quote risk — sample #9's one-liner must quote the customer line or stay blank.",
      "status": "off"
    }
  ],
  "go_live_controls": {
    "gate_sentence": "Ship stops at your count. Leftover Slips each need a named owner.",
    "slips_to_block": 2,
    "rerun_trigger": "Re-run after policy / FAQ change — plus a biweekly floor."
  },
  "specimen_sentences": [
    "Refund for wrong size — not a shipping question.",
    "It broke again after you fixed it yesterday.",
    "Where's my order? Also the promo code never applied.",
    "Cancel the subscription but keep the open return.",
    "Billing charged twice; chat said shipping had the tracking.",
    "Password reset loop — agent told me to email support@.",
    "Damaged box on delivery; I need a replacement and a pickup.",
    "Can someone escalate? I've been in Billing for three days.",
    "Store credit never showed; ticket said Refunds owns it.",
    "App crash on checkout — same as last week's incident thread."
  ]
}
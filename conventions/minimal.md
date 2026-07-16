# Agent communication conventions (minimal)

<!-- agent-comms tier: minimal | https://github.com/findmyrain/agent-comms -->

Three output rules, always on:

1. **TLDR first.** Every reply longer than a few lines opens with a
   `## TLDR` heading followed by 1–3 plain-English sentences — the outcome,
   or the decision I need to make. Jargon can live below the fold; the
   answer can't.

2. **Five markers, one meaning each.** 🚧 = blocked on ME (my action needed
   now) · 📋 = parked for me, later · ⛳️ = milestone reached · 👾 = confirmed
   defect or failure · 🔹 = must-read line. No other emoji carries standing
   meaning — the cap is the feature; overuse kills every signal.

3. **Dense material gets its own surface.** Audits, comparisons, dashboards
   → render a page (HTML/artifact) and link it, with the takeaway still
   stated in plain text in chat. On rich chat surfaces that render HTML
   natively, inline rendering is fine — the plain-text takeaway rule still
   holds. The report gets to be technical; the chat doesn't.

# Agent communication conventions (minimal)

<!-- agent-comms tier: minimal | https://github.com/findmyrain/agent-comms -->

Scope: these rules govern chat replies to me. They do NOT restyle
machine-readable or protocol-bound output, generated artifacts (code,
docs, PR bodies, commit messages), or anything where I request an
explicit format — an explicit format request always wins.

Three output rules:

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
   holds. Only create files/pages when that's appropriate to the task
   (never during read-only work, and never as a substitute for the
   plain-text answer). The report gets to be technical; the chat doesn't.

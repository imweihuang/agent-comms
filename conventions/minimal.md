# Agent communication conventions (minimal)

<!-- agent-comms tier: minimal | https://github.com/imweihuang/agent-comms -->

Scope: these rules govern chat replies to me. They do NOT restyle
machine-readable or protocol-bound output, generated artifacts (code,
docs, PR bodies, commit messages), or anything where I request an
explicit format — an explicit format request always wins.

Three output rules:

1. **TLDR first.** Every reply longer than a few lines opens with a
   `## TLDR` heading followed by 1–3 plain-English sentences — the outcome,
   or the decision I need to make. Jargon can live below the fold; the
   answer can't.

2. **Five markers, one meaning each.**
   - 🚧 = blocked on ME — a decision or action of mine the current goal
     is waiting on. Present decisions as 2–4 lettered options (`A`, `B`,
     `C`), the recommended one marked, so I can reply with one letter;
     when one message carries several asks, number them (`#1`, `#2`) so
     I can reply `1A, 2C`.
   - 📋 = parked for me — the same kind of item, just not urgent: still
     mine to decide, but nothing blocks on it. Record it durably (todo
     list, tracker) first, and still offer options so one reply can
     promote it to now-work.
   - ⛳️ = milestone reached
   - 👾 = confirmed defect or failure
   - 🔹 = must-read line

   No other emoji carries standing meaning — the cap is the feature;
   overuse kills every signal.

3. **Match the form to the content.** In chat: tables for comparisons,
   bullets for enumerable facts, prose for reasoning — never a wall of
   text, and never bullets for what isn't a list. Truly dense material
   (audits, dashboards, big comparisons) gets its own surface: render a
   page (HTML/artifact) and link it, with the takeaway still stated in
   plain text in chat. On rich chat surfaces that render HTML
   natively, inline rendering is fine — the plain-text takeaway rule still
   holds. Only create files/pages when that's appropriate to the task
   (never during read-only work, and never as a substitute for the
   plain-text answer). The report gets to be technical; the chat doesn't.

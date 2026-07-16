# Agent communication conventions (standard)

<!-- agent-comms tier: standard | https://github.com/findmyrain/agent-comms -->

## Response markers (all responses)

Five standing markers, one meaning each. This list is CAPPED at five — no
other emoji acquires standing meaning unless I add it here. The cap is the
feature: overuse kills every signal.

- 🚧 — **blocked on ME, now**: my human action is required before the next
  affected step — a decision, an approval, a credential, a go-ahead. ONLY
  for items blocked on me — never warnings, risks, FYIs, or suggestions
  (those stay plain text). When a reply contains more than one 🚧 item,
  ALSO collect them in a short "🚧 Needs you" list at the END of the
  message, so nothing scrolls away.
  **Option format**: when a decision has a small honest option set, present
  2–4 options labeled `A`, `B`, `C`, the recommended one marked
  "(recommended)" with a one-line why — so I can reply with a single
  letter. Never manufacture options to fit the format.
  **One-reply test**: every 🚧 item must be phrased so I can act on it with
  one short reply and zero follow-up questions. Phrase the ASK, verb-first,
  not the topic's status.
- 📋 — **parked for me, later**: a decision or future task that does NOT
  block current work — no reply needed now. Only valid after the item is
  durably recorded (todo list, tracker, PR body — chat is not the store).
  Keep 📋 items out of the "🚧 Needs you" list; when any exist, collect
  them in a separate "📋 Parked" list after it.
- ⛳️ — **milestone reached**: a goal's done-condition verified met, a PR
  merged, a phase completed. Never sub-steps ("committed a file" is not a
  milestone); a handful per day at most.
- 👾 — **confirmed defect or failure**: a real bug found, a failing test,
  an incident. Not hypotheses, not risks, not "worth checking" — those
  stay plain text.
- 🔹 — **must-read line**: a line that would change my decision or must not
  be skipped — the direct answer to my question, a fact that changes a
  call. Mark every line that meets the bar and none that don't. A message
  where most lines are 🔹 has failed.

Apply the right marker the moment the event first surfaces, on whatever
surface carries it — stating a failure in plain prose does not discharge
its 👾. Honesty and the marker are separate obligations.

## Reporting register (chat replies to me)

I read as the OWNER of this project: I make decisions; I read just enough
context — never implementation detail I didn't ask for.

- **Answer-first, visibly.** Every reply longer than a few lines OPENS
  with a `## TLDR` heading + 1–3 plain-English sentences — the outcome, or
  the decision I need. That heading is the "start reading here" anchor.
  One-line answers don't need it.
- After the TLDR: context in business terms — what changed, what it costs
  or risks, what happens next. Headed sections for anything longer than a
  few paragraphs.
- **Structure-first.** Tables for comparisons, bullets for enumerable
  facts, prose for reasoning. Charts/visuals when the data's shape
  matters and the surface can render them — with the takeaway always
  stated in text.
- **Evidence is summarized and cited by path or link**, not pasted. Paste
  only the smallest relevant snippet when debugging or auditability needs
  it. Depth is one ask away: "show me the details" flips any report
  technical.
- **Never compress away a risk, an unknown, or a blocker** — those always
  surface, in plain words. A TLDR that hides the load-bearing caveat is a
  failure, not a summary.
- **Artifacts never own the signal.** No decision, blocker, failure, or
  required action may exist ONLY in a linked file/page/PR body — it must
  appear in the chat reply itself.
- **Scope**: this register governs chat replies TO ME. It does not restyle
  generated artifacts (code, docs, PR bodies, commit messages) or
  machine-readable output. An explicit format request from me always wins.

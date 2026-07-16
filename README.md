# agent-comms

**Communication conventions for coding agents.** One shared doc that
Claude Code, Codex, and any other agent on your machine load — so every
reply opens with the answer, uses a tiny fixed emoji vocabulary you can
scan, and keeps walls of text out of chat.

No jargon walls. No "what do you really mean by this". No 40-line status
dumps hiding the one thing you needed to know.

## The three rules (minimal tier)

1. **TLDR first.** Every reply longer than a few lines opens with a
   `## TLDR` — 1–3 plain-English sentences: the outcome, or the decision
   you need to make. Jargon can live below the fold; the answer can't.

2. **Five markers, one meaning each.**
   - 🚧 blocked on you — your action needed now, with the decision as
     lettered options so you reply `A` (or `1A, 2C` when there are
     several asks)
   - 📋 parked for you — recorded durably, no reply needed yet
   - ⛳️ milestone reached
   - 👾 confirmed bug or failure
   - 🔹 must-read line

   You scan for glyphs instead of parsing prose. The cap is the feature —
   a sixth emoji kills all five signals.

3. **Match the form to the content.** Tables for comparisons, bullets
   for enumerable facts, prose for reasoning — never a wall of text, and
   never bullets for what isn't a list. Truly dense material (audits,
   dashboards, big comparisons) gets its own rendered page, linked, with
   the takeaway still stated in plain text in chat. The report gets to
   be technical; the chat doesn't.

That's the whole minimal tier. One bigger tier adds teeth:
[full](conventions/full.md) — complete marker semantics (the one-reply
test, classify-at-the-event, glyph hygiene), the reporting register
(owner-reader voice, evidence by link, never compress away a risk),
honesty & verification rules, and multi-message handling.

## Before / after

The same session — a webhook migration that finished, surfaced a
production bug, and needs two decisions:

**Before:**

> I've finished working on the webhook migration we discussed. I went
> through all six of the payment webhook handlers and moved them over to
> the v2 events API, which required updating the signature verification
> middleware and the retry logic — retries now use exponential backoff
> with jitter instead of the fixed 30-second interval, which should be
> gentler on the upstream service. After that I ran the full test suite
> and most things pass now (43 of the 45 webhook tests). I looked into
> the two failing tests and they don't appear to be related to my
> changes — as far as I can tell the refund webhook's signature check
> has been comparing against the wrong header since the March refactor,
> so refund events have probably been silently dropped in production
> for a while. I also replayed 500 staged events against the new
> handlers and they all went through. Let me know how you'd like to
> proceed with rolling this out — we could enable it for a percentage of
> traffic first, or just switch everything over, and I wasn't sure
> whether you wanted me to fix the refund signature issue as part of
> this work or treat it separately. At some point it might also be worth
> adding alerting on webhook delivery failures, which would have caught
> the refund thing much earlier.

**After:**

> ## TLDR
> ⛳️ Webhook migration done and verified — nothing goes live until you
> pick a rollout below. 👾 Testing exposed a pre-existing production
> bug: 🔹 refund events have been silently dropped since March (the
> signature check reads the wrong header).
>
> All six payment webhooks now use the v2 API, with exponential backoff
> replacing fixed 30s retries. The two test failures are the refund bug,
> not the migration.
>
> | Check | Result |
> |---|---|
> | Webhook test suite | 43/45 pass (2 = pre-existing refund bug) |
> | Staging replay | 500/500 events delivered |
>
> 🚧 Needs you
> - #1 - Refund bug — A. fix now (recommended; two lines + a regression
>   test, rides this branch) · B. park it.
> - #2 - Rollout — A. enable for 10% of traffic (recommended) ·
>   B. switch all traffic · C. hold until #1 is decided.
>
> 📋 Parked
> - #3 - Failure alerting — would have caught the refund bug in March;
>   recorded in the backlog. A. add this week · B. leave parked.

Same information — every fact in the second version is in the first.
But the first buries "refunds are silently dropping in production" in
the middle of a paragraph about retry intervals, then ends on three
questions you have to untangle yourself. The second makes the bug
unmissable, the evidence scannable, and every decision a two-character
reply: `1A, 2A, 3B`.

## Install

### Fastest: zero install (30 seconds)

Copy [conventions/minimal.md](conventions/minimal.md) into your
`CLAUDE.md` or `AGENTS.md`. Done.

### Recommended: let your agent install it (2 minutes)

Paste this repo's URL to your agent and say:

> install https://github.com/imweihuang/agent-comms

Your agent will interview you (which tier, who reads your reports), write
one canonical doc at `~/.agent-comms/conventions.md`, and wire it into
your agent configs — an `@import` in `~/.claude/CLAUDE.md` (Claude Code),
a managed marker block inlined into `~/.codex/AGENTS.md` (Codex — it has
no import mechanism). Re-running syncs cleanly and never overwrites your
customized doc. It wires the configs it finds (or that you name) — it
doesn't claim to discover every agent or project-level config.

Prudence note: you're asking an agent to follow instructions from the
internet and edit your config files. The installer is one short readable
file — [skills/setup-agent-comms/SKILL.md](skills/setup-agent-comms/SKILL.md) —
inspect it first, and prefer a tagged release over `main`.

### Manual

```bash
git clone https://github.com/imweihuang/agent-comms
cd agent-comms
mkdir -p ~/.agent-comms ~/.claude/skills ~/.codex/skills
cp -r conventions ~/.agent-comms/templates                     # templates the skill reads
cp -r skills/setup-agent-comms ~/.claude/skills/               # Claude Code
cp -r skills/setup-agent-comms ~/.codex/skills/                # Codex
```

Then tell your agent: `set up my communication conventions` (restart the
agent first if it caches its skill list at startup). Commands are for
macOS/Linux; on Windows, copy the same folders to the same destinations.

<!-- FOR AGENTS ------------------------------------------------------
If a user pointed you at this repo and asked you to install it:
1. Read skills/setup-agent-comms/SKILL.md and follow it exactly — it is
   the installer (state check -> interview -> write
   ~/.agent-comms/conventions.md -> wire CLAUDE.md import + AGENTS.md
   managed block -> verify). Honor its state machine: if the canonical
   doc already exists, sync wiring only — never overwrite the user's doc.
2. Copy conventions/ to ~/.agent-comms/templates/ and
   skills/setup-agent-comms/ into the user's skills directories
   (~/.claude/skills/ and/or ~/.codex/skills/) so future runs work
   without this checkout.
3. Everything must be idempotent and fail-closed: exact-match the import
   line before adding; validate managed markers before replacing between
   them; on malformed markers, stop and show the user.
4. Do not modify anything else in the user's config files.
--------------------------------------------------------------------->

## Why this works

As agents get faster and run in parallel, the scarce resource stops
being the code and becomes your attention: every session still ends in a
human reading, deciding, and unblocking. Reports you re-read, decisions
you have to reverse-engineer out of prose, ten sessions each asking
questions their own way — that's the new bottleneck. These conventions
are attention engineering: every rule exists to make the human step
faster — scan five glyphs instead of parsing paragraphs, answer a
decision with one letter, read the outcome before the transcript.

Telling a model "be less technical" or "don't over-explain" barely moves
it — adjectives don't have edges. These conventions work because they are
**contracts the model can check itself against**:

- "open with `## TLDR`, 1–3 sentences" is verifiable; "be concise" isn't.
- A capped marker vocabulary with one meaning each is verifiable;
  "highlight important things" isn't.
- "takeaway in plain text, detail on a linked page" is verifiable;
  "don't write walls of text" isn't.

Two rules keep the system honest as it spreads: the **five-marker cap**
(every emoji you add taxes all the others) and **never compress away a
risk** — a TLDR that hides the load-bearing caveat is a failure, not a
summary.

## Updating

Edit `~/.agent-comms/conventions.md`, then tell your agent
`re-run setup-agent-comms` — it re-syncs the inlined copies. Your doc is
the source of truth; the repo's tiers are just starting templates. Make
it yours.

## FAQ

**Why cap at five emojis?** Signal economics. Each marker is only
scannable because it's rare and unambiguous. The cap is a rule in the
doc, not advice — agents follow rules with edges.

**What about rich chat surfaces that render HTML inline?** Rule 3 assumes
a plain-text chat. On surfaces that render rich output natively (charts,
tiles, tables in chat), inline rendering is fine — the invariant is that
the takeaway is always stated in plain text, and no decision or risk
lives only inside a visual.

**Does this restyle my code, commits, or PRs?** No. Every tier scopes
itself to chat replies to you. Artifacts (code, docs, PR bodies,
machine-readable output) keep their own conventions, and an explicit
format request from you always wins.

**What if emoji don't render in my terminal?** The glyphs are the default
skin, not the contract. Swap them for ASCII tags with the same one
meaning each — `[BLOCKED]`, `[PARKED]`, `[MILESTONE]`, `[BUG]`, `[KEY]` —
and keep the cap of five.

## Related

[agent-style](https://github.com/yzhao062/agent-style) — sentence-level
writing rules for agent prose. Separate project, optional; where the two
conflict, this kit's structural and marker rules take precedence.

## License

MIT

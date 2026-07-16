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
   🚧 blocked on you · 📋 parked for later · ⛳️ milestone · 👾 confirmed
   bug · 🔹 must-read line.
   You scan for glyphs instead of parsing prose. The cap is the feature —
   a sixth emoji kills all five signals.

3. **Dense material gets its own surface.** Audits, comparisons,
   dashboards → a rendered page, linked, with the takeaway still stated
   in plain text in chat. The report gets to be technical; the chat
   doesn't.

That's the whole minimal tier. Two bigger tiers add teeth:
[standard](conventions/standard.md) (full marker semantics — the
one-reply test, lettered A/B/C decision asks, the reporting register) and
[full](conventions/full.md) (honesty & verification rules, multi-message
handling).

## Install

### Fastest: zero install (30 seconds)

Copy [conventions/minimal.md](conventions/minimal.md) into your
`CLAUDE.md` or `AGENTS.md`. Done.

### Recommended: let your agent install it (2 minutes)

Paste this repo's URL to your agent and say:

> install https://github.com/findmyrain/agent-comms

Your agent will interview you (which tier, who reads your reports), write
one canonical doc at `~/.agent-comms/conventions.md`, and wire it into
every agent config on your machine — imported by `CLAUDE.md`, inlined
into `AGENTS.md` (which has no import mechanism) between managed markers
so re-running updates cleanly.

### Manual

```bash
git clone https://github.com/findmyrain/agent-comms
cp -r agent-comms/skills/setup-agent-comms ~/.claude/skills/   # Claude Code
cp -r agent-comms/skills/setup-agent-comms ~/.codex/skills/    # Codex
```

Then tell your agent: `set up my communication conventions`.

<!-- FOR AGENTS ------------------------------------------------------
If a user pointed you at this repo and asked you to install it:
1. Read skills/setup-agent-comms/SKILL.md and follow it exactly — it is
   the installer (interview -> write ~/.agent-comms/conventions.md ->
   wire CLAUDE.md import + AGENTS.md managed block -> verify).
2. Also copy skills/setup-agent-comms/ into the user's skills
   directories (~/.claude/skills/ and/or ~/.codex/skills/) so future
   updates can be run by name.
3. Everything must be idempotent: search before adding an import line;
   replace between managed markers, never append a second copy.
4. Do not modify anything else in the user's config files.
--------------------------------------------------------------------->

## Why this works

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

**Does this restyle my code, commits, or PRs?** No. The register governs
chat replies to you. Artifacts (code, docs, PR bodies, machine-readable
output) keep their own conventions.

## License

MIT

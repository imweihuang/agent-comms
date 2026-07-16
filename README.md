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
   - 🚧 blocked on you — your action needed now
   - 📋 parked for you — recorded, no reply needed yet
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

That's the whole minimal tier. Two bigger tiers add teeth:
[standard](conventions/standard.md) (marker semantics — the one-reply
test, lettered A/B/C and numbered multi-ask decision formats — plus the
reporting register) and [full](conventions/full.md) (standard + honesty
& verification rules + multi-message handling + deeper marker
discipline: classify-at-the-event, parked-item options, glyph hygiene).

## Before / after

The same event — a refactor finished, and testing surfaced a bug:

**Before:**

> I've completed the refactoring of the authentication module as
> requested. I began by analyzing the existing token validation flow in
> src/auth/validate.ts, which was using the deprecated jwt-simple
> library, and replaced it with jose, updating all call sites
> accordingly. I also consolidated the middleware chain… *(nine more
> lines)* …Note that while testing I noticed session expiry appears to
> use local time rather than UTC, which could be worth investigating at
> some point.

**After:**

> ## TLDR
> Auth refactor done and verified — 47/47 tests pass. 👾 Testing
> surfaced a pre-existing bug: session expiry uses local time, not UTC —
> users west of UTC stay logged in hours too long.
>
> 🚧 Needs you
> - Expiry bug — A. fix now (recommended; one line + a test) · B. park it.

Same information. One version buries a session-security bug in line
twelve of a paragraph about libraries; the other makes it impossible to
miss and one keystroke to act on.

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

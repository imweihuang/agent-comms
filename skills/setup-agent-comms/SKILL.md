---
name: setup-agent-comms
description: Set up agent communication conventions — a shared canon doc that both CLAUDE.md and AGENTS.md load, so every reply opens with a TLDR, uses a fixed five-emoji marker vocabulary, and keeps dense material out of chat. Use when the user says "set up my communication conventions", "install agent-comms", "make your replies open with a TLDR", or similar.
---

# Setup: agent communication conventions

You are installing a communication-conventions canon for the user. The
result: one canonical doc, loaded by every wired coding agent on this
machine (Claude Code via CLAUDE.md import; Codex via AGENTS.md), so those
agents report the same way.

## Step 0 — Locate templates and determine state

**Templates** live at, in order of preference:
1. `~/.agent-comms/templates/<tier>.md` (placed there by install), or
2. `conventions/<tier>.md` in a checkout of this repo, if you are running
   from one.

If neither exists, ask the user where they cloned the repo — do not
invent template content from memory.

**State** — check for `~/.agent-comms/conventions.md`:

- **A. Missing → fresh install.** Do the interview (Step 1), seed the doc
  from the chosen template (Step 2), wire agents (Step 3).
- **B. Exists → sync run.** Do NOT modify the canonical doc — it is the
  user's source of truth and may contain their customizations. Skip the
  interview and go straight to Step 3, regenerating wiring from the
  CURRENT canonical doc.
- **C. User explicitly asked to change tier or reset.** Show a diff
  between the chosen template and the current canonical doc, get explicit
  confirmation, then overwrite and re-wire. Never enter this state
  implicitly.

## Step 1 — Interview (fresh install only; 3 questions, offer defaults)

1. **Tier** — `minimal` (3 rules incl. lettered decision options;
   recommended first) · `full` (complete marker semantics, reporting
   register, honesty/verification, multi-message handling).
2. **Reader** — who reads the reports? (default: "the project owner —
   decisions, not implementation detail". A highly technical reader may
   want evidence pasted rather than linked; tune the register wording
   accordingly. The minimal tier has no register section — skip tuning
   for it.)
3. **Which agents** — Claude Code, Codex, or both? (default: every
   global config found: `~/.claude/CLAUDE.md`, `~/.codex/AGENTS.md`.)

If the user already answered any of these in their request, don't re-ask.

## Step 2 — Seed the canonical doc (fresh install or explicit reset only)

Copy the chosen tier template to `~/.agent-comms/conventions.md`
(create the directory if needed), applying any reader tuning as light
edits to the register section — do not restructure the doc.

## Step 3 — Wire each agent (idempotent, fail-closed)

**Claude Code** (`~/.claude/CLAUDE.md`):

- Check for the EXACT import line (exact-match this full line — do not
  match on the substring "agent-comms", which also appears in pasted
  template copies):

  ```
  @~/.agent-comms/conventions.md
  ```

- Absent → append it at the end of the file. Present → leave it alone.
- **Migration check**: if the file contains an `agent-comms tier:`
  marker comment, the user previously pasted a template directly
  (zero-install path). Show them the pasted block and offer to remove it
  in favor of the import — a stale pasted copy alongside the import means
  two drifting versions.

**Codex** (`~/.codex/AGENTS.md`) — no import mechanism, so inline the
canonical doc's content between managed markers:

```
<!-- agent-comms:begin (managed - edit ~/.agent-comms/conventions.md and re-run setup-agent-comms) -->
...content of ~/.agent-comms/conventions.md...
<!-- agent-comms:end -->
```

Fail-closed rules — count the markers BEFORE writing:
- Exactly 0 `begin` and 0 `end` → append a new block at the end.
- Exactly 1 `begin` and 1 `end`, in that order → replace the content
  between them with the current canonical doc.
- ANYTHING else (duplicates, reversed, orphaned, nested) → STOP. Show
  the user what you found and let them repair it. Never guess, never
  write into a malformed file.
- Run the same migration check for a pasted `agent-comms tier:` block.

**Other agents**: only if the user names one, apply the same pattern —
import where supported, managed block where not. Do not claim to have
covered agents you didn't wire.

Never remove or reorder the user's existing config content. After each
edit, re-read the file and confirm the result is exactly one import line
/ one well-formed managed block.

## Step 4 — Verify and demonstrate

1. Confirm each wired file: correct path, no duplicates, no pasted-copy
   leftovers.
2. Show the user a short sample reply formatted under THEIR tier — a
   `## TLDR`, one or two markers used correctly, and a lettered A/B/C
   decision ask (both tiers define the option format).
3. Tell them the update path: edit `~/.agent-comms/conventions.md`, then
   re-run this skill — a sync run re-inlines AGENTS.md and never touches
   their doc. Suggest committing config files that are under version
   control.

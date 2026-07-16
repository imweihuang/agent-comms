---
name: setup-agent-comms
description: Set up agent communication conventions — a shared canon doc that both CLAUDE.md and AGENTS.md load, so every reply opens with a TLDR, uses a fixed five-emoji marker vocabulary, and keeps dense material out of chat. Use when the user says "set up my communication conventions", "install agent-comms", "make your replies open with a TLDR", or similar.
---

# Setup: agent communication conventions

You are installing a communication-conventions canon for the user. The
result: one canonical doc, loaded by every coding agent on this machine
(Claude Code via CLAUDE.md import; Codex via AGENTS.md), so all agents
report the same way.

## Step 1 — Interview (keep it to 3 questions, offer defaults)

Ask, with a recommended default for each:

1. **Tier** — which conventions set?
   - `minimal` — 3 rules (~20 lines): TLDR-first, five markers, dense
     material off-chat. (recommended for first-time setup)
   - `standard` — full marker semantics + the reporting register (~90
     lines).
   - `full` — standard + honesty/verification rules + multi-message
     stretch handling (~150 lines).
2. **Reader** — who reads the reports? (default: "the project owner,
   non-implementation detail" — used to tune the register's voice; a
   highly technical reader may want evidence pasted rather than linked)
3. **Which agents** — Claude Code, Codex, or both? (default: every agent
   config found on the machine)

If the user already answered any of these in their request, don't re-ask.

## Step 2 — Create the shared canon doc

Copy the chosen tier's template from this skill's repo
(`conventions/<tier>.md`) to the canonical path:

```
~/.agent-comms/conventions.md
```

Create the directory if needed. If the file ALREADY exists, this is an
update: show the user a diff of what would change and confirm before
overwriting. Apply any reader-tuning from Step 1 by lightly editing the
register section (do not restructure the doc).

## Step 3 — Wire each agent

**Claude Code** (`~/.claude/CLAUDE.md`): add this import line if not
already present (search for `agent-comms` before adding — never duplicate):

```
@~/.agent-comms/conventions.md
```

**Codex** (`~/.codex/AGENTS.md`): AGENTS.md has no import mechanism, so
inline the doc between managed markers. If the markers already exist,
replace the content between them; never append a second copy:

```
<!-- agent-comms:begin (managed - do not edit between markers; edit ~/.agent-comms/conventions.md and re-run setup-agent-comms) -->
...doc content...
<!-- agent-comms:end -->
```

**Other agents**: if the user names another agent with a global config
file, apply the same pattern — import where supported, managed inline
block where not.

## Step 4 — Verify and demonstrate

1. Re-read each wired file and confirm: exactly one import line / one
   managed block, correct path, no duplicates.
2. Show the user a short sample reply formatted under the new
   conventions (a `## TLDR` + one 🚧 example with lettered options), so
   they see the effect immediately.
3. Tell them the one-line update path: edit
   `~/.agent-comms/conventions.md`, then re-run this skill to re-sync the
   inlined copies.

## Rules

- Idempotent, always: running this twice must produce the same result as
  running it once. Search before adding; replace between markers, never
  append.
- Never remove or reorder the user's existing CLAUDE.md / AGENTS.md
  content. Add the import at the end of the file unless the user says
  otherwise.
- If a wired file is under version control, tell the user what changed so
  they can commit it.

# 07 — Mixing antipatterns

How people break multi-harness setups — and the rules that prevent it.

## 1. Two repo agents editing the same worktree
Claude Code and OpenCode (or two Claude Code sessions) pointed at the same directory, both "helping." Result: conflicting edits, overwritten files, permission-prompt collisions, and agents fighting over git state.
**Rule:** one agent per worktree. Use separate clones/worktrees for parallel agents, or serialize (finish → verify → merge → next).

## 2. Nested orchestrators (recursion without a base case)
Hermes delegates to Claude Code, which is configured to shell out to OpenCode, which spawns another agent... Each layer multiplies context, cost, and failure modes; nobody owns the final artifact.
**Rule:** exactly one orchestrator. Repo agents never spawn other repo agents. If a task needs a sub-agent, the orchestrator owns the fan-out.

## 3. Duplicated instruction files (CLAUDE.md vs AGENTS.md vs both, contradictory)
Claude Code reads CLAUDE.md; OpenCode and others read AGENTS.md. Teams that maintain both end up with drift — agents follow different rules depending on which harness they run under. 
**Rule:** pick **one** source of truth (AGENTS.md is the cross-tool convention; point CLAUDE.md at it if both exist: `@AGENTS.md`). Keep instructions in one place.

## 4. Context bleed between harnesses
Pasting the same giant conversation into every tool; or expecting Hermes's memory to automatically appear in Claude Code. Each harness has its own context model — information doesn't flow unless you build the bridge.
**Rule:** the orchestrator's memory is the *only* long-term store; repo agents get a *task brief*, not a brain dump. Write the brief (goal, constraints, files, definition of done) — it's cheaper and more reliable than hoping context transfers.

## 5. Permission fatigue / unattended deadlock
A cron job fires Claude Code headless but hits an interactive permission prompt → hangs silently until the user notices. Or you set `--dangerously-skip-permissions` everywhere to avoid prompts → the agent now has full run of your machine.
**Rule:** per-automation allow-lists (permitted tools + paths), never blanket skip. Test the unattended path once interactively, then lock it down.

## 6. Cost blindness across tools
Each harness meters separately (or not at all) → you can't answer "what did this task actually cost?" One-off models, subscription caps, credits, and BYO keys all bill differently.
**Rule:** route cost telemetry through the orchestrator's single ledger (per-call rows: model, tokens, cost). If a tool doesn't report usage, wrap it and record it.

## 7. The "everything agent" trap
Buying the pitch that one harness does it all → a repo agent asked to manage your calendar, or a gateway agent asked to do surgical multi-file refactors. Each excels in its lane and is mediocre outside it.
**Rule:** match the harness to the job class (see [05](05-comparison.md)). When in doubt: coding → repo agent; life/ops/autonomy → gateway agent; cheap/free experimentation → open agent.

## 8. Ignoring the shared skills ecosystem
Skills written for Claude Code silently ignored by OpenCode (or vice versa) because the folder conventions differ — or duplicated skills drifting apart.
**Rule:** use the interoperable Agent Skills format where possible; one skill library, loaded by whichever harness runs the task.

## The one-sentence philosophy
> **One brain, many hands, single ledger** — one orchestrator decides and remembers, dedicated agents execute in their lane, and every action is metered in one place. Anything that violates that (two brains, two editors, two memory stores) is an antipattern.

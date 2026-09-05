# 05 — Comparison & competition

Where the four fight, and who wins each fight.

## The matrix

| Dimension | Hermes | Claude Code | OpenCode | Antigravity CLI |
|---|---|---|---|---|
| Loop | Chat-turn agent + subagents | Session agent + subagents | Session agent + agents-config | Session agent + subagents + teams |
| Anchor | Machine/home, cross-session | Repository | Repository | Repository + Google workspace |
| Memory | 3-layer: db + curated memory + skills | Repo (CLAUDE.md) + resume | Repo (AGENTS.md) + resume | Platform state + sync |
| Model freedom | Any provider | Claude family | Any provider/local | Gemini family |
| Scheduling | **Native cron** | Via shell/hooks | Via shell | Via shell/platform |
| Chat platforms | **Native gateways** | No | No | No (workspace-adjacent) |
| Hooks/events | Plugin/tool events | **Mature hook lifecycle** | Limited | Hooks (SDK + CLI) |
| Cost control | Built-in metering | Subscription or API | BYO keys | AI credits |
| Deepest extension | Skills + plugins + cron + delegation | Hooks + MCP + skills | Agents-as-config + LSP | SDK + personas + browser + teamwork |
| Headless/CI | Yes | Yes (`-p`) | Yes (`run`) | Yes |
| Free-tier economics | BYO (cheap models OK) | Free with Pro sub (fair-use) | BYO (cheap/local OK) | Credits (Gemini only) |

## When they are competitive (head-to-head fights)

1. **Claude Code vs OpenCode vs Antigravity CLI — "in-repo coding work."** If your task is *"implement this feature in this repo"*, all three will do it. The fight is decided by: model quality (Claude Code wins if you want Claude; Antigravity if Gemini; OpenCode if you want to pick per task), extension needs (hooks → Claude Code; agents-as-config → OpenCode; platform/SDK/browser → Antigravity), and economics (subscription → Claude Code; cheap/local models → OpenCode; credits → Antigravity).

2. **Hermes vs the coding trio — only when the task is "an agent that lives in your chat and does repo work on command."** Hermes can drive repos (terminal + file tools), but it's not repo-ergonomic. If your whole workflow is single-repo coding at a desk, a coding CLI beats Hermes at its own job. Hermes wins when the *interface* is chat, the work is cross-project, or autonomy (cron) matters.

3. **OpenCode vs Claude Code — the closest civil war.** Same shape, same skills ecosystem, same repo conventions. The real differentiators: model freedom/cost arbitrage (OpenCode) vs product polish + Claude subscription + hooks maturity (Claude Code). Switching between them is cheap by design — that's why they're friendly competitors, not hostile ones.

4. **Antigravity CLI vs both coders.** Google's pitch: same agent, all surfaces, plus browser + SDK + teamwork. It competes hardest on *platform reach*; it loses on model freedom (Gemini-only) and, today, on ecosystem age.

## When they are NOT competitive (different jobs)

- **"Be my assistant across email, calendar, media, and research, reachable from my phone, working on a schedule"** → only Hermes plays (gateways + cron + memory).
- **"Edit this repo with the best Claude reasoning and let me hook into every step"** → Claude Code, no contest.
- **"Run the same agent on whatever model is cheapest today, including free previews and local"** → OpenCode.
- **"One agent harness across my terminal, IDE, desktop, and a browser recorder, managed by Google"** → Antigravity.

## Decision guide — two lenses, opposite rules

The naive question — *"which model do I want, then which harness runs it?"* — is 2024 thinking. Models have decoupled from harnesses: OpenCode runs Claude and Gemini by key; vendor harnesses (Claude Code ↔ Anthropic, Antigravity ↔ Google) are *products*, not model transports. The real decision variables differ by context — and the individual and the enterprise follow **opposite rules**.

### Lens 1 — Individual practitioner (cost → control → coupling → model dial)

```mermaid
flowchart TD
    A["Pick a coding harness"] --> B{"Cost structure?"}
    B -->|"already pay a subscription<br/>(Claude Pro / credits)"| P["Vendor product wins<br/>(marginal cost ≈ $0)"]
    B -->|"BYO tokens / cheap + local"| O["Open harness wins<br/>(OpenCode class)"]
    P --> C{"Control needs?"}
    O --> C
    C -->|"hooks, gate every step"| H["prefer hook-rich product"]
    C -->|"agents-as-config, LSP"| AG["prefer open tool"]
    C -->|"enterprise policy"| E["prefer platform tier"]
    H & AG & E --> D["Coupling tolerance?"]
    D -->|"accept vendor polish"| V["product (CC / Antigravity)"]
    D -->|"keep freedom"| F["open (OpenCode)"]
    V --> M["Model = per-task dial<br/>inside the harness"]
    F --> M
```

**Rules of this lens:** economics and control pick the *platform*; the model is a per-task dial *within* it (swap cheap/frontier/local per task); the harness is a platform decision you make once per repo — the orchestrator routes tasks, it never re-litigates the harness.

### Lens 2 — Enterprise (governance → procurement → platform → catalog → ergonomics last)

```mermaid
flowchart TD
    A["Adopt an agent harness"] --> B{"Data classification"}
    B -->|"public code / IP"| C1["standard contracts"]
    B -->|"PII / PHI / PCI / regulated"| C2["restricted vendor contracts<br/>+ residency + DLP"]
    C1 & C2 --> D{"Regulatory checklist"}
    D -->|"SSO · audit · HITL ·<br/>retention · model inventory"| OK["pass = eligible"]
    OK --> E{"Platform standardization"}
    E -->|"existing cloud / agent platform"| F["harness rides the platform<br/>(Vertex · Azure · Bedrock +<br/>sanctioned agent tooling)"]
    F --> G["Approved model catalog<br/>(2–5 models, risk-tiered)"]
    G --> H["Dev ergonomics —<br/>last, within the approved set"]
```

**Rules of this lens:** governance and procurement decide *everything — the model included*. Freedom is shadow-AI risk; vendor coupling is accountability (SLA, support, indemnification); the model set is a governed catalog routed through a central gateway, not a per-developer dial; decisions are made once at the architecture review board and change through change management.

### Why the model diamond is obsolete (and when it still holds)
- **Obsolete:** "want Claude → Claude Code" assumes the harness is the model's only transport. It isn't — any OpenAI-compatible harness runs any model by key.
- **Still holds when:** you're spending a *subscription* (OAuth works only inside the vendor product — marginal cost ≈ $0 makes it the rational pick), or you need the vendor's *best integration* (Anthropic hooks + Opus; Google browser recorder + Gemini), or you're in an enterprise where the *catalog* (not the developer) decides the model anyway.

**The uncomfortable truth:** the individual pattern (try anything, dial per task) is precisely the behavior enterprise governance exists to contain. Two lenses, opposite rules — both correct in their domain.

**The honest summary:** the three coders compete in a triangle on the same turf; Hermes isn't in that turf at all — it's the orchestrator layer *above* it. Treating Hermes as "another coding agent" (or expecting a coding agent to be your life assistant) is the category error this repo exists to prevent.

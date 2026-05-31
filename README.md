# Food Chain

<p align="center">
  <img src="banner.png" alt="Food Chain — Adversarial Stress Test" width="100%" />
</p>

**Adversarial stress-testing suite for Claude.** Four skills. One install.
Puts your product ideas, architecture decisions, and execution plans through
live elimination battles with animal agents. No dependencies. Works everywhere.

```
npx skills add CodedRichy/food-chain-ideation
```

---

## The Suite

| Skill | What it does | Trigger |
|---|---|---|
| **food-chain-ideation** | Stress-test product ideas against a 32-animal behavioral DNA library | "food chain", "pressure test", "what kills this" |
| **food-chain-code** | Stress-test architecture decisions before writing code | "food chain code", "stress test this architecture" |
| **apex-to-action** | Turn battle output into a 90-day execution plan | "apex to action", "what do I build first" |
| **food-chain-monitor** | Re-test after pivots, feature changes, or market shifts | "food chain monitor", "re-battle this" |

One install gives you the complete adversarial product thinking workflow:
**Stress-test → Execute → Monitor → Re-test.**

---

## How It Works

The God Agent reads your idea and selects adversarial agents from a behavioral
DNA library — each matched to the specific threat vectors in your problem.
A Crow is assigned because it finds hidden structural flaws. A Tapeworm because
your product is built on a platform that can terminate the relationship.
A Tortoise because the person who needs to adopt this has used a spreadsheet
for nine years and is not in pain.

Each agent attacks under strict role-lock — zero awareness of the other attacks.
In environments that support it (Claude Code), each animal is spawned as an
independent subagent with architecturally enforced isolation. In single-context
environments (Claude.ai, Cursor), role-lock is instruction-enforced with
automatic fallback.

The weakest argument is eliminated. The survivor absorbs its sharpest insight
and evolves. The idea patches itself. The next round's agents attack the
patched version.

One apex predator stands. That is the thing worth building.

---

## v2.0 — What's New

- **Subagent mode** — true architectural isolation between attackers when Agent tool
  is available. Each animal is a separate agent with zero shared context.
- **Blind scoring** — attacks scored by an independent agent with no knowledge of
  which animal produced which attack. Removes God Agent self-assessment bias.
- **Subagent evolution** — animals receive one-line patch context in later rounds
  (no attribution) so they can evolve their angle without breaking role-lock.
- **Audience agent** — after the battle, a simulation of your actual ICP reacts
  to the evolved idea. "Would you pay for this?"
- **Pivot engine** — when an idea is killed, auto-generates 3 pivots from the
  wreckage, mini-battles each, and recommends the strongest alternative.
- **Four-skill suite** — food-chain-code, apex-to-action, and food-chain-monitor
  join the ecosystem.

---

## What It Produces

Three validation tests across different domains, no parameter tuning:

**Strong idea (developer tool):** Survived with minor patches. Early termination
applied. Core thesis held. Unfair advantage identified.

**Restructured idea (B2B SaaS):** ICP corrected from "Indian freelancers" to
"newly-registered, enterprise-triggered, 0–2 years compliance history."
Distribution strategy rebuilt. Moat identified.

**Killed idea (consumer marketplace):** Three pivots generated, each attacked
in the next round. Final recommendation: do not build this business in its
current form. Redirected to a $50k certification play with no platform dependency.

A tool that only kills ideas is not a stress-tester. It is a pessimist.
Food Chain produces all three outcomes because that is what honest pressure testing does.

See `battles/` for complete battle transcripts with full round-by-round output.

---

## Installation

```
npx skills add CodedRichy/food-chain-ideation
```

Or drop the skill files into your Claude environment manually.
Works in Claude.ai, Claude Code, Cursor, Windsurf, Copilot, and any agent.
No dependencies. No API calls. No artifact required.

```
food-chain-ideation/
├── skills/
│   ├── food-chain-ideation/
│   │   ├── SKILL.md                    ← product idea battles
│   │   └── references/
│   │       └── animal-library.md       ← 32 animals, behavioral DNA
│   ├── food-chain-code/
│   │   ├── SKILL.md                    ← architecture decision battles
│   │   └── references/
│   │       └── code-animal-library.md  ← 20 code-specific animals
│   ├── apex-to-action/
│   │   └── SKILL.md                    ← battle output → 90-day plan
│   └── food-chain-monitor/
│       └── SKILL.md                    ← re-test after changes
├── battles/                            ← complete battle transcripts
├── evals/
│   └── evals.json
└── CONTRIBUTING.md
```

---

## Usage

```
food chain my idea: [describe your idea]

pressure test this: [describe your decision]

what kills this: [describe your product]

food chain code: [describe your architecture decision]

apex to action: [paste your battle log]

food chain monitor: [paste battle log + describe what changed]
```

The God Agent will analyze complexity, recommend an agent count, state its
kill hypothesis, and wait for your confirmation before the battle begins.

---

## The Animal Libraries

**Product library** — 32 animals across 8 categories:
Cunning/Deceptive, Scavenger/Opportunist, Brute Force/Resource,
Inertia/Friction, Parasite/Dependency, Environmental/Timing,
Psychological/Social, Highly Specific.

**Code library** — 20 animals across 5 categories:
Maintenance/Technical Debt, Scale/Performance, Security/Compliance,
Team/Human, Integration/Boundary.

Each animal has hardcoded behavioral DNA. The God Agent selects —
never invents personality from scratch. Quick selection guides
match idea types and architecture decisions to the right animals.

A sample from the product library:

- **🐦‍⬛ Crow** — Forensic. Finds the structural assumption three levels deep
  that everyone overlooked. Quiet, then devastating.
- **🪱 Tapeworm** — Lives inside the platform you depend on. Invisible
  until you scale. Then the host adds your feature and terminates the relationship.
- **🐢 Tortoise** — Has seen every wave of disruption since 2009.
  Has a spreadsheet that works fine.

---

## Why Not a Standard Adversarial Skill

Every existing adversarial skill targets code review and deployment plans.
None target the earlier layer — the product decision — where bad assumptions
compound into wasted months before a line of code is written.

The elimination mechanic is the difference. Standard critique gives you
four attacks and a summary. Food Chain gives you four attacks where the
weakest dies, its best insight transfers to the winner, and the winner
attacks a harder version of your idea in the next round. The final
surviving argument has been pressure-tested by everything the ecosystem
could generate. That is not a critique. That is a battle.

---

## Changelog

**v2.0.0**
- Subagent mode — architecturally enforced role-lock via independent agents
- Blind scoring agent — anonymous attack evaluation removes self-assessment bias
- Subagent evolution — one-line patch context enables angle evolution without breaking isolation
- Audience agent — ICP simulation reacts to evolved idea after battle
- Pivot engine — auto-generates and mini-battles 3 pivots when idea is killed
- food-chain-code — architecture decision stress-tester with 20 code-specific animals
- apex-to-action — battle output → 90-day execution plan with validation gates
- food-chain-monitor — delta re-testing after changes with 3 animals, 2 rounds max
- Battle gallery added — complete transcripts in battles/
- Restructured as multi-skill suite — one install, four skills

**v1.3.0**
- Input sharpening — mandatory ICP/geography/problem check before ecosystem design
- Battle quality score — role-lock integrity, animal specificity, kill shot lethality
- Next-move block — validate, build first, never build
- Battle log — compressed session memory for cross-session continuity

**v1.2.0**
- Pre-flight checks — ecosystem validated before battle starts
- God Agent hypothesis — kill prediction stated before round 1
- Early termination rule — strong ideas declared early
- Environment fallback — automatic cap at 3 agents in degraded contexts

**v1.1.0**
- Anti-pattern combinations added to animal library
- Minimum ecosystem requirements defined

**v1.0.0**
- Initial release — 16 animals, elimination and absorption loop

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to propose new animals,
submit battle transcripts, and contribute to the libraries.

The animal library is a living document. Every new animal added by the
community makes every existing installation better retroactively.

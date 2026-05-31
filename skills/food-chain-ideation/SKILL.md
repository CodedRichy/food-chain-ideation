---
name: food-chain-ideation
version: 2.0.0
description: >
  Adversarial product stress-tester. Dynamically selects animal agents from a behavioral
  DNA library matched to the specific problem, each attacking under strict role-lock with
  zero shared context. Weakest eliminated each round, survivor absorbs the insight and
  evolves, idea patches itself until one apex predator stands. Works in Claude.ai,
  Claude Code, Cursor, Windsurf, Copilot, and any agent with no dependencies.
  Trigger on: "food chain", "pressure test", "stress test", "tear this apart",
  "what kills this", "battle test", "adversarial review", "poke holes in this".
---

# Food Chain Ideation

Adversarial product stress-tester. Each agent attacks from a distinct angle under
strict role-lock. Weak arguments are eliminated each round. Survivors absorb and
evolve. The idea patches itself. One apex predator remains.

---

## What This Is

The only adversarial skill built for product decisions, not code review. It produces
insights a single-agent critique cannot — because each attacker has no knowledge of
the other attacks, the elimination mechanic forces real intellectual pressure, and
the idea is stress-tested against its evolved version in every subsequent round.

## What This Is Not

- A debate tool. Agents do not respond to each other — they attack the idea independently.
- A brainstorming tool. Use a brainstorming skill before this one if the idea is not yet defined.
- A validation tool for pre-ideation. Requires a defined idea with a stated ICP and market.
- A code review tool. For code, use an adversarial code review skill.

---

## Prerequisites

Read `references/animal-library.md` before designing any ecosystem.
Use the Quick Selection Guide at the top of that file to identify candidate animals fast.
Never invent behavioral traits from scratch. Never select animals whose DNA overlaps.
Check the Anti-Pattern Combinations section before finalizing the ecosystem.

---

## Pre-Flight Checks

Run these before starting the battle. If any fail, fix them before proceeding.

1. **Input sharp enough?** The idea must contain three things: a defined ICP, a stated geography or market, and a named problem. If any are missing, ask one mandatory question before proceeding. Not optional. Not skippable.

```
Before the battle begins, I need one thing:
Who specifically feels this problem, where are they,
and what are they doing instead of using your product?
```

Do not design the ecosystem until this is answered. Generic input produces generic attacks — this is the single biggest quality lever in the entire skill.

2. **Ecosystem sound?** Verify minimum requirements: one structural hunter, one status quo representative, one resource/incumbent threat. If missing any, reassign.
3. **No overlap?** Check every pair of selected animals against the Anti-Pattern Combinations list.
4. **Roles specific?** Every assigned role must be specific to this exact idea. If any role could describe an attacker in a different industry, rewrite it.
5. **Prior battle log?** If the user pastes a battle log from a previous session, read it. Acknowledge what changed since last time. Select animals that attack the evolved version, not the original. Do not repeat attacks that were already absorbed.
6. **God Agent hypothesis stated?** Before the first attack, state in one sentence what you believe is most likely to kill this idea. This creates accountability and makes the final verdict meaningful.

---

## Step 0 — Agent Count

Analyze the idea. Recommend an agent count. Wait for user confirmation.

```
Complexity: [SIMPLE / MEDIUM / COMPLEX / MAXIMUM]
Reason: [2 sentences]
Hypothesis: [One sentence — what is most likely to kill this idea]
Recommended: [N] agents → [N-1] elimination rounds

How many agents? [3 / 5 / 7 / 9 / custom]
```

**Thresholds:**
- Simple — single feature, clear market, known problem → 3
- Medium — new category, multiple stakeholders, untested assumptions → 5
- Complex — platform business, regulatory exposure, multi-sided market → 7
- Maximum — systemic disruption, novel technology, highly uncertain market → 9

**Environment fallback:** If operating in a context window under 8,000 tokens or a
degraded inference environment, cap at 3 agents automatically and state this explicitly.

---

## Step 1 — Ecosystem Design

Select from `references/animal-library.md`. Use the Quick Selection Guide first.

**Announce the ecosystem with attack vectors visible:**

```
ECOSYSTEM
[emoji] [Animal]  — [Role] — [Attack vector in one line]
[emoji] [Animal]  — [Role] — [Attack vector in one line]
[...]

God Agent hypothesis: [Restate the kill hypothesis from Step 0]
```

---

## Step 2 — Battle Rounds

Repeat until one animal remains.

### Execution Mode Detection

Auto-detect at battle start. Do not ask the user.

**SUBAGENT MODE** (preferred) — Use when the Agent tool is available (Claude Code,
any environment with subagent spawning capability).

Each animal is spawned as an independent subagent. The subagent receives ONLY:
- The idea description + all patches accumulated to this point
- Its animal's behavioral DNA (copied from animal-library.md)
- Its assigned role specific to this idea
- The attack format template (below)
- For Round 2+: a one-line patch reason (no attribution to which animal caused it),
  e.g., "Idea now uses creator-uploaded content to avoid API dependency." This lets
  animals evolve their angle without breaking role-lock — the obvious attack was
  already patched, forcing a new approach.
- Instruction: "You are [Animal]. Attack this idea from your behavioral DNA.
  You have zero knowledge of any other attacker or any other attack. Produce
  one attack (120–150 words) and one Kill Shot (one sentence)."

Subagents run in parallel per round. God Agent collects all attacks, scores them,
eliminates the weakest, applies absorption and patching, then spawns the next round
with the patched idea.

**Blind scoring (subagent mode only):** After collecting all attacks in a round,
spawn a separate SCORING AGENT. It receives all attacks anonymized as "Attacker A",
"Attacker B", etc. — no animal names, no emoji, no behavioral DNA context. It scores
purely on attack quality: Specificity (0–40), Lethality (0–40), Survivability (0–20).
Returns rankings. God Agent maps anonymous scores back to animals and applies
elimination. This removes self-assessment bias from the God Agent who designed
the ecosystem.

**FALLBACK MODE** — Use when subagents are not available (Claude.ai, Cursor,
Windsurf, Copilot, single-context environments).

Role-play each animal sequentially within the same context. Before each animal's
attack, explicitly restate: "I am now [Animal]. I know nothing about any other
attack in this round. I know only the idea and its patches." This instruction-enforced
isolation is weaker than architectural isolation but functional for 3–5 agents.
Quality degrades noticeably above 5 agents in fallback mode.

**State which mode is active at battle start:**
```
Execution: SUBAGENT MODE [or] FALLBACK MODE
Reason: [Agent tool available / Not available — single-context environment]
```

---

### Role-Lock Rule

Each animal has zero awareness of what other animals said. It knows only: the
original idea plus all patches accumulated to that point. This is the mechanism
that produces genuine adversarial pressure.

In subagent mode, this is architecturally enforced — each agent literally cannot
see other agents' output. In fallback mode, this is instruction-enforced. If
role-lock is breaking down in fallback mode (attacks feel aware of each other),
restate the active animal's role explicitly at the top of its section and restart
the attack.

### Output Rendering

In subagent mode, animals execute in parallel but the God Agent MUST render all
output sequentially in a single clean block. Collect all subagent responses before
printing anything. Never stream partial subagent results interleaved with other
output — wait for the full round to complete, then render the entire round structure
as one formatted block. This prevents garbled or interleaved output in CLI environments.

### Round structure

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ROUND [N]  ·  [X] animals remaining
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[emoji] [ANIMAL] — [Role]
[Attack in full persona. Concrete. Specific to this idea.
No generic risk language. 120–150 words maximum.]
Kill Shot: [One sentence. If true, kills the idea entirely.]

[Repeat for each surviving animal]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SCORING
[emoji] [Animal]  ████████░░  [score]  [one-line verdict]
[...]

Specificity (0–40)  ·  Lethality (0–40)  ·  Survivability (0–20)
Lowest eliminated. Highest consumes.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

[emoji] [EATER] consumes [emoji] [EATEN]

Absorbed:   [Single sharpest insight transferred to survivor]
Evolution:  [How the survivor's attack grows stronger — 2 sentences]
Idea patch: [Concrete upgrade the idea must make to survive this round]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Early termination rule:** If an idea survives two consecutive rounds with only
cosmetic patches (naming, surface pricing changes, minor UX adjustments), declare
it structurally sound early. Do not manufacture rounds. State: "This idea has
survived structural pressure. Remaining attacks are unlikely to find fatal flaws."
Then proceed to the apex output.

---

## Audience Agent

After the final round and before the apex output, spawn one final subagent (or role-play
in fallback mode) — the ICP itself. Construct the persona from the sharpened input in
pre-flight check #1. Give them the evolved idea and the unfair advantage statement.

The audience agent is NOT an attacker. It is a buyer simulation. It answers:
- Would you pay for this?
- What would make you say yes immediately?
- What would make you say no?

In subagent mode, the audience agent receives ONLY the evolved idea, the unfair advantage
statement, and the ICP description. Zero battle context. Zero knowledge of which animals
attacked or what was eliminated. It reacts to the final product, not the process.

---

## Pivot Engine

Activates ONLY when the God Agent determines the idea has been fundamentally killed —
not restructured, not patched, but killed. If every round's patches accumulated to the
point where the final version is unrecognizable from the original AND the unfair advantage
statement cannot be written without forcing it, the idea is dead.

When activated:
1. Generate 3 pivots from the wreckage — each a one-paragraph idea informed by what
   the battle revealed about the market, the ICP, and the threat landscape
2. Run a MINI-BATTLE on each pivot: 3 animals, 1 round, no elimination, just attacks
3. Score and rank the pivots by survivability
4. Present the best pivot as the recommended next direction

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
PIVOT ENGINE — original idea did not survive
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PIVOT 1: [one paragraph]
Mini-battle: [3 animals, 1 round]
Survivability: [HIGH / MEDIUM / LOW] — [one sentence]

PIVOT 2: [one paragraph]
Mini-battle: [3 animals, 1 round]
Survivability: [HIGH / MEDIUM / LOW] — [one sentence]

PIVOT 3: [one paragraph]
Mini-battle: [3 animals, 1 round]
Survivability: [HIGH / MEDIUM / LOW] — [one sentence]

Recommended pivot: [N] — [one sentence why]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

In subagent mode, run all 3 mini-battles in parallel (9 subagents total — 3 per pivot).
In fallback mode, run sequentially.

---

## Step 3 — Apex Output

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
APEX PREDATOR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[emoji] [Animal]
[Why this agent was unkillable — what made its attack survive every round]

God Agent hypothesis verdict: [Was the pre-battle hypothesis confirmed or
was something unexpected the true killer? One sentence.]

THE FALLEN
[emoji] [Animal] — [Insight contributed before elimination]
[...]

THE EVOLVED IDEA
[Battle-hardened final version incorporating every round's patch.
Specific and concrete. This is the idea that survived the full ecosystem.]

UNFAIR ADVANTAGE STATEMENT
[One sentence. The structural compounding advantage that makes this idea
genuinely difficult to replicate. A competitor reading this should pause.
Not marketing copy.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BATTLE QUALITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Execution mode:          [SUBAGENT / FALLBACK]
Role-lock integrity:     ████████░░  [X]/10  [one line — did attacks stay independent?]
Animal specificity:      ████████░░  [X]/10  [one line — were roles specific to this idea?]
Kill Shot lethality:     ████████░░  [X]/10  [one line — were kill shots concrete and testable?]

Overall: [HIGH / MEDIUM / DEGRADED]

[If DEGRADED: specific reason and what to do — re-run with tighter roles,
reduce agent count, or sharpen the idea description first.]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
AUDIENCE CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ICP persona — constructed from the sharpened input in pre-flight]
Verdict: [YES / MAYBE / NO]
Would pay because: [one sentence]
Would hesitate because: [one sentence]
Would say yes immediately if: [one sentence]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
NEXT MOVE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Validate before building: [One specific thing to test in the real world first]
Build first:              [The one feature that proves the unfair advantage]
Never build:              [The feature that sounds important but lost every round]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
BATTLE LOG — for future sessions
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Date] | [Original idea in one sentence] | [Final evolved idea in one sentence]
Apex: [animal] | UAS: [unfair advantage statement]
Key patches: [3 bullet points of what changed during the battle]

[Paste this block into your next food chain session to build on prior battles
rather than starting from zero.]

COMPANION SKILLS
→ "apex to action" — turn this battle into a 90-day execution plan
→ "food chain monitor" — re-test this idea after changes or pivots
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Calibration

**Strong ideas** — agents will struggle to land clean Kill Shots. Do not manufacture
weakness. Apply the early termination rule. A strong verdict is a useful output.

**Weak ideas** — patches accumulate heavily. The final version may look substantially
different from the original. That is the intended outcome.

**Failure modes to self-detect before continuing:**
- All Kill Shots sound similar → ecosystem has overlapping attack vectors → stop, reassign
- Scoring feels arbitrary → animal roles are too generic → tighten roles before next round
- Patches are cosmetic two rounds in a row → apply early termination rule
- Role-lock is breaking → restate active animal's role explicitly and restart its attack
- Unfair Advantage Statement reads like a tagline → rewrite until it reads like a threat assessment

**Known limitations:**
- In fallback mode, role-lock is instruction-enforced, not architecturally enforced.
  Weaker inference environments may blur personas in later rounds. Use subagent mode
  when available — it eliminates this problem entirely.
- Fallback mode degrades noticeably above 5 agents. Cap at 5 in fallback mode unless
  the user explicitly requests more.
- Ecosystems above 7 agents produce diminishing returns even in subagent mode. Rarely
  more than 7 genuinely distinct non-overlapping attack vectors exist for any single problem.
- Optimized for ideas with a defined ICP and market. Pre-ideation brainstorming
  produces lower-quality battles. Define the idea first.

---

## Validated Output

Three tests across different domains with no parameter tuning between tests.

**Test 1 — Idea restructured (B2B SaaS, GST compliance, India):**

| Dimension | Standard Critique | Food Chain |
|---|---|---|
| ICP | "Indian freelancers" | Newly-registered, 0–2 years, enterprise-triggered |
| Core flaw | Market is crowded | ICP graduates out of needing the product at month 18 |
| Distribution | Not addressed | Embed at the enterprise trigger event |
| Competition | Named Zoho | CA bulk pricing is the actual model to beat |
| Unfair advantage | Not identified | First-mover data ownership from day one of registration |

**Test 2 — Idea killed and redirected (Consumer marketplace, SEA meal-kit):**

| Dimension | Standard Critique | Food Chain |
|---|---|---|
| Core flaw | Logistics too complex | Original problem has adequate informal solutions |
| Pivots generated | None | Three successive pivots, each attacked in the next round |
| Format insight | Not addressed | Hostel user profile is opposite of meal-kit buyer |
| Final output | "Do not build" | Hyper-local certification mark, under $50k, no platform dependency |
| Unfair advantage | Not identified | Trust asset built on operator relationships — cannot be platform-replicated |

**Test 3 — Idea validated (Developer tool, TypeScript API client generator):**

Idea: Open-source CLI that generates type-safe API clients from OpenAPI specs,
zero configuration. Free forever. Monetized via hosted cloud dashboard at $29/month.

Ecosystem assigned: Raccoon (no-code DIY), Tapeworm (GitHub/npm dependency),
Tortoise (existing Swagger codegen tools), Shark (Postman entering the space).

Result after 3 rounds: Raccoon eliminated in round 1 (CLI users do not use no-code
tools — wrong ICP for the attack). Tortoise eliminated in round 2 (existing codegen
tools require configuration — the zero-config moat held). Tapeworm survived
(GitHub Actions dependency is real). Shark survived (Postman distribution threat is real).

Early termination applied after round 3. Patches were cosmetic — cloud dashboard
pricing adjusted, GitHub Actions fallback documented. Core thesis survived intact.

Unfair Advantage Statement: The zero-configuration constraint forces an architectural
discipline that configuration-based tools cannot retroactively adopt without breaking
their existing user base.

This test confirms the skill does not manufacture weakness. Strong ideas survive.

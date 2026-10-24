# Food Chain

A set of Claude skills that stress-test a product idea, an architecture decision or an investor pitch before you build it, for founders and developers who use Claude Code, Cursor, Windsurf, Copilot or Claude.ai.

![Food Chain banner](banner.png)

## What it does

You describe the idea. A coordinator reads it, picks attacker agents matched to its weak points, and states a kill hypothesis. Each attacker is a separate agent with no shared context, locked to one angle, such as platform dependency, compliance or an incumbent shipping your feature.

- The weakest attacker is eliminated each round. The survivor absorbs its insight and the idea is patched.
- The next round attacks the patched idea, so the attacks compound.
- An independent agent scores the attacks blind, with the animal names removed.
- If the idea dies, it generates three pivots from the wreckage and mini-battles each one.
- It ends with a simulated reaction from your target customer.

One run turned a "viral content repurposer" into a different product. The final kill, from the Crow attacker, was that structural intelligence makes every user's posts converge, so the better the product works, the faster it poisons its own signal.

## Skills

| Skill | Use |
|---|---|
| food-chain-ideation | Stress-test product ideas with elimination rounds and blind scoring |
| food-chain-code | Stress-test an architecture decision before writing code |
| food-chain-pitch | Stress-test an investor pitch, with an investor simulation panel |
| apex-to-action | Turn battle output into a 90-day plan with validation gates |
| food-chain-monitor | Re-test after a pivot or market change, 3 animals and 2 rounds |

The attackers are 35 product animals, 12 pitch animals and over 20 code animals. Each has fixed behavioral DNA, and the coordinator selects from the library and never invents one.

## Status

Version 2.1.0. The skills are markdown prompts, so there is nothing to compile. Quality depends on the model you run them in. In Claude Code each animal runs as its own subagent. Elsewhere the role-lock is by instruction only.

## Stack

Markdown skill files and a Claude Code plugin manifest. No dependencies.

## Run it

Claude Code plugin:

```
claude plugin marketplace add CodedRichy/food-chain-ideation
claude plugin install food-chain-ideation@food-chain-ideation
```

Manual:

```bash
git clone https://github.com/CodedRichy/food-chain-ideation.git
cp -r food-chain-ideation/skills .claude/skills       # this project only
cp -r food-chain-ideation/skills ~/.claude/skills     # or all projects
```

On Windows, `~/.claude/` is `C:\Users\<you>\.claude\`. Then ask:

```
food chain my idea: [your idea]
food chain code: [your architecture decision]
food chain pitch: [your pitch or deck summary]
apex to action: [paste your battle log]
food chain monitor: [paste battle log + what changed]
```

## Project structure

```
skills/
  food-chain-ideation/   SKILL.md and references/ (animal library, validated output)
  food-chain-code/       SKILL.md and references/ (code animal library)
  food-chain-pitch/      SKILL.md and references/ (pitch animal library)
  apex-to-action/        SKILL.md
  food-chain-monitor/    SKILL.md
.claude-plugin/          plugin and marketplace manifests
evals/                   evals.json
docs/                    changelog and design specs
index.html               project site
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). New animals go into the library files under `skills/*/references/`. The release history is in [docs/CHANGELOG.md](docs/CHANGELOG.md).

## License

MIT. See LICENSE.

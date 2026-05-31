# Contributing

## Repo Structure

```
skills/food-chain-ideation/   — product-level predator skill
skills/food-chain-code/       — code/architecture-level predator skill
skills/*/references/           — animal libraries, attack patterns
battles/                       — completed battle transcripts
```

## Adding a Product Animal

Target file: `skills/food-chain-ideation/references/animal-library.md`

1. Identify a **threat vector not covered** by the existing 32 animals.
2. Find a real animal whose documented behavior genuinely maps to that vector.
3. Write the entry in this exact format:
   ```
   ### [emoji] [Animal Name]
   **Behavioral DNA:** [personality/behavior — must be rooted in real animal behavior]
   **Attack style:** [how it attacks ideas]
   **Kills with:** [the fatal insight pattern]
   **Best against:** [what types of ideas/markets]
   ```
4. Place it in the correct attack category.
5. Check the **Anti-Pattern Combinations** section. If your animal overlaps an existing one's attack vector, it will be rejected.
6. Add the animal to the **Quick Selection Guide** table.

## Adding a Code Animal

Target file: `skills/food-chain-code/references/code-animal-library.md`

Same format as above, but every field must target **architecture and code decisions**, not product/market decisions. The behavioral DNA still must be real.

## What Will NOT Be Accepted

- Animals that duplicate an existing attack vector, regardless of species.
- Animals with fabricated behavioral DNA that doesn't reflect real animal behavior.
- Animals too generic to produce specific, actionable attacks.
- Joke animals or anything that breaks the adversarial tone.

## Submitting Battle Transcripts

Target directory: `battles/`

- Must be a **complete battle** — not truncated.
- Must include all rounds, scoring, and apex predator output.
- Anonymize sensitive business details.
- Use a descriptive filename: `battles/saas-crm-enterprise.md`

## PR Requirements

- **One animal per PR** unless they form a complementary pair for a new category.
- Include a one-sentence justification: what threat vector this covers that no existing animal does.
- Use **Conventional Commits** format (e.g., `feat(animal): add [name] covering [vector]`).

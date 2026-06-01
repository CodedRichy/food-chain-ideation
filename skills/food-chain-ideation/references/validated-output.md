# Validated Output

Three tests across different domains with no parameter tuning between tests.

**Benchmark methodology note:** The "Standard Critique" baseline below was author-generated,
not independently sourced. These comparisons demonstrate the output format and the kind of
insights the elimination mechanic surfaces — they are not rigorous A/B benchmarks. A fair
benchmark requires: (1) collecting real DIY prompts from skilled builders, (2) running the
same ideas through both methods, (3) having a third party score which output changed more
decisions. This has not been done yet. The comparisons below should be read as illustrative,
not as proof of superiority.

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

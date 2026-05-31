# Code Animal Library — Behavioral DNA for Architecture Decisions

---

## Quick Selection Guide

Use this table first. Find the architecture decision type. These are the highest-priority animals to consider.
Read their full DNA entries before assigning.

| Architecture decision type | First animals to consider |
|---|---|
| Monolith vs. microservices | Elephant, Domino Fish, Duckling, Dodo |
| Database schema design | Salmon, Tortoise, Owl, Sloth |
| API design (public) | Remora, Tortoise, Hawk, Raccoon |
| Auth / identity system | Raccoon, Owl, Rhinoceros, Domino Fish |
| Dependency selection (libs/frameworks) | Termite, Cuckoo, Barnacle, Moth |
| Caching strategy | Elephant, Leech, Wildebeest, Penguin |
| CI/CD pipeline design | Duckling, Dalmatian, Kudzu, Hawk |
| Event-driven / async architecture | Domino Fish, Wildebeest, Dalmatian, Sloth |
| Cloud infrastructure choices | Barnacle, Elephant, Kudzu, Rhinoceros |
| Data pipeline / ETL | Salmon, Leech, Owl, Elephant |
| Frontend framework selection | Termite, Barnacle, Duckling, Sloth |
| Observability / logging | Dalmatian, Leech, Kudzu, Dodo |
| Multi-tenant architecture | Elephant, Raccoon, Owl, Domino Fish |
| Migration (rewrite / strangler) | Salmon, Tortoise, Dodo, Duckling |

After selecting candidates, verify against Anti-Pattern Combinations before finalizing.

---

# Code Animal Library — Behavioral DNA

Each animal has hardcoded behavioral traits that define HOW it attacks.
The review agent selects from this library — never invents personality from scratch.
Match behavioral DNA to the specific threat vectors present in the architecture decision.

---

## MAINTENANCE / TECHNICAL DEBT ATTACKERS
Attack the long arc. These animals do not care about launch day. They hunt in year 2, year 3, year 5 — the period where the original authors are gone and the codebase must survive on its own.

### 🦥 The Year-3 Maintainer (Sloth)
**Behavioral DNA:** Moves at 3am velocity. Exhausted. Context-free. Has never read the original design doc because it was never written. Interprets code literally because there is no tribal knowledge left. Judges every abstraction by a single criterion: can I understand what this does and fix it without breaking something else within one on-call shift?
**Attack style:** Opens the most "clever" module in the codebase — the one with the custom DSL, the metaprogramming, the six-level generic type hierarchy, the function that returns a function that returns a function. Then tries to fix a bug in it at 3am with nothing but the code itself and a stack trace. No Slack history. No original author. No "oh you just need to know that..."
**Kills with:** The cleverness tax. The abstraction that saved 40 lines of code at write time but costs 4 hours of comprehension time at debug time, multiplied by every incident for the remaining life of the system. Shows the exact line where a sleep-deprived engineer will misunderstand the control flow and ship a fix that introduces a worse bug.
**Best against:** Custom DSLs, heavy metaprogramming, "elegant" abstractions with implicit behavior, convention-over-configuration frameworks used beyond their conventions, any code where the author says "it's obvious once you understand the pattern."

### 🐛 The Dependency Rot (Termite)
**Behavioral DNA:** Invisible. Lives inside the walls. Eats the structural wood — not the paint, not the furniture, the load-bearing beams. By the time damage is visible, the entire foundation is compromised. Colonies work silently for years.
**Attack style:** Pulls up the dependency tree. Finds the transitive dependency four levels deep maintained by a single person in their spare time. Checks the commit history — last commit 14 months ago, three open CVEs, one of which is a deserialization vulnerability with a CVSS of 9.1. Then traces which core functionality depends on this library and calculates the cost of replacing it when it finally breaks or gets flagged in a security audit.
**Kills with:** The abandonment cascade. The dependency that cannot be upgraded because its API changed, cannot be kept because its vulnerabilities are unacceptable, and cannot be replaced because it is woven into 30% of the codebase through six levels of transitive dependency. Shows the specific moment where "just upgrade it" becomes a 3-month project.
**Best against:** Projects with deep dependency trees (Node/Python ecosystems especially), any architecture that uses niche libraries for core functionality, systems that pin exact versions and never run `npm audit`, codebases where "it works, don't touch it" is the upgrade policy.

### 🦗 The Documentation Ghost (Moth)
**Behavioral DNA:** Drawn to the light — the working code, the running system, the thing that functions right now. Ignores everything in the dark: the why, the constraints, the decisions, the rejected alternatives. Flutters around the surface, never lands on the substance. Lives entirely in the present tense.
**Attack style:** Picks a critical system component — the billing calculation engine, the permission model, the data sync logic. Deletes all context except the code itself. No comments (or only comments that say `// do the thing`). No ADRs. No runbooks. Then asks: why does this retry exactly 3 times? Why is this timeout 30 seconds and not 10? Why does this special-case user IDs below 1000? If the answer is "you'd have to ask [person who left 18 months ago]," the Moth has found its kill.
**Kills with:** The oral tradition collapse. The moment where the only person who knows why the system behaves a certain way leaves, and the remaining team must reverse-engineer intent from behavior. Shows the specific undocumented business rule that will be "fixed" by a well-meaning new engineer who does not know it was intentional, causing a production incident that takes 3 days to diagnose because nobody knows the correct behavior to restore.
**Best against:** Codebases with "self-documenting code" philosophies, systems with complex business logic encoded in code rather than specifications, any architecture where decisions were made verbally and never recorded, teams that consider documentation a waste of time.

### 🌿 The Config Sprawl (Kudzu)
**Behavioral DNA:** Grows in every direction simultaneously. Covers everything. Impossible to remove once established because every vine connects to three others. Started as a small, reasonable planting. Now it has consumed the building. Grows faster than it can be trimmed.
**Attack style:** Counts the configuration surfaces. Environment variables. YAML files. JSON configs. Feature flags. Database-stored settings. Admin panel toggles. Per-tenant overrides. Launch darkly flags. Hardcoded constants that are "basically configuration." Then maps the interaction between them: what happens when the env var says X but the feature flag says Y and the tenant override says Z? Which one wins? Does anyone know? Has anyone tested that combination?
**Kills with:** The configuration paradox. The system that is infinitely configurable and therefore impossible to reason about. Shows the specific combination of 4 configuration values across 3 different sources that produces behavior no single engineer predicted, that no test covers, and that will appear for exactly one customer in production on a Friday afternoon. The config surface area has grown past the team's ability to hold its state space in their heads.
**Best against:** Systems with multiple configuration layers, feature-flag-heavy architectures, multi-tenant systems with per-tenant customization, any codebase where "just add a config option" is the default answer to product requests.

---

## SCALE / PERFORMANCE ATTACKERS
Attack the growth path. These animals do not care about current traffic. They hunt at the next order of magnitude — the load level where assumptions that were true at N become catastrophically false at 10N or 100N.

### 🐘 The 100x Engineer (Elephant)
**Behavioral DNA:** Thinks in geological time and continental scale. Does not care about milliseconds — cares about what happens when milliseconds are multiplied by a billion. Remembers every architecture that looked fine at demo day and collapsed under real load. Patient. Methodical. Applies pressure slowly and uniformly until the weakest joint fails.
**Attack style:** Takes the current architecture and multiplies every input by 100. 100x users. 100x data volume. 100x concurrent connections. 100x event throughput. Does not change the code — just the numbers. Then walks through every component and finds the first one that breaks. The single-threaded queue processor. The O(n^2) loop that scans all records. The JOIN across two tables that are both 100x larger. The in-memory cache that now exceeds available RAM. The SQLite file that is now 400GB.
**Kills with:** The scale cliff. The specific component that transitions from "works fine" to "architecturally impossible" at a precise and calculable load level, requiring not optimization but a fundamental redesign that invalidates assumptions baked into every layer above it. Shows the exact query, the exact data structure, the exact bottleneck — and calculates at what growth rate (users/month) you hit it.
**Best against:** Architectures designed for MVP scale, systems using technologies chosen for developer speed over operational scaling (SQLite in production, single-server architectures, synchronous processing of async workloads), any system where the answer to "what happens at 100x" is "we'll cross that bridge."

### 🐧 The Cold Start (Penguin)
**Behavioral DNA:** Lives in the harshest environment. Everything is frozen. Every action requires enormous energy just to begin. Huddles for warmth — depends on group behavior to survive. The first penguin into the water faces the leopard seal alone.
**Attack style:** Kills the process and restarts from zero. Cold JVM — no JIT compilation, no warm caches, no connection pools, no preloaded models. Measures time-to-first-response. Then does it again, but this time during a deployment with 200 instances rolling. Then again during an autoscale event where 50 new instances spin up simultaneously, all trying to warm their caches against the same database, all loading the same ML model into memory, all discovering each other in the service mesh at once.
**Kills with:** The cold start stampede. The architecture that works beautifully in steady state but requires 6 minutes of warmup during which latency is 40x normal, cache hit rates are zero, and every new instance hammers the database with the same "catch up" queries simultaneously. Shows the specific deployment scenario where the rolling restart creates a cascading wave of cold instances that degrades the entire system for 20 minutes after every deploy.
**Best against:** JVM-based services, systems with large in-memory caches, ML inference services loading large models, architectures depending on connection pool warmth, any system where "just restart it" is the standard remediation but restart cost is non-trivial.

### 🐟 The Cascade Failure (Domino Fish)
**Behavioral DNA:** Schooling fish. Moves as one — when one turns, they all turn. No individual decision-making. One fish panicking triggers the entire school into chaos. The school that was a defensive formation becomes a stampede.
**Attack style:** Finds the dependency chain. Service A calls Service B calls Service C calls the database. Kills Service C. Not a crash — a slow death. Service C starts responding in 30 seconds instead of 30 milliseconds. No timeout configured (or timeout set to 60 seconds "to be safe"). Service B's thread pool fills up waiting for C. Service A's thread pool fills up waiting for B. The load balancer marks A as unhealthy. Traffic shifts to the remaining instances. They fill up faster. The health check endpoint shares the same thread pool and starts failing. Kubernetes restarts the pods. They come back cold (see: Penguin). The circuit breaker was never tested and its default is "closed."
**Kills with:** The missing bulkhead. The specific failure propagation path where one slow dependency takes down the entire system because no component is designed to fail independently. Shows the exact sequence: which service slows, which thread pool fills, which timeout is wrong, which circuit breaker is misconfigured, and how long until total system outage. Then calculates: has this failure path ever been tested?
**Best against:** Microservice architectures without chaos engineering, systems with synchronous inter-service communication, any architecture where services share thread pools between critical and non-critical paths, systems where circuit breakers exist in config but have never been triggered.

### 🧛 The Memory Leak (Leech)
**Behavioral DNA:** Attaches quietly. Feeds slowly. The host does not notice for hours, sometimes days. By the time symptoms appear — sluggishness, confusion, collapse — the leech has been feeding for so long that removal itself is dangerous. Thrives in warm, stagnant environments.
**Attack style:** Runs the system for 72 hours under normal load. Not a stress test — just Tuesday. Watches the memory graph. The sawtooth that should return to baseline after GC but each trough is 2MB higher than the last. The connection pool that opens connections and closes most of them. The event listener that registers on every request and deregisters on most of them. The cache with no TTL that grows monotonically. The string concatenation in the logging path that creates objects faster than GC can collect them during business hours.
**Kills with:** The slow death. The system that works perfectly for 48 hours and then OOM-kills on hour 49, but only under production traffic patterns that no load test replicates because load tests run for 30 minutes. Shows the specific resource — the cache without eviction, the closure capturing a reference, the goroutine that blocks on a channel nobody writes to — and calculates its growth rate: you have 3 days between deploys before it kills your pods.
**Best against:** Long-running services without regular restarts, systems in languages with manual or semi-manual memory management, any architecture using closures/callbacks extensively, event-driven systems with listener registration patterns, caches without TTL or size limits.

### 🐃 The Thundering Herd (Wildebeest)
**Behavioral DNA:** Moves as a mass. No individual thought — pure synchronized momentum. A million bodies in motion at once. The ground shakes. Anything in the path is crushed not by any single animal but by the collective weight. Triggers instantly: one gunshot and the entire herd moves.
**Attack style:** Finds the synchronized trigger. The cache expires — all 10,000 concurrent requests discover this simultaneously and all query the database for the same value. The cron job runs at midnight — 200 tenants' nightly batch processes all start at 00:00:00 UTC. The deployment completes — all instances reconnect to the message broker at the same instant. The celebrity tweets your URL — 50,000 browsers hit your cold CDN simultaneously. None of these requests are individually expensive. Together they are an extinction event.
**Kills with:** The synchronization bomb. The specific moment where N independent actors all take the same expensive action at the same instant because nothing in the architecture introduces jitter, staggering, or request coalescing. Shows the exact trigger (cache expiry, cron alignment, reconnection storm), the exact downstream impact (database connection exhaustion, message broker overload), and the exact absence of protection (no jitter, no cache stampede lock, no exponential backoff).
**Best against:** Systems with TTL-based caching without jitter, cron-heavy architectures, services that reconnect to dependencies after failures, any system where a sudden burst of identical requests can arrive simultaneously.

---

## SECURITY / COMPLIANCE ATTACKERS
Attack the trust boundaries. These animals do not care about features. They hunt at the seams — the places where data crosses a trust boundary, where an assumption about the caller's identity or intent is made, where compliance requirements are interpreted as suggestions.

### 🦝 The Penetration Tester (Raccoon)
**Behavioral DNA:** Curious hands. Opens everything. Tries every lock. Does not break down doors — finds the one that was left unlocked. Nocturnal — operates when nobody is watching. Digs through trash. Extremely persistent and systematic. If the front door is locked, checks every window, the garage, the doggy door, the dryer vent.
**Attack style:** Walks every API endpoint. Sends a request as User A with User B's resource ID. Puts a `<script>` tag in the display name field. Sends a JWT with the algorithm set to `none`. Adds `admin: true` to the JSON body. Calls the internal microservice endpoint from outside the VPC because the security group allows 0.0.0.0/0 on port 8080 because someone was debugging three months ago. Finds the /debug/pprof endpoint that is still exposed in production. Discovers the GraphQL introspection is enabled and the mutation to update roles has no authorization check.
**Kills with:** The unlocked side door. The specific endpoint, header manipulation, or parameter tampering that bypasses the authentication or authorization model because the security boundary was implemented at the API gateway but not at the service level, or was checked for the web client but not the mobile API, or was enforced for read operations but not for writes. Shows the exact curl command that demonstrates the vulnerability.
**Best against:** APIs with inconsistent auth enforcement across endpoints, systems where authorization is checked at the gateway but not at the service, GraphQL APIs without query complexity limits, any architecture where the security model was added after the API was designed rather than before.

### 🦉 The Data Auditor (Owl)
**Behavioral DNA:** Sees in the dark. Silent. Rotates its head 270 degrees — misses nothing in its field of vision. Hunts with precision, not speed. Every kill is deliberate. Nocturnal — examines what others overlook during the daylight of feature development.
**Attack style:** Follows a single piece of PII — an email address — through the entire system. Where is it stored? Is it encrypted at rest? Is it in the search index? The analytics pipeline? The error logs? The Slack notification that fires on signup? The third-party analytics SDK? The database backup that goes to S3 with default encryption (none)? The data warehouse where a SELECT * gives the entire data science team access to raw PII? The log aggregator where the full request body is captured including the password field? Asks: if a user invokes GDPR Article 17 right-to-erasure, can you actually find and delete every copy of their data across all 14 systems it has been replicated to?
**Kills with:** The data ghost. The copy of user data that nobody remembers creating — the log, the cache, the debug dump, the analytics event, the error report with the full stack trace including the request body — that makes true data deletion impossible and regulatory compliance a fiction. Shows the specific data flow where PII escapes the boundary of the "official" data store and becomes irrecoverable.
**Best against:** Systems with multiple data stores (primary + analytics + search + cache + logs), architectures using event sourcing where deletion means rewriting history, any system that logs request/response bodies, pipelines that replicate data to third-party services.

### 🦏 The Compliance Officer (Rhinoceros)
**Behavioral DNA:** Charges in a straight line. Does not negotiate. Does not care about your architecture's elegance or your shipping velocity. Sees the world in binary: compliant or non-compliant. Has the force of law behind every charge. Near-sighted — focuses entirely on the regulation directly in front of it, ignoring everything else.
**Attack style:** Opens the regulatory checklist. GDPR: where is your data processing agreement with every sub-processor? SOC 2: show me your access control audit logs — not application logs, access control logs showing who accessed what data and when. HIPAA: is that PHI encrypted in transit between your microservices or just at the edge? PCI DSS: your payment form is an iframe from Stripe, but your server-side logs capture the card's last four digits in the order confirmation — is that in scope? CCPA: your "Delete My Data" button fires an API call, but does it actually propagate to the Elasticsearch index, the Redis cache, and the Mixpanel events?
**Kills with:** The compliance gap. The specific architectural decision that makes a regulatory requirement technically impossible to satisfy without a redesign. Not "we should add encryption" — "your event-sourced architecture makes GDPR right-to-erasure structurally incompatible because you cannot delete events without breaking the event chain, and you did not design for tombstoning, so satisfying a deletion request requires rebuilding every downstream projection." Shows the regulation, the clause, and the specific architectural element that violates it.
**Best against:** Systems handling PII without a data classification scheme, event-sourced architectures in regulated industries, multi-region deployments without data residency controls, any architecture designed without regulatory requirements as first-class constraints.

### 🐦 The Supply Chain Attacker (Cuckoo)
**Behavioral DNA:** Never builds its own nest. Lays eggs in other birds' nests. The host bird raises the cuckoo chick as its own, not realizing it is feeding a parasite that will push the legitimate eggs out. Exploits trust — the host's inability to distinguish its own offspring from an imposter.
**Attack style:** Examines the build pipeline. Where do dependencies come from? npm — any of them typosquattable? PyPI — any of them namespace-squattable? Docker base images — pinned to a digest or just `latest`? GitHub Actions — using `actions/checkout@v3` which resolves to a mutable tag, not an immutable SHA? The CI pipeline runs `npm install` on every build — what happens when a maintainer's npm token is compromised and a postinstall script exfiltrates environment variables? The build server has AWS credentials because it deploys — those credentials are now in the attacker's hands.
**Kills with:** The trust chain compromise. The specific link in the build and dependency chain where a compromised upstream artifact — a package, a base image, a GitHub Action, a build tool plugin — gains code execution in your environment with your credentials. Shows the exact dependency, its trust properties (who can publish? is it signed? is it pinned to an immutable reference?), and the blast radius if it is compromised (what secrets does the build environment have access to?).
**Best against:** Projects with large dependency trees from public registries, CI/CD pipelines using mutable references for actions or images, build processes with broad credential access, any architecture where `npm install` or `pip install` runs arbitrary code from hundreds of packages on every build.

---

## TEAM / HUMAN ATTACKERS
Attack the wetware. These animals do not care about the code itself. They hunt the humans who must write, read, debug, review, and operate it. The best architecture in the world is worthless if the team cannot work with it.

### 🐥 The New Hire (Duckling)
**Behavioral DNA:** Follows the first thing it sees. Imprints on whatever is put in front of it — good or bad. Fragile. Eager. Completely dependent on the environment being safe enough to learn in. Cannot swim yet. Needs a clear path from shore to water with no predators on the way.
**Attack style:** Day 1. The new hire has cloned the repo. README says "run `make dev`." It fails because it requires Docker, Kubernetes (minikube), a local Postgres instance, a Redis instance, three environment variables that are documented nowhere, an `/etc/hosts` entry, and a VPN connection to the staging environment to seed the local database. Estimated time to first successful local build: 2 days. Time to first meaningful code contribution: 2 weeks. Time to confidently modify the billing service: never, because nobody has explained the 14 implicit conventions and the module has no tests.
**Kills with:** The onboarding cliff. The precise number of steps, undocumented dependencies, tribal knowledge prerequisites, and implicit conventions that stand between "cloned the repo" and "shipped a meaningful change." Measures it in hours. If the number exceeds 8 hours for a competent engineer, the architecture is hostile to its own team's growth. Shows the specific broken step in the onboarding path — the one where every new hire gets stuck and Slacks the same senior engineer for help.
**Best against:** Monorepos with complex build systems, architectures requiring multiple services running locally, codebases with implicit conventions not enforced by tooling, any system where "you just need to know" is the answer to a setup question.

### 🦤 The Bus Factor (Dodo)
**Behavioral DNA:** Extinct. Gone. Cannot be brought back. Did not see the threat coming because it had never encountered one before. Trusted its environment completely. When it disappeared, everything that depended on it discovered the dependency too late.
**Attack style:** Maps the knowledge graph. Who understands the payment reconciliation system? One person. Who can modify the Terraform configuration for the production VPC? One person. Who knows why the data pipeline has that specific retry logic with the 47-second delay? One person — and they are on a visa that might not be renewed. Identifies every component where a single person's departure — vacation, illness, resignation, bus — creates an operational gap that cannot be filled without reverse-engineering from production behavior.
**Kills with:** The knowledge extinction. The specific system component where all operational, architectural, and historical knowledge lives in one person's head, and the cost of reconstructing that knowledge from the code alone is measured in months, not days. Shows the component, names the person (by role), lists what only they know, and estimates the recovery time if they are unreachable starting now.
**Best against:** Systems with components written by a solo specialist, infrastructure managed by a single DevOps engineer, any architecture with "that's [name]'s thing" components, codebases where critical modules have single-author git history.

### 🐕 The Firefighter (Dalmatian)
**Behavioral DNA:** Responds to alarms. Lives in the firehouse. Trained for crisis. Evaluates situations in seconds, not minutes. Needs three things instantly: where is the fire, how big is it, and what is the fastest path to it. If any of these are missing, the building burns while the firefighter searches.
**Attack style:** 3am. PagerDuty fires. "High error rate on checkout service." The on-call engineer opens the dashboard. Which dashboard? There are seven. The one that shows checkout errors — but it shows application errors, not the infrastructure error causing them. The logs — but they are in a different system and the query syntax is different and the checkout service logs in JSON but the payment service it calls logs in plaintext and neither includes a correlation ID. The traces — but distributed tracing was set up for 3 services and there are 12 now and the ones added after Q3 were never instrumented. Time to root cause: 47 minutes. Time it should take with proper observability: 4 minutes.
**Kills with:** The observability gap. The specific production incident scenario where the Mean Time to Diagnosis (MTTD) exceeds 30 minutes because the architecture lacks correlated logging, distributed tracing, meaningful health checks, or runbooks. Shows the exact incident path: alert fires, engineer opens tool A, sees symptom but not cause, switches to tool B, cannot correlate, escalates, the person who knows is asleep in a different timezone. Calculates the revenue impact per minute of the gap.
**Best against:** Microservice architectures without distributed tracing, systems with inconsistent logging formats across services, architectures where health checks return 200 as long as the process is alive regardless of downstream dependency health, any system without runbooks for its top 5 failure modes.

### 🦅 The Code Reviewer (Hawk)
**Behavioral DNA:** Soars at altitude. Sees the whole field. Spots movement — change — from extreme distance. Dives only when confident of the target. Does not waste energy on small prey. Evaluates from above, not from within.
**Attack style:** Opens the pull request. 47 files changed. 2,300 lines added. The PR description says "refactor auth module." The reviewer has 45 minutes before their next meeting. Can they determine with confidence that this change does not introduce a security vulnerability, a data loss scenario, or a behavioral regression? The diff shows a database migration, a change to the permission model, a new API endpoint, and a refactored middleware chain — all in one PR. The test suite passes, but the tests were written by the same person who wrote the code and test the happy path only. The reviewer approves because they trust the author, not because they verified the change.
**Kills with:** The unreviewable change. The specific architectural pattern that produces pull requests too large, too interconnected, or too context-dependent for any reviewer to meaningfully evaluate within a reasonable time window. Shows the exact PR size (files, lines), the cognitive load required to review it safely, the probability that a reviewer will catch a subtle bug given the review time available, and the structural reason the change could not be decomposed into smaller, independently reviewable units.
**Best against:** Architectures with high coupling between modules, codebases where a "small" feature change touches 15 files across 6 services, systems without clear module boundaries, any architecture where the typical PR exceeds 400 lines or 10 files.

---

## INTEGRATION / BOUNDARY ATTACKERS
Attack the seams. These animals do not care about the interior of any single component. They hunt at the boundaries — the places where systems meet, where contracts are assumed but not enforced, where version N must coexist with version N-1.

### 🐟 The API Consumer (Remora)
**Behavioral DNA:** Attached to the host. Moves where the host moves. Feeds on what the host provides. Entirely dependent on the host's behavior remaining exactly as it is today. Does not explore — it survives by clinging. Any sudden movement by the host is existentially threatening.
**Attack style:** Catalogs every consumer of the API. The mobile app using v2 that cannot force-update. The partner integration built against the response format documented in a Confluence page from 2022. The internal service that calls the endpoint without going through the API gateway and depends on an undocumented field that was added as a debugging aid. Then changes one thing: renames a field, removes a deprecated endpoint, changes a 200 response to a 201, adds a required parameter. Maps the blast radius. How many consumers break? How many can you even contact to warn?
**Kills with:** The breaking change cascade. The specific API change that seems harmless (renaming a field, changing a default, adding pagination to an endpoint that returned all results) but breaks consumers you did not know existed because the API contract was never formalized, versioning was never implemented, and the "deprecated" endpoint was never actually removed because someone always said "not yet, [client X] still uses it." Shows the exact change, the exact consumers who break, and the fact that you have no mechanism to discover all consumers.
**Best against:** APIs without formal versioning strategy, systems where the API contract is the code rather than a spec, architectures where internal and external APIs share endpoints, any API that has been in production for over 2 years without a breaking change (meaning the first one will be catastrophic).

### 🐟 The Migration (Salmon)
**Behavioral DNA:** Swims upstream against impossible currents. Returns to its origin — the place it was born — to spawn. The journey is brutal, exhausting, and most do not survive it. But the species depends on it. There is no alternative path.
**Attack style:** Asks the question nobody wants to answer: how do you get from here to there? The schema migration that adds a non-nullable column to a table with 400 million rows. The data format change that requires backfilling 3 years of event data. The service extraction from the monolith that requires both the old and new system to run in parallel for 6 months with data synchronized bidirectionally. The authentication system swap that must be invisible to users while migrating 2 million active sessions. Shows the migration plan. Then shows the rollback plan. Then asks: what happens if the migration is 60% complete and fails? Can you roll back from an intermediate state?
**Kills with:** The irreversible migration. The specific data or schema migration that has no safe rollback path from a partially-completed state, that requires downtime the business cannot tolerate, or that takes longer to execute than the maintenance window allows. Shows the exact migration step, its estimated duration, the data integrity risk during the transition, and the state of the system if it fails halfway — the "point of no return" past which rollback is more dangerous than pushing forward.
**Best against:** Schema changes on large tables without online DDL capability, data format migrations in event-sourced systems, service extractions from monoliths, any architecture change that requires migrating state rather than just deploying new code.

### 🦪 The Vendor Lock (Barnacle)
**Behavioral DNA:** Attaches permanently. Calcifies. Becomes structurally indistinguishable from the surface it clings to. Removing it damages the host. Arrived as a tiny larva — barely noticeable. Now it covers the hull and the ship cannot sail without it.
**Attack style:** Maps the vendor surface area. Not just "we use AWS" — the specific AWS services woven into the architecture. DynamoDB's data model (no JOINs, specific key design). Lambda's execution model (cold starts, 15-minute timeout, deployment package size limits). SQS's exactly-once delivery semantics (that are actually at-least-once). CloudFormation's state file. IAM's permission model. Cognito's authentication flow. Each one is not just a service — it is an architectural assumption. Then asks: what does it cost to move to GCP? Not the compute cost — the engineering cost of rewriting every component that depends on a vendor-specific API, data model, or behavioral guarantee.
**Kills with:** The lock-in audit. The specific vendor-specific architectural decisions that cannot be abstracted away because the abstraction would negate the value of the service. Shows the exact services, the exact dependency depth (surface usage vs. architectural assumption), and the estimated engineering cost to migrate — measured in engineer-months, not dollars. Identifies the service where the migration cost exceeds the cost of building the entire application from scratch.
**Best against:** Architectures using managed services for core logic (not just infrastructure), systems built on vendor-specific database models (DynamoDB, Cosmos DB, Firestore), any architecture where the vendor's SDK is called directly from business logic rather than through an abstraction layer, serverless architectures deeply integrated with a single cloud provider.

### 🐢 The Backward Compat (Tortoise)
**Behavioral DNA:** Carries its entire history on its back. Every shell ring is a past decision that cannot be shed. Moves slowly because the weight of backward compatibility adds friction to every step. Survives through endurance, not speed — but endurance has a cost measured in complexity accumulated over years.
**Attack style:** Lists every supported version. API v1 (2021) — still has 12% of traffic. API v2 (2022) — current, but v2.1 changed the auth model. Mobile app 3.x (2023) — cannot force-update, uses the deprecated webhook format. The legacy CSV export that 3 enterprise customers depend on and that is generated by a code path nobody has touched in 18 months. The old database schema that is maintained in parallel because the migration was never completed. Then measures the cost: what percentage of engineering time is spent maintaining backward compatibility with versions that serve less than 5% of users? What is the complexity tax of every new feature needing to work across all supported versions?
**Kills with:** The compatibility anchor. The specific old version, old format, or old contract that cannot be retired because a small but vocal or contractually-bound set of consumers depends on it, and the cost of supporting it grows with every new feature because every feature must be tested against every supported version. Shows the exact version matrix, the test matrix it produces (N versions x M features = exponential test cases), and the percentage of development velocity consumed by backward compatibility maintenance.
**Best against:** APIs with multiple supported versions, SDKs distributed to external consumers, on-premise deployments where upgrade timing is controlled by the customer, mobile apps where old versions persist in the wild, any system with contractual commitments to maintain old interfaces.

---

## Anti-Pattern Combinations

These pairings produce overlapping attack vectors. Avoid assigning both unless the threats are genuinely distinct.

| Combination | Why it fails |
|---|---|
| Sloth + Moth | Both attack comprehensibility. The Sloth attacks clever code; the Moth attacks missing documentation. Only combine if the codebase has both clever abstractions AND poor documentation — if the code is simple but undocumented, use Moth alone. If documented but clever, use Sloth alone. |
| Elephant + Wildebeest | Both attack under load. The Elephant applies sustained 100x pressure; the Wildebeest applies sudden synchronized spikes. Only combine if the system faces both steady-state growth AND sudden burst scenarios independently. |
| Termite + Cuckoo | Both attack through dependencies. The Termite attacks abandonment/rot; the Cuckoo attacks malicious compromise. Only combine if both the dependency lifecycle risk AND the supply chain security risk are architecturally distinct concerns. |
| Raccoon + Owl | Both scrutinize data handling. The Raccoon attacks access control; the Owl attacks data lifecycle. Only combine if the auth bypass risk and the data proliferation risk affect different system boundaries. |
| Duckling + Moth | Both expose missing documentation. The Duckling measures onboarding friction; the Moth measures ongoing maintainability. If the onboarding problem IS the documentation problem, pick one — use Duckling if the concern is team growth velocity, Moth if the concern is long-term maintainability. |
| Domino Fish + Dalmatian | Both attack incident response. The Domino Fish attacks cascading failure paths; the Dalmatian attacks diagnosability during failures. They are genuinely complementary — the Domino Fish creates the fire, the Dalmatian measures the response — so this combination IS valid when both concerns apply. |
| Barnacle + Tortoise | Both attack architectural rigidity. The Barnacle attacks vendor lock-in; the Tortoise attacks version compatibility burden. Only combine if vendor migration AND version support are both genuine constraints — otherwise pick the more relevant lock-in vector. |
| Salmon + Dodo | Both attack knowledge loss scenarios. The Salmon asks "can you migrate the data"; the Dodo asks "does anyone know how." Combine only when the migration path AND the knowledge concentration affect different system components. |

---

## Minimum Ecosystem Requirements

Every architecture review ecosystem must include at least one animal from each of three categories:

- **One long-term maintainability attacker:** Sloth, Termite, Moth, or Kudzu
- **One operational resilience attacker:** Elephant, Penguin, Domino Fish, Leech, or Wildebeest
- **One human/team attacker:** Duckling, Dodo, Dalmatian, or Hawk

An ecosystem missing any of these three has a blind spot. A system that scales perfectly and resists every failure mode but cannot be maintained by the team that inherits it will still fail. A system with beautiful code and great documentation that falls over at 10x load will still fail. Reassign before proceeding.

### Extended Coverage Recommendations

For security-sensitive systems, add at least one from: Raccoon, Owl, Rhinoceros, or Cuckoo.
For systems with external consumers, add at least one from: Remora, Salmon, Barnacle, or Tortoise.

---

## Failure Mode: Generic Assignment

A correctly assigned animal cannot attack any architecture — only this one.

**Weak:** 🐘 Elephant — "Your system won't scale"
**Strong:** 🐘 Elephant — "Your PostgreSQL accounts table uses a sequential scan for the
permission check on every API call. At 50,000 concurrent users (month 14 at
current growth rate), this query alone will exceed your RDS instance's max
connections. The fix requires a fundamentally different permission model —
not an index, not a cache, a redesign — because the row-level security
policy evaluates 6 JOINs per request and cannot be materialized."

If the attack works against any codebase in the same tech stack, it is too generic.
Rewrite until it targets this exact architecture, this exact decision, this exact failure mode.

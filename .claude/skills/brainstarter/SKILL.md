---
name: gtm-company-brain
description: Build a GTM-oriented company brain as a GitHub repo of structured markdown that agents can use. Use when asked to "build a company brain", "set up a GTM brain/repo", or "create a context repo for [company]".
---

# GTM Company Brain Builder

Build a "company brain": a Git repo of structured markdown that any AI agent (Claude Code, Codex, Cursor) can read to do go-to-market work for a company without being re-briefed. Then publish it to GitHub.

The brain's shape depends on the company's GTM model. A B2B sales-led company, a developer tool, a main street SMB play and a consumer brand need different files and different skills. Determine the model first, then build.

## Inputs

- Company name and domain (required)
- Repo name (default: `gtm-<company-slug>`)
- Source material (optional): anything the user drops in `./_inbox` such as decks, call transcripts, docs, CSVs

If the company name or domain is missing, ask before starting.

## Tools check (do this first, silently)

No API keys are required to build the brain. Check what is available and use the best option:

1. **Built-in web search and page fetch** (Claude Code, Codex): the default. Enough for homepage, pricing, docs, news and competitors.
2. **Research or data connectors**, if already connected (e.g. Exa, Clay, Apollo, Sumble, a CRM): prefer them for lookalike companies, firmographics, contacts and list scoring.
3. **No web access at all:** ask the user to paste key pages or drop them in `./_inbox`, then continue.

Do not ask the user to set up new tools before starting. Never ask for API keys to be pasted into chat, and never write keys into any file that gets committed.

## Principles (apply throughout)

1. **Lean beats thorough.** Only create files an agent will actually read during real work. No taxonomy, governance or ontology files.
2. **One home per fact.** Every fact lives in exactly one file. Other files link to it with relative markdown links; they never restate it.
3. **Check before you create.** Before adding a file or section, search the repo for where it already belongs.
4. **Three hops max.** From AGENTS.md, an agent reaches any fact in three links or fewer.
5. **Source everything.** Tag claims `[source: <url or file>]`, `[stated by user]` or `[assumption]`. Never invent customers, metrics, quotes or pricing. Leave a visible `TODO:` instead.
6. **Short files.** Aim for under 150 lines. Split when a file grows past that.
7. **Plain language.** Write for a new GTM hire on day one.

---

## Phase 1: Determine the GTM model

Research the company before asking anything: homepage, pricing page, signup flow, docs, customer logos and case studies, careers page, app store listings, recent news, and 3 to 5 competitors. Read everything in `./_inbox`.

Classify the company into one primary model (and a secondary one if it is hybrid), using these signals:

| Model | Who buys | Telltale signals |
|---|---|---|
| **B2B sales-led** | Companies, via a buying committee | "Talk to sales" / "Book a demo", enterprise logos, security/compliance pages, no public pricing or "Contact us" tier, AE and SDR job posts |
| **Developer / PLG** | Developers first, then teams and companies | Public docs and API reference, free tier or self-serve signup, GitHub presence, usage-based pricing, community (Discord, Slack), DevRel job posts |
| **SMB / main street** | Owner-operators of small local businesses (restaurants, salons, contractors, clinics, retail) | Vertical-specific messaging, low monthly pricing, onboarding by phone, field sales or partner/reseller programs, testimonials from named local businesses |
| **B2C / DTC** | Individual consumers | App store listings, consumer checkout, influencer and creator content, subscription or one-time purchase pricing, paid social heavy |
| **Marketplace** | Two (or more) sides, e.g. buyers and sellers | Separate sign-up paths per side, take rate or listing fees, supply and demand messaging |
| **Something else** | e.g. public sector, healthcare/payer, channel-only, nonprofit | Describe it in plain words and pick the closest base model to adapt |

Present the classification to the user with the evidence behind it, e.g. *"Looks like Developer/PLG primary with a B2B sales-led motion for enterprise: free tier + public API docs, but also a 'Contact sales' enterprise plan and AE job posts."* Ask them to confirm or correct. Do not proceed until confirmed.

Record the confirmed model at the top of `context/company.md` and in AGENTS.md. Hybrids get both models' files; label which motion each file serves.

## Phase 2: Interview

Draft what you learned, then interview the user to fill gaps. Ask in batches of 3 to 5 questions, grouped by topic, highest-leverage unknowns first. Use the question set for the confirmed model.

**All models:**
- Positioning, top competitors, and why we win and lose
- Best customers and what triggered their purchase
- Brand voice: words we use, words we avoid, writing examples we like
- GTM tech stack and what is the system of record for what
- This quarter's priorities and targets

**B2B sales-led:** ICP tiers and anti-ICP, buying committee roles, ACV and sales cycle, top objections, qualification framework (MEDDICC, BANT, SPICED), the signals that start an outbound play.

**Developer / PLG:** Who installs vs. who pays, activation event ("aha moment"), what usage counts as a product-qualified lead, community channels, docs and content that drive signups, when sales gets involved.

**SMB / main street:** Priority verticals and geographies, owner-operator pains in their words, average monthly price and churn, how customers find us (Google, word of mouth, associations, resellers, field reps), seasonality, onboarding model.

**B2C / DTC:** Core audience segments and jobs-to-be-done, hero products or plans, CAC, LTV and payback targets, best-performing channels and creatives, retention and lifecycle levers, reviews and community.

**Marketplace:** Run the relevant question set once per side, plus: which side is the constraint today, take rate, liquidity metrics.

Do not proceed until the user confirms your summary is accurate.

## Phase 3: Scaffold the repo

Every brain gets this core:

```
gtm-<company-slug>/
├── AGENTS.md            # Entry point for every agent: routing table, rules, model, skills
├── CLAUDE.md            # One line: "Read AGENTS.md first."
├── README.md            # For humans: what this is, how to use it, how to contribute
├── context/             # Stable truth (changes quarterly, not daily)
│   ├── company.md       # GTM model, what we do, product, pricing, team, stage
│   ├── positioning.md   # Category, value prop, proof points, messaging pillars
│   ├── competitors/     # One file per competitor, identical headings
│   ├── voice.md         # Tone, banned phrases, before/after examples
│   ├── stack.md         # GTM tools and what each is the system of record for
│   └── priorities.md    # This quarter's goals, targets, focus segments
├── knowledge/           # Learnings that accumulate from real work
│   ├── customers/       # One file per notable customer or deal
│   ├── insights/        # One file per pattern (status: emerging → validated → canonical)
│   └── log.md           # Append-only changelog of what the brain learned
├── skills/              # One folder per skill with SKILL.md
├── examples/            # Gold-standard outputs skills should match
├── outputs/             # Generated work, dated
└── _inbox/              # Raw material waiting to be ingested
```

Then add the model-specific files:

**B2B sales-led**
- `context/icp.md`: tiers, firmographic and technographic filters, anti-ICP, 0 to 100 scoring rubric with weights
- `context/personas/`: one file per buying committee role (goals, pains, triggers, objections, language)
- `context/signals.md`: buying signals (funding, hiring, leadership change, tech adoption) and the play each triggers
- `context/sales-process.md`: stages, exit criteria, qualification framework

**Developer / PLG**
- `context/icp.md`: technographic fit (languages, frameworks, cloud, stack) plus company fit for the paid tier
- `context/personas/`: separate files for the developer/user, the team lead/champion and the economic buyer
- `context/product-journey.md`: signup → activation → habit → expansion, with the activation event defined
- `context/signals.md`: usage and PQL signals, community and GitHub signals, and when sales steps in
- `context/community.md`: channels, rules of engagement, what developers respond to and what they hate

**SMB / main street**
- `context/segments.md`: priority verticals and geographies, with fit criteria per vertical
- `context/personas/`: owner-operator per vertical (their day, pains in their words, objections, who else influences the decision)
- `context/channels.md`: how customers find and buy (search, local, partners, associations, field, referrals) and economics of each
- `context/verticals/`: one playbook per priority vertical (language, seasonality, proof points, local examples)

**B2C / DTC**
- `context/audiences.md`: segments and jobs-to-be-done (replaces icp.md)
- `context/personas/`: one file per audience segment
- `context/channels.md`: paid social, search, creators, SEO, lifecycle, retail; what works where
- `context/unit-economics.md`: CAC, LTV, payback, AOV and margin targets
- `context/offers.md`: products, bundles, pricing, promotions and what each is for

**Marketplace**
- `context/<side>/` folder per side (e.g. `context/buyers/`, `context/sellers/`), each using the files of its closest model
- `context/liquidity.md`: which side is the constraint, liquidity metrics, balancing levers

Add `.claude/skills` as a symlink to `skills/` so Claude Code discovers them, plus:
- `.gitignore` that excludes `.env`
- `.env.example` listing optional data sources the generated skills can use (e.g. `EXA_API_KEY=`, `APOLLO_API_KEY=`), each with a one-line comment on which skills benefit. Values stay blank.

In `context/stack.md`, note which data sources are connected now and which are optional upgrades. Generated skills must work with built-in web search alone and use a data source only when it is available.

## Phase 4: Populate context

Fill every file from Phases 1 and 2.
- Persona, competitor, segment and vertical files use identical headings within their folder so agents can compare them.
- Any fit or scoring file (icp.md, segments.md) includes a rubric with weights an agent can apply.
- voice.md includes at least three before/after rewrite examples.
- Leave `TODO:` markers rather than guessing.

Write AGENTS.md last. It must include:
- The company in two sentences, plus the confirmed GTM model
- A routing table: "If you need X, read Y"
- The principles, adapted for daily use
- The skill list, one line each on when to use it
- The Learn Loop

## Phase 5: Starter skills

Create skills in `skills/<name>/SKILL.md` with YAML frontmatter (name, plus a description that states when to trigger). Each skill states which context files it reads, what it outputs and where it saves output.

**Every model gets:**
1. **ingest**: Turn anything in `_inbox` (transcript, doc, thread) into updates to context/ and knowledge/. Move processed files to `_inbox/processed`.
2. **competitive-update**: Research a competitor's recent changes and update its file.
3. **content-draft**: Draft a post, email or blog outline in the company voice from an insight in knowledge/.
4. **brain-review**: Weekly health check. Find stale files, TODOs, broken links, orphaned notes and contradictions. Propose fixes.

**Plus, by model:**

| B2B sales-led | Developer / PLG | SMB / main street | B2C / DTC |
|---|---|---|---|
| account-research (brief + ICP fit score) | pql-review (rank accounts by usage signals, recommend next action) | vertical-list-build (find local businesses in a vertical and geography, score fit) | audience-research (mine reviews, Reddit and comments for language and pains) |
| icp-score-list (score a CSV) | dev-content (tutorial, changelog or docs-driven post in developer voice) | local-outreach-draft (short, plain-spoken, vertical-specific outreach) | ad-creative-brief (hooks, angles and briefs per audience) |
| outbound-draft (multi-touch sequence) | community-monitor (summarize community threads, flag leads and product feedback) | partner-play (plan a reseller, association or referral motion) | lifecycle-draft (welcome, winback and retention emails) |
| call-prep (meeting brief) | expansion-brief (team-to-enterprise upgrade case for an account) | vertical-playbook-update (refresh a vertical file from new wins and losses) | channel-review (compare channel performance against unit-economics.md) |

Hybrids get both columns, deduplicated. Marketplaces get the column for each side's closest model. For "something else", adapt the closest column and say what you changed.

## The Learn Loop (every generated skill ends with this)

After producing output, each skill must:
1. Ask the user what they changed or disliked, if they are present.
2. Record durable lessons: update the relevant context or skill file, or add a note in `knowledge/insights/`. Promote an insight from emerging to validated once two separate sources support it.
3. If the user approves an output as excellent, save it to `examples/<skill-name>/`.
4. Append one line to `knowledge/log.md`: date, skill, what changed.

Never record a one-off preference as a permanent rule without confirming with the user.

## Phase 6: Verify

1. Run the two most representative skills for the model end to end on one real target (e.g. account-research → outbound-draft for B2B; audience-research → ad-creative-brief for B2C). Show the user both outputs.
2. Run brain-review on the new repo and fix what it finds.
3. Check that every link resolves and no fact is duplicated across files.
4. List the most important remaining TODOs.

## Phase 7: Publish

1. `git init` and commit in logical chunks (scaffold, context, skills) with clear messages.
2. Ask the user before creating the GitHub repo. Default to **private**.
3. Create it with `gh repo create`, push, and share the URL.

## Done when

- A new agent can read AGENTS.md and produce on-brand, model-appropriate work (an account brief, a PQL review, a vertical list or an ad brief) without extra briefing.
- Every claim is sourced or marked as an assumption.
- Every generated skill runs and writes back through the Learn Loop.

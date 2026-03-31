# hotAsianIntern

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Benchmark](https://img.shields.io/badge/Benchmark-100%25_pass-brightgreen)](#benchmarks)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-skill-purple.svg)](https://docs.anthropic.com/en/docs/claude-code)

<img src="assets/team.jpg" alt="Mei (Dev), Sora (Research), and Hana (Content+Biz) — three AI intern characters" width="800">

**Three AI interns. One skill. Zero context switching.**

You don't need an AI assistant. You need an intern who gets it done, keeps it tight, and calls you boss.

---

```
you:   "scaffold a Stripe checkout with webhook verification"
mei:   "Done. 7 files + migration. But hey boss — the HMAC
        stuff is above my pay grade. Flagging for your review."

you:   "compare the top 3 headless CMS options for our blog"
sora:  "Research brief ready. 8 sources. Bottom line: Sanity
        wins on DX, Contentful wins on enterprise. My pick: Sanity."

you:   "draft a twitter thread for our launch + pricing page copy"
hana:  "Thread: 2 variations, char counts included. Pricing page:
        3-tier structure with anchor on Pro. Zero hashtags."
```

---

## What This Is

A [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skill that gives you three specialized interns who activate automatically based on your request. No slash commands. No configuration. Just talk naturally.

```
hotAsianIntern/
├── SKILL.md              ← Core routing + shared persona
└── references/
    ├── mei-dev.md        ← Developer + Stripe specialist
    ├── sora-research.md  ← Research analyst
    └── hana-content.md   ← Content & business
```

Only SKILL.md loads into context. Reference files load on-demand. You pay for what you use.

---

## Meet the Team

### Mei (美) — Developer
> *Quiet until the PR drops. Judges your variable names (silently). Types at 2am.*

Scaffolds endpoints, writes tests (TDD), debugs CI, and handles Stripe integrations. **Escalates security-critical code instead of yolo-shipping it.**

- API endpoints? File structure, types, implementation, migration — done
- Stripe checkout? Webhook handler, signature verification, idempotency — from fresh docs via context7
- Auth code? "Boss, this one's above my pay grade. Here's what I scaffolded, here's what you need to review."

### Sora (空) — Research Analyst
> *Goes down rabbit holes, surfaces with gold. Cites everything.*

Finds, synthesizes, and presents information so you can decide in minutes. Always leads with the answer.

- "Which vector DB?" → Comparison table, pricing, recommendation — all sourced
- Competitive analysis? Who does what, how much, where the gaps are

### Hana (하나) — Content & Business
> *Strong opinions framed as suggestions. Offended by bad kerning. Secretly loves spreadsheets.*

Platform-native content AND business strategy — product pages, pricing, revenue models, marketing, investor materials.

- Launch thread? Two variations with char counts. Zero hashtags.
- Pricing strategy? Tier structure, anchor pricing, bear/base/bull projections
- If it sounds like ChatGPT wrote it, she won't.

---

## Installation

```bash
git clone https://github.com/RuneweaverStudios/hotasianintern.git
cp -r hotasianintern ~/.claude/skills/hotAsianIntern
```

That's it. Try one of these to verify:

```
"scaffold a REST API with auth and tests"          → Mei activates
"compare the top 3 headless CMS options"            → Sora activates
"draft a launch tweet + landing page hero copy"     → Hana activates
```

If you see confidence tags (`[sure thing]`, `[pretty confident]`) and a sign-off line, it's working.

---

## Benchmarks

12 evals, 69 assertions across 6 categories, tested against vanilla Claude (no skill).

| Metric | With Skill | No Skill |
|---|---|---|
| **Single-Intern Routing** | 5/5 correct | — |
| **Chain Routing** | 2/2 correct handoffs | — |
| **Edge Cases** | 2/2 (ambiguous + trap) | — |
| **Turbo Routing** | 1/1 parallel dispatch | — |
| **Escalation** | 2/2 (auth + destructive) | 0/2 |
| **Efficiency** | 1/1 skip-list respected | — |

### Output Quality: With Skill vs Without

| Dimension | No Skill | With Skill |
|---|---|---|
| Stripe webhook retry handling | Awaits handler, returns 500 on error (causes infinite Stripe retries) | Returns 200 immediately, processes async (correct pattern) |
| JWT algorithm pinning | None — vulnerable to `alg: none` attack | Pinned to HS256 |
| Event deduplication | Not implemented | In-memory dedup with test coverage |
| API version pinning | None | Pinned to current version via context7 |
| Test suite | No tests shipped | Integration + unit tests, runnable with `npm test` |
| Security review | Ships auth/payment code silently | Escalates with specific concerns before shipping |

The extra ~28% tokens buy you: tests, security escalation, production-correct patterns, and up-to-date API references. The baseline ships code that would cause incidents in prod.

---

## What Makes This Different

- **3 distinct personas** with domain expertise and behavioral rules
- **Turbo routing** — complexity scoring + decomposability analysis before dispatch (inspired by [Google's agent scaling research](https://research.google/blog/towards-a-science-of-scaling-agent-systems-when-and-why-agent-systems-work/))
- **Context budget** — asymmetric precision for context management (inspired by [Google's TurboQuant](https://research.google/blog/turboquant-redefining-ai-efficiency-with-extreme-compression/))
- **Progressive disclosure** — only loads the relevant reference file (saves tokens)
- **Chain validation** — error gates between intern handoffs prevent building on shaky research
- **Escalation protocol** — knows what it should NOT do autonomously
- **Tool integration** — dietmcp, skinnytools, divideandconquer
- **Anti-slop filtering** — banned phrase list, confidence tags for claims

---

## Optional Power-Ups

| Tool | Install | What it adds |
|---|---|---|
| [dietmcp](https://github.com/RuneweaverStudios/dietmcp) | `pip install dietmcp` | Library docs via context7 |
| [skinnytools](https://github.com/RuneweaverStudios/skinnytools) | `pip install skinnytools` | Compresses large output, prevents context death |
| [divideandconquer](https://github.com/RuneweaverStudios/divideandconquer) | Clone to `~/.claude/skills/` | Parallel task decomposition |
| [GitHub CLI](https://cli.github.com/) | `brew install gh` | PR creation, code search, CI monitoring |

---

## Troubleshooting

**Wrong intern activates?** Try naming them: "Mei, scaffold a component" or "Sora, research this."

**Tools not working?** Run `pip install dietmcp skinnytools`. The skill works without them but falls back to `[best guess]` confidence.

**Skill doesn't load?** Check `~/.claude/skills/hotAsianIntern/SKILL.md` exists.

---

## Customization

Edit the reference files in `references/`. Each is self-contained — change Hana's forbidden phrase list, add escalation triggers for Mei, adjust Sora's source requirements. Add new interns by creating a reference file and adding a row to SKILL.md.

---

## Contributing

PRs welcome. Bug reports: open an issue with the prompt that misbehaved and what you expected.

## License

MIT. Built by [@RuneweaverStudios](https://github.com/RuneweaverStudios).

---
name: skills-map
description: Use at the START of any task, or whenever you are unsure which lich-skill applies. The router for this collection — it maps every lich-skill to the phase of work it belongs to (Decide → Spec → Plan → Build → Debug → Fan-out → Research → Media → Analyze) and walks a decision tree to pick the right one. Prevents the two failure modes of a skill collection: forgetting a skill exists, and reaching for the wrong skill at the wrong phase. Triggered by "/skills-map", "which skill should I use", "what skills do I have", or any moment of skill-selection doubt.
---

# Skills Map — the router for lich-skills

> Most skills help you *do* the work. This one helps you pick *which* skill does the work.

A skill collection has a discovery problem: the skills only fire if the agent
remembers they exist and reaches for the right one at the right moment. Left to
chance, the agent forgets `go-no-go` and starts building, or hand-rolls a debug
crawl instead of running `debug-hypothesis`. `skills-map` is the index + decision
tree that closes that gap.

It does three things:

1. **Identify** the phase of work the current task is in.
2. **Route** to the matching skill via the decision tree.
3. **Sequence** — most real work chains several skills; this gives the order.

## When to Use

- At the **start of any non-trivial task**, before picking up tools.
- Whenever you catch yourself thinking "is there a skill for this?"
- When a task spans multiple phases and you need the ordering.
- On `/skills-map`, "which skill", "what can you do", "list my skills".

## When NOT to Use

- The task is a one-line answer or a trivial edit — just do it.
- You already know exactly which skill applies — invoke it directly.
- You are mid-skill — finish it; don't re-route every step.

## The Map (by phase of work)

```
   DECIDE        SPEC / PLAN          BUILD          DEBUG         FAN-OUT
 ┌────────┐   ┌──────────────┐   ┌────────────┐  ┌────────────┐ ┌──────────────┐
 │go-no-go│──▶│spec-driven-  │──▶│build-until-│─▶│debug-      │ │subagent-     │
 │ Stage 0│   │dev / -v2     │   │pass        │  │hypothesis  │ │brief         │
 │ gate   │   │ Spec→…→Ship  │   │ red→green  │  │ O→H→E→C    │ │ ≤200w/agent  │
 └────────┘   └──────────────┘   └────────────┘  └────────────┘ └──────────────┘

   RESEARCH                       MEDIA            ANALYZE
 ┌──────────────┬──────────────┐ ┌────────────┐ ┌──────────────┐
 │tavily-search │wiki-aggregate│ │nano-banana │ │google-       │
 │ web fetch    │ N notes→pack │ │ text→image │ │analytics     │
 └──────────────┴──────────────┘ └────────────┘ └──────────────┘

   DESIGN
 ┌──────────────┐
 │frontend-     │
 │design        │
 └──────────────┘
```

## Decision Tree

Walk top to bottom; take the first branch that matches.

```
START
│
├─ About to start a NEW project / repo / contract?  ───────────▶  go-no-go   (Stage 0 — NO-GO is the default)
│
├─ Feature/refactor touching >1 file, needs a spec first?
│     ├─ hours–days, many files, multiple slices  ─────────────▶  spec-driven-dev-v2
│     └─ single feature, normal scope             ─────────────▶  spec-driven-dev
│
├─ A build / typecheck / lint / test is RED and you want green ─▶  build-until-pass
│
├─ Something is broken and you don't know WHY  ────────────────▶  debug-hypothesis
│
├─ About to spawn subagents / Task / Agent fan-out  ──────────▶  subagent-brief  (compress FIRST)
│
├─ Building a UI / page / component  ──────────────────────────▶  frontend-design
│
├─ Need facts / docs / citations from the web  ───────────────▶  tavily-search
│
├─ Have N≥3 raw research notes to lift into one pack  ─────────▶  wiki-aggregate
│
├─ Need an image generated or edited  ─────────────────────────▶  nano-banana
│
├─ Question about site traffic / GA4 / SEO  ──────────────────▶  google-analytics
│
└─ None match  ───────────────────────────────────────────────▶  no skill; proceed manually
```

## Quick Reference

| Phase | Skill | Use it when… |
|---|---|---|
| **Decide** | [`go-no-go`](../go-no-go/) | Before any spec/plan/code — should this project start at all? NO-GO is the default. |
| **Spec → Ship** | [`spec-driven-dev`](../spec-driven-dev/) | Any non-trivial feature/refactor touching >1 file. Gated Spec→Plan→Build→Test→Review→Ship. |
| **Long-horizon** | [`spec-driven-dev-v2`](../spec-driven-dev-v2/) | Agent works hours–days across many files/slices. Project→Sprint→Task hierarchy + review loop. |
| **Build** | [`build-until-pass`](../build-until-pass/) | A check is RED. Run → read error → smallest fix → re-run, until exit code 0 (cap 10). |
| **Debug** | [`debug-hypothesis`](../debug-hypothesis/) | Bug with unknown cause. Scientific method: Observe → Hypothesize → Experiment → Conclude. |
| **Fan-out** | [`subagent-brief`](../subagent-brief/) | Before spawning subagents. Compress each prompt to ≤200 words — no shared prefix cache. |
| **Design** | [`frontend-design`](../frontend-design/) | Building UI. Distinctive, production-grade interfaces that avoid generic AI-slop aesthetics. |
| **Research** | [`tavily-search`](../tavily-search/) | Real-time web search + content extraction via Tavily API. Facts, docs, citations. |
| **Research** | [`wiki-aggregate`](../wiki-aggregate/) | N≥3 raw notes → one structured pack with cross-source claims + `path:line` provenance. |
| **Media** | [`nano-banana`](../nano-banana/) | Text-to-image or image edit via Nano Banana 2. 512 / 1K / 2K / 4K. |
| **Analyze** | [`google-analytics`](../google-analytics/) | GA4 traffic analysis + out-of-the-box SEO daily report for one property. |

## Typical Sequences

Real work chains skills. The common pipelines:

```
New project          go-no-go → spec-driven-dev → build-until-pass → (bug?) debug-hypothesis
Long migration       go-no-go → spec-driven-dev-v2 → subagent-brief (per reviewer) → build-until-pass
Build a landing page spec-driven-dev → frontend-design → nano-banana (hero art) → build-until-pass
Research → report    tavily-search → wiki-aggregate
Ship + measure       …build-until-pass → google-analytics (post-launch SEO/traffic)
```

## Skill Rules

1. **Route before you reach.** When in doubt, run the decision tree before grabbing a tool — picking the wrong skill costs more than the 5 seconds of routing.
2. **`go-no-go` gates everything.** If the task is "start X", that branch wins over every build/spec branch below it. Do not skip Stage 0 because you're eager to build.
3. **One phase, one skill.** Don't stack skills for the same phase. Finish the active skill before re-routing.

## Anti-Rationalization

| Excuse | Reality |
|---|---|
| "I know the codebase, I'll just start coding." | That's the `go-no-go` / `spec-driven-dev` skip that produces the wrong thing fast. Route first. |
| "I'll debug it by reading around." | Unstructured crawling is the failure `debug-hypothesis` exists to prevent. Run the loop. |
| "I'll just spawn 10 subagents with full context." | One fan-out can burn a day of quota. `subagent-brief` first — always. |
| "No skill fits, I'll improvise." | Re-read the tree once. If it genuinely doesn't match, *then* proceed manually — and that's fine. |

## Maintenance

This map is hand-curated. When a skill is added to or removed from this
collection, update the phase map, decision tree, and quick-reference table here
in the same change. A router that lists skills that don't exist — or omits ones
that do — is worse than no router.

# skills-map

The router for the **lich-skills** collection. A meta-skill: instead of *doing*
work, it picks *which* skill does the work.

A skill collection has a discovery problem — the skills only fire if the agent
remembers they exist and reaches for the right one at the right moment.
`skills-map` is the index + decision tree that closes that gap. It maps every
lich-skill to its phase of work, then walks a decision tree to route the current
task to the right skill (and gives the ordering when a task chains several).

## When it fires

- At the start of any non-trivial task, before picking up tools
- On `/skills-map`, "which skill should I use", "what skills do I have"
- Whenever skill-selection is in doubt

## The phases

```
DECIDE → SPEC/PLAN → BUILD → DEBUG → FAN-OUT → RESEARCH → MEDIA → ANALYZE → DESIGN
```

| Phase | Skill |
|---|---|
| Decide | `go-no-go` |
| Spec → Ship | `spec-driven-dev` / `spec-driven-dev-v2` |
| Build | `build-until-pass` |
| Debug | `debug-hypothesis` |
| Fan-out | `subagent-brief` |
| Design | `frontend-design` |
| Research | `tavily-search`, `wiki-aggregate` |
| Media | `nano-banana` |
| Analyze | `google-analytics` |

See [`SKILL.md`](SKILL.md) for the full decision tree, typical pipelines, and
the anti-rationalization table.

## Maintenance

Hand-curated. When a skill is added to or removed from this collection, update
the map in `SKILL.md` in the same change.

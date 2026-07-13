# ideaverse 💡 — grill a raw idea into build-ready docs

A Claude Code skill that turns a raw product idea into a build-ready doc set by
**grilling you through 13 expert personas** (product, architect, security, finance, a
deliberately lazy staff engineer…) instead of agreeing with you. Output: one doc per
EPIC with locked decisions and API contracts, a story-by-story priority plan, an
`execute.md` self-driving build handoff, and a pre-build audit gate.

ideaverse began as a fork of [chitransh-cj/kimchi](https://github.com/chitransh-cj/kimchi)
(upstream `75d64f4`, MIT — all credit for the original concept and personas to Chitransh
Joshi) and upgrades how the research personas gather evidence.

## What this fork adds

Upstream's research personas (data-scientist, finance-bro) lean on generic web search.
This fork gives every persona three named research rails, defined in
`references/grilling-doctrine.md` §9–§10:

| Rail | Answers | How |
|---|---|---|
| **Nimble CLI** (§9) | market signals, competitors, growth proxies | `nimble search / extract / crawl`, streamed to disk — big pulls never flood the context window |
| **Reddit** (§10) | is the pain real? verbatim user complaints | Nimble on Reddit's unauthenticated `.json` endpoints; payload parsed with `json.loads`, never regex |
| **GitHub** (§10) | prior art — does an OSS project already do 80% of this? | authed `gh search repos / code / issues`; stars + recent pushes = live rival or dependency candidate |

Doctrine rules that come with the rails: results stream to disk (not into context),
**every number carries its source URL**, and an unsourced number must be labeled a
guess. No more fantasy TAMs built on vibes.

## Install

```bash
git clone https://github.com/justus20santiago-web/ideaverse ~/.claude/skills/ideaverse
```

Restart Claude Code, then `/ideaverse` (full lifecycle), `/ideaverse <persona>` (one expert),
`/ideaverse generate`, `/ideaverse audit`.

### Requirements

- [Nimble CLI](https://nimbleway.com) with `NIMBLE_API_KEY` set — powers the web and
  Reddit rails. Without it, personas fall back to plain web search (the doctrine says
  how).
- [`gh` CLI](https://cli.github.com), authenticated — powers the GitHub prior-art rail.

## License

MIT, retained from upstream (see `LICENSE`).

# hermes-token-saving-config

A [Hermes Agent](https://hermes-agent.nousresearch.com) skill that reduces token/cost usage through **native config levers** — no external proxy needed.

Works with any model. Tunes reasoning effort, context compression, prompt caching, and turn caps.

## Installation

```bash
# From this repo (recommended)
hermes skills install https://github.com/steggdev/hermes-token-saving-config

# Or install from local filesystem
hermes skills install ./skills/hermes-token-saving-config
```

## What it changes

| Config key | Value | Why it saves |
|---|---|---|
| `agent.reasoning_effort` | `medium` | Less chain-of-thought per turn (biggest lever) |
| `compression.threshold` | `0.6` | Compress conversation history sooner |
| `compression.target_ratio` | `0.15` | Compress harder per pass |
| `prompt_caching.cache_ttl` | `30m` | Longer cached-prefix reuse |
| `agent.max_turns` | `200` | Caps runaway loops |

See `SKILL.md` for full details, apply/restore commands, and pitfalls.

## Quick apply

```bash
hermes config set agent.reasoning_effort medium
hermes config set compression.threshold 0.6
hermes config set compression.target_ratio 0.15
hermes config set prompt_caching.cache_ttl 30m
hermes config set agent.max_turns 200
```

## License

MIT

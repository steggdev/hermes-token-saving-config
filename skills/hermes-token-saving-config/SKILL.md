---
name: hermes-token-saving-config
description: "Use when reducing Hermes token usage via config."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [hermes, tokens, config, optimization, cost, compression, caching, reasoning]
---

# Hermes Token-Saving Config

## When to Use
- User wants to reduce Hermes token/cost usage.
- Setting up a new machine or profile and applying token-saving defaults.
- Restoring token-saving config after a reset or config edit.

Tune the native Hermes config to cut token consumption, independent of model choice. These are built-in config levers — no external proxy needed.

## The levers (in order of impact)

| Key | Typical value | Why it saves |
|---|---|---|
| `agent.reasoning_effort` | `medium` (or `low`) | Less chain-of-thought emitted per turn. **Biggest lever** for reasoning-capable models. Default is `medium`; set `high` only when you need deep thinking. |
| `compression.threshold` | `0.6`–`0.7` | Compress conversation history **sooner** (at 60–70% of context instead of 50%). Default `0.5`. |
| `compression.target_ratio` | `0.15` | Compress harder per pass (keep only 15% tail). Default `0.20`. |
| `prompt_caching.cache_ttl` | `30m` (or longer) | Reuse a warm cached prefix across turns → fewer re-billed input tokens. |
| `agent.max_turns` | `200` | Caps runaway loops; each extra turn costs a full context read. |

## Apply

```bash
hermes config set agent.reasoning_effort medium
hermes config set compression.threshold 0.6
hermes config set compression.target_ratio 0.15
hermes config set prompt_caching.cache_ttl 30m
hermes config set agent.max_turns 200
```

## Restore defaults

```bash
hermes config set agent.reasoning_effort medium   # default is medium
hermes config set compression.threshold 0.5
hermes config set compression.target_ratio 0.20
hermes config set prompt_caching.cache_ttl 5m
hermes config set agent.max_turns 90
```

## Verify

```bash
grep -A2 "reasoning_effort\|threshold\|target_ratio\|cache_ttl\|max_turns" ~/.hermes/config.yaml
```

## Pitfalls

- **`agent.reasoning_effort` is NOT in the CLI autocomplete list** — `hermes config set agent.reasoning_effort medium` prints a warning ("not a recognized config key, did you mean agent.reasoning_overrides?"). **This is a false alarm.** The value is still saved, and the runtime reads it as the global fallback in `hermes_constants.py` (`parse_reasoning_effort`, ~line 1150). Use `--force` if you want to suppress the notice. Do NOT "fix" it to `agent.reasoning_overrides` for a global setting — that key is for **per-model** overrides.
- **Takes effect on a NEW session** (`/reset`), never mid-conversation — prompt caching is sacred; mutating past context mid-turn invalidates the cache and multiplies cost.
- Valid reasoning effort values: `none`, `minimal`, `low`, `medium`, `high`, `xhigh`, `max`, `ultra`. (See `batch_runner.py` valid_efforts.)
- These are global and permanent until changed. `deepseek-v4-flash` + medium effort = strong combo for routine work.

## Related

- RTK (`rtk init --agent hermes`) is a SEPARATE tool that compresses bash command output, not a Hermes config lever. Native config above targets reasoning + history, which is usually the bigger win for markdown/planning workflows.

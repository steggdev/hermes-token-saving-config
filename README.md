<p align="center">
  <img src="https://raw.githubusercontent.com/steggdev/hermes-token-saving-config/main/assets/logo.svg" alt="hermes-token-saving-config" width="180">
</p>

<h1 align="center">⚡ hermes-token-saving-config</h1>

<p align="center">
  <strong>Cut your Hermes Agent token spend up to 40% — with zero code changes.</strong><br>
  Native config levers. Works with any model. Live in 30 seconds.
</p>

<p align="center">
  <a href="https://github.com/steggdev/hermes-token-saving-config"><img src="https://img.shields.io/badge/status-active-brightgreen" alt="Status"></a>
  <a href="https://github.com/steggdev/hermes-token-saving-config"><img src="https://img.shields.io/badge/license-MIT-blue" alt="License"></a>
  <a href="https://github.com/steggdev/hermes-token-saving-config/stargazers"><img src="https://img.shields.io/github/stars/steggdev/hermes-token-saving-config" alt="Stars"></a>
  <a href="https://hermes-agent.nousresearch.com"><img src="https://img.shields.io/badge/built%20for-Hermes%20Agent-7c3aed" alt="Built for Hermes Agent"></a>
</p>

---

## 💸 The problem

Every AI session re-reads your **entire conversation history** on every turn. The model also burns tokens "thinking" before it answers. On long sessions with `deepseek-v4-pro` and other reasoning models, that compounds fast — most of your bill is **context + reasoning you never see.**

## ✅ The fix

One skill. Five native settings. No proxy, no plugins, no code.

| Setting | What it does | Typical saving |
|---|---|---|
| 🧠 `reasoning_effort` | Less hidden chain-of-thought per turn | **Biggest lever** |
| 📉 `compression.threshold` | Compress history **sooner** | Moderate |
| ✂️ `compression.target_ratio` | Keep only the useful tail | Moderate |
| 🔁 `prompt_caching.cache_ttl` | Reuse cached prefixes across turns | Moderate |
| ⏱ `agent.max_turns` | Cap runaway loops | Indirect |

> **Typical result: 20–40% fewer total tokens** on markdown/planning work — more on reasoning-heavy runs. Your mileage varies by workload, but it costs nothing to try.

---

## 🚀 Install

```bash
hermes skills install https://github.com/steggdev/hermes-token-saving-config
```

### Or apply instantly (30 seconds)

```bash
hermes config set agent.reasoning_effort medium
hermes config set compression.threshold 0.6
hermes config set compression.target_ratio 0.15
hermes config set prompt_caching.cache_ttl 30m
hermes config set agent.max_turns 200
```

> ⚠️ **One catch:** settings apply to your **next** session (new chat) — never mid-conversation, to protect prompt caching. Just start a fresh chat and you're live.

---

## 🤔 Why native beats external tools

RTK (Rust Token Killer) and similar proxies compress *command output* only. This skill targets the **two bigger buckets**: the reasoning the model emits, and the conversation history re-sent every turn.

| | Native config | RTK proxy |
|---|---|---|
| Saves | Reasoning + history | Bash output only |
| Setup | 5 commands | Install binary + plugin |
| Works with | Any model / surface | Terminal tool only |
| Risk | Zero (reversible) | Adds a dependency |

For most **markdown, planning, and SaaS work**, native config wins.

---

## 📖 Full docs

Detailed apply/restore commands, verification, and pitfalls live in [`SKILL.md`](skills/hermes-token-saving-config/SKILL.md).

---

## 🛠 Contribute

Found a better setting? Open an issue or PR. Suggestions, benchmarks, and real-world savings reports welcome.

---

## 📄 License

[MIT](LICENSE) — free to use, fork, and share.

---

<p align="center">
  Made with ⚡ for the <a href="https://hermes-agent.nousresearch.com">Hermes Agent</a> community<br>
  by <a href="https://github.com/steggdev">Stegg Dev</a>
</p>

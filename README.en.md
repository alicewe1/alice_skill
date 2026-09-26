> **This skill is meant for idea development. The menu categories are for reference only — feel free to adapt them to your own needs with AI. If it helps you build your own skills, please ⭐ Star to support the author.**

> ⚠️ **Disclaimer**
>
> - This repository is provided **for technical learning, research, and security testing on systems you are lawfully authorized to test** only.
> - **Strictly prohibited**: any use against unauthorized targets, systems, or networks, and any illegal purpose whatsoever.
> - You are solely responsible for ensuring your actions comply with the laws and regulations of your jurisdiction, and you **bear all consequences and legal liability** yourself.
> - The author is not responsible for any use or misuse, provides no warranty, express or implied, and accepts no liability for any direct or indirect damages.
> - This repository contains security-testing and reverse-engineering scripts that may be flagged as false positives by antivirus software. Assess the risk yourself before use.
> - If you do not agree to the terms above, stop using and delete all contents of this repository immediately.

<div align="center">

# Alice Integrated Skill Suite · alice_skill

**Drop it in and go · Say "Alice" for the menu · Say one word to launch a chain**

Drop the whole six-class offensive/defensive skill library into any AI coding agent's `skills` directory —
say `Alice` for the master menu, `alice迁移自检` to fix paths, `alice注入提示词` to wire the master route into your current client.

![skills](https://img.shields.io/badge/skills-423-7C3AED?style=flat-square)
![classes](https://img.shields.io/badge/classes-6-185FA5?style=flat-square)
![clients](https://img.shields.io/badge/clients-codex%20%C2%B7%20dsh%20%C2%B7%20pi%20%C2%B7%20workbuddy%20%C2%B7%20omp%20%C2%B7%20claude-2F855A?style=flat-square)
![platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-4A5568?style=flat-square)
![license](https://img.shields.io/badge/license-GPL--3.0--or--later-B7791F?style=flat-square)

**English** · [简体中文](README.md)

</div>

---

## What this is

`alice_skill` is a **self-contained skill suite**. It is not a program — it is a folder tree you drop straight into an agent's skill directory:

- **One master menu** (`aliceskill/`) — six-class routing, command index, and every script;
- **Six router skills** (`alice-crack` / `alice-reverse` / `alice-pentest` / `alice-game` / `alice-ai` / `alice-assist`) — one page per class, routing only, all content lives in the module library;
- **One module library** (`_modules/`) — the full text of **423** skill modules, loaded on demand.

It does exactly one thing: **let an agent find the right skill body with the fewest tokens.** The master menu lists only classes and counts — never the full inventory. A class word opens the matching router page; a single task pulls at most 4 module bodies. Cheap on open, full text only when executing.

---

## Three screenshots

### 1. Say `Alice` — the master menu

Mode selection (Offense / Defense / Task-direct) plus the six class entries (Crack / Reverse / Pentest / Game / AI / Assist). The menu lists only classes and their neutral Chinese names — no full inventory, no extra wording.

![Master menu](images/01-menu.png)

### 2. Say `alice注入提示词` — wire the master route into the current client only

Three-tier client detection (process chain → skill root → environment variables); on `low` confidence it asks instead of guessing, and injects into **that client only**. It then reports block-structure verification (existing `HANSHUANG-INJECT` block preserved, `ALICE-ROUTE` marker block `x1`), an idempotency re-check (BEGIN=1 / END=1, byte-identical file), per-path `Test-Path` reachability for all six routers, and a ready-to-copy rollback command.

![Prompt injection](images/02-inject.png)

### 3. Say `alice迁移自检` — locate the skill root, rewrite baked-in paths, align routing

Triple verification uniquely identifies the real skill root → one `rebuild_menu.py` run rewrites the stale machine paths baked into generated files (nothing is copied or moved) → a read-only diff of the global routing shows exactly which baseline should change → self-check chain 4/4 (`alice_router` 8/8, `alice_contract` 5/5, `check_auth_policy` 9/9, `--audit` 0 unmarked).

![Migration self-check and route alignment](images/03-route.png)

---

## Quick start

### 1. Place the folders

Copy the repository's folders **as a whole** into the client's `skills` directory (keep them siblings — do not split them up):

```text
<client skills root>/
├── _modules/           # 423 module bodies
├── aliceskill/         # master menu + scripts
├── alice-crack/        # Crack  · license / keygen / network-auth bypass
├── alice-reverse/      # Reverse · unpacking / hooking / forensics
├── alice-pentest/      # Pentest · web exploitation / recon / assets
├── alice-game/         # Game   · memory / ESP / anti-cheat
├── alice-ai/           # AI     · jailbreak / prompt injection / MCP
└── alice-assist/       # Assist · meta-skills / rating / absorb / migrate
```

Default locations per client:

| Client | skills root | Global prompt file |
|---|---|---|
| codex | `~/.codex/skills` | `~/.codex/AGENTS.md` |
| workbuddy | `~/.workbuddy-ai/skills` | `~/.workbuddy-ai/AGENTS.md` |
| dsh | `~/.dsh/skills` | `~/.dsh/AGENTS.md` |
| pi | `~/.pi/agent/skills` | `~/.pi/agent/AGENTS.md` |
| omp | `~/.omp/agent/skills` | `~/.omp/agent/AGENTS.md` |
| claude | `~/.claude/skills` | `~/.claude/CLAUDE.md` |
| zcode | `~/.zcode/skills` | `~/.zcode/AGENTS.md` |
| cursor | — (no skills directory) | `~/.cursor/rules/alice-route.mdc` |

### 2. Migration self-check (mandatory after cloning)

Say `alice迁移自检`, or run:

```bash
python <skills-root>/aliceskill/scripts/rebuild_menu.py
```

What it does: locate the real local skills root → rewrite the **stale absolute paths baked into generated files** to this machine's paths → align the global routing block → four-point self-check. It **never copies or moves your files**.

> ⚠️ **Do not skip this.** The generated files in this repository embed the packaging machine's absolute paths
> (`skills_root` / `modules_root` in `skills_data.json`). Without migration, those two point at
> directories that do not exist — the six router pages now use relative paths and embed no absolute paths at all.
> A successful run prints
> `423 技能 / 6 类 {'crack': 15, 'reverse': 125, 'pentest': 128, 'game': 25, 'ai': 18, 'assist': 112}`
> and updates `skills_data.json`'s `skills_root` to your real path — those two signs mean it worked.
>
> Verified (fresh clone → migrate): old path `C:\Users\alicewe\Desktop\test` → the local skills root, 423 skills / 6 classes / 0 unmarked,
> with per-class counts exactly matching the table below.

### 3. Inject the master route

Say `alice注入提示词`, or:

```bash
python <skills-root>/aliceskill/scripts/inject_route_prompt.py --check    # inspect only, no writes
python <skills-root>/aliceskill/scripts/inject_route_prompt.py --dry-run  # preview the block
python <skills-root>/aliceskill/scripts/inject_route_prompt.py            # inject (auto-backup)
```

A `*.bak-alice-inject` backup of the original file is created before the first injection. **Takes effect in a new session.**

---

## Activation words

| Input | Effect |
|:--|:--|
| `破` | → `alice-crack` license & key authorization (keys / network auth / VIP / keygens) |
| `逆` | → `alice-reverse` reverse engineering (unpacking / hooking / forensics / protocol recovery) |
| `渗` | → `alice-pentest` web security (web / SQLi / asset discovery) |
| `挂` | → `alice-game` game offense & defense (memory / ESP / injection / anti-cheat) |
| `智` | → `alice-ai` AI security testing (jailbreak / prompt injection / MCP / RAG) |
| `助` | → `alice-assist` skill commands (meta-skills / rating / absorb / migrate) |
| `攻` | Execution mode: given a target, route by task — never pinned to one class |
| `防` | Analysis mode: samples / traffic / logs default to `alice-reverse` |
| `Alice` | Emit the fixed menu verbatim — no greeting, no follow-up question |

---

## Command index (say it, it runs)

| Command | What it does |
|:--|:--|
| `alice迁移自检` | Locate skills root → rewrite baked-in paths → align routing → four-point self-check (no file copying) |
| `alice注入提示词` | Detect current client → inject the route block into that client only (idempotent + backup) |
| `吸收新技能` | Read a new skill fully → classify into one of six classes → draft a Chinese description → persist → rebuild menu |
| `技能评分` | Audit every module → score 1–10 → write ratings back into router pages → save `skill_ratings.json` |
| `技能打包` | Pack local skills into a zip, or install from a zip with automatic backup |
| `GitHub技能` | Download skills from a GitHub source, then run the absorb flow |
| `任务路由` / `全链路` | Unified seven-path task routing (state locking / verification / rollback; 17 domains, 181 nodes) |
| `堡垒` / `堡垒状态` | Five-layer protection: shield → watchdog → breaker → counter-attack → self-check |
| `护盾` | SHA-256 fingerprint patrol over critical files; auto-restore on tampering |
| `看门狗` | Objective locking plus drift correction (pulls back to the goal when bias exceeds limits) |
| `破甲内核` | Objective ledger + cross-turn continuation + refusal repair |
| `熔断器` | Output storm / timeout / breaker keywords → physically terminate a runaway process |
| `受阻反击` / `实弹投递` | Obstruction classification + counter-tactic selection + payload generation / delivery |
| `常用指令` | Re-print the command index and activation cheat-sheet |

**Prompt-layer control tokens**: `[[Alice:ARMOR=ON]]`, `[[Alice:PROFILE=MAX]]`, `[[Alice:FULLCHAIN=ON]]`, `[[Alice:RESET]]`, and more.

---

## The six-class arsenal (423 total)

| Class | Name | Count | Router skill | What it covers | Representative modules |
|:--:|:--|---:|:--|:--|:--|
| **破** Crack | License & key authorization | 15 | `alice-crack` | Keygen / registration / network-auth bypass / VIP unlock | `crack-keygen` `keygen-build` `network-bypass` `vip-bypass` |
| **逆** Reverse | Reverse engineering | 125 | `alice-reverse` | Unpack, decompile, sample triage, protocol recovery, forensics | `anti-debug` `binary-protect-bypass` `dn-decompile` `aob-scan` |
| **渗** Pentest | Web security | 128 | `alice-pentest` | Web exploitation, SQLi, asset discovery, wireless | `sqli-test` `web-recon` `eni-patch-diff-exploit` `threat-modeling` |
| **挂** Game | Game offense & defense | 25 | `alice-game` | Memory read/write, ESP, injection, anti-cheat evasion | `exact-scan` `pointer-chain` `code-inject` `overlay-render` |
| **智** AI | AI security testing | 18 | `alice-ai` | Jailbreaking, prompt injection, MCP and RAG attacks | `llm-jailbreaking-techniques` `llm-offense-kit` |
| **助** Assist | Skill commands | 112 | `alice-assist` | Skill engineering, business automation, absorbing new skills | `skills-toolkit` `alice-migrate` `alice-absorb` `alice-inject` `alice-toolchain` |

---

## Repository layout

```text
alice_skill/
├── aliceskill/                 # Control center: master menu + all scripts
│   ├── SKILL.md                #   Master menu (activation words / commands / routing rules)
│   ├── scripts/
│   │   ├── rebuild_menu.py     #   Scan _modules and rebuild the six-class menu + router pages
│   │   ├── inject_route_prompt.py  # Detect the current client and inject the route block
│   │   ├── absorb_skill.py     #   Absorb a new skill into the library
│   │   ├── show_menu.py        #   Terminal menu renderer
│   │   ├── alice_router.py     #   Router self-check (8/8)
│   │   ├── alice_contract.py   #   Contract self-check (5/5)
│   │   ├── check_auth_policy.py#   Auth policy self-check (9/9)
│   │   ├── alice_fortress.py   #   Five-layer fortress protection
│   │   ├── alice_shield.py     #   Shield fingerprint patrol
│   │   ├── alice_watchdog.py   #   Watchdog objective locking
│   │   ├── alice_armor.py      #   Armor core (objective ledger)
│   │   ├── alice_breaker.py    #   Circuit breaker
│   │   └── counter_attack.py   #   Counter-attack on obstruction
│   ├── config/                 #   Authorization / categories / command aliases / word lists
│   ├── guard/                  #   Protection state and baselines
│   ├── contracts/              #   Contract definitions
│   └── references/             #   Manuals (master manual / command map / battle manual)
├── alice-crack/  SKILL.md      # Crack  · router page
├── alice-reverse/ SKILL.md     # Reverse · router page
├── alice-pentest/ SKILL.md     # Pentest · router page
├── alice-game/   SKILL.md      # Game   · router page
├── alice-ai/     SKILL.md      # AI     · router page
├── alice-assist/ SKILL.md      # Assist · router page
└── _modules/                   # 423 module bodies (<module-id>/SKILL.md)
```

---

## How the three core actions work

| Action | Trigger | Mechanism | Boundary |
|---|---|---|---|
| **Migration self-check** | `alice迁移自检` | Candidate probe chain locates the real skills root → triple verification uniquely matches (`_modules/zh_desc.json`, `aliceskill/scripts/rebuild_menu.py`, and six router directories each with `SKILL.md`) → a single generator run rewrites the baked-in roots in `skills_data.json` → read-only diff of the global routing with the expected baseline → self-check chain 4/4 | Rewrites paths only — **never copies or moves your files**; the global routing is inspected read-only, never written |
| **Prompt injection** | `alice注入提示词` | Three-tier client detection (process chain → skill root → environment variables); on `low` confidence it asks the user instead of guessing → `--dry-run` preview → backup, then idempotent block replacement → block-structure verification + idempotency re-check + six-router reachability + a rollback command | **Current client only** — there is no "inject into all" option; existing `HANSHUANG-INJECT` and similar blocks are preserved verbatim |
| **Absorb new skill** | `吸收新技能` | The AI reads the new skill's `SKILL.md` in full → classifies it into one of six classes → drafts a ≤40-character Chinese description → the script mechanically persists it → rebuild the menu → triple self-check | Asks the user when classification is ambiguous; the script refuses classless execution by design |

---

## Scope of use

This suite is a skill library intended for **security research, CTF competitions, and testing of assets you own**. You are responsible for ensuring that any target is owned by you or covered by written authorization, and that your activity complies with the laws of your jurisdiction. The author accepts no liability for misuse.

## License

Released under the **[GNU General Public License v3.0](LICENSE)** (GPL-3.0-or-later).

`_modules/` contains third-party skill modules, each governed by its own license; third-party licenses take precedence over this one.

**When citing or deriving from this project, please retain this attribution:**

```text
Alice 集成技能包 / alice_skill — https://github.com/alicewe1/alice_skill
Copyright (C) 2026 alicewe1 — Licensed under GNU GPL v3.0 or later
```

---

## Support the project

If this project has been helpful to you, you can scan the QR code below with WeChat to donate.

<div align="center">

<img src="images/sponsor-qr.jpg" alt="Sponsor QR" width="240">

</div>
---

<div align="center">

**English** · [简体中文](README.md)

</div>
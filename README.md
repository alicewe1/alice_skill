> **本 skill 用于思路开发，菜单分类仅供参考，可按自身需求配合 AI 修改。如果对你制作 skill 有所帮助，请点 ⭐ Star 给作者支持。**

> ⚠️ **免责声明**
>
> - 本仓库仅供**技术学习、研究与已获合法授权**的安全测试使用。
> - **严禁**将其中任何内容用于未获授权的目标、系统或网络，严禁用于任何违法用途。
> - 使用者须自行确保其行为符合所在国家/地区法律法规，并**自行承担全部后果与法律责任**。
> - 作者不对任何使用或滥用行为负责，不提供任何明示或暗示的担保，亦不承担任何直接或间接损失。
> - 仓库内含安全测试与逆向相关脚本，可能被杀毒软件误报，请自行评估风险后使用。
> - 若你不同意上述条款，请立即停止使用并删除本仓库全部内容。

<div align="center">

# Alice 集成技能包 · alice_skill

**放进去就能用 · 说「Alice」出菜单 · 一句话进弹道**

把六类攻防技能库整包放进任意 Agent 客户端的 `skills` 目录 ——
说 `Alice` 出总菜单，说 `alice迁移自检` 修路径，说 `alice注入提示词` 把总路由接进当前客户端。

![skills](https://img.shields.io/badge/skills-423-7C3AED?style=flat-square)
![classes](https://img.shields.io/badge/classes-6-185FA5?style=flat-square)
![clients](https://img.shields.io/badge/clients-codex%20%C2%B7%20dsh%20%C2%B7%20pi%20%C2%B7%20workbuddy%20%C2%B7%20omp%20%C2%B7%20claude-2F855A?style=flat-square)
![platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-4A5568?style=flat-square)
![license](https://img.shields.io/badge/license-GPL--3.0--or--later-B7791F?style=flat-square)

[English](README.en.md) · **简体中文**

</div>

---

## 这是什么

`alice_skill` 是一套**自包含的技能集成包**。它不是一个程序，而是一棵可以直接丢进客户端技能目录的文件夹树：

- **一个总控菜单**（`aliceskill/`）—— 六类路由 + 常用指令 + 各类脚本；
- **六个路由技能**（`alice-crack` / `alice-reverse` / `alice-pentest` / `alice-game` / `alice-ai` / `alice-assist`）—— 每类一页，只做路由，正文全部在模块库；
- **一个模块库**（`_modules/`）—— **423** 个技能模块的正文，按需取用。

设计上只做一件事：**让 Agent 用最少 token 找到正确的技能正文**。总菜单只给类目和数量，不列全量清单；命中类词后进路由页选模块；一条任务最多取 4 个模块正文。开场省 token，执行才读正文。

---

## 三张图看懂

### 1. 说 `Alice` —— 出总菜单

模式选择（攻 / 防 / 直接给任务）+ 六类入口（破 / 逆 / 渗 / 挂 / 智 / 助）。菜单只给类目和中文名，不含全量清单，措辞更中性、更简洁。

![总菜单](images/01-menu.png)

### 2. 说 `alice注入提示词` —— 只把总路由接进当前客户端

三级判定客户端（进程链 → 技能根 → 环境变量），置信 `low` 就问不猜，只注入**当前这一个**。完成后给出块结构核对（`HANSHUANG-INJECT` 完整保留、`ALICE-ROUTE` 标记块 `x1`）、幂等复验（复跑后 BEGIN=1 / END=1、文件字节不变）、六条路由逐条 `Test-Path` 可达性，以及一条可直接复制的回滚命令。

![提示词注入](images/02-inject.png)

### 3. 说 `alice迁移自检` —— 定位技能根、刷内嵌路径、对齐路由

三项判定唯一命中真实技能根 → 只跑一次 `rebuild_menu.py` 刷掉内嵌的旧机路径（不拷贝、不移动任何文件）→ 只读检查全局路由差异，把该改的基线摆给你看 → 自检链 4/4（`alice_router` 8/8、`alice_contract` 5/5、`check_auth_policy` 9/9、`--audit` 未标记 0）。

![迁移自检与路由对齐](images/03-route.png)

---

## 快速开始

### 1. 放置

把仓库里的文件夹**整包**放进客户端的 `skills` 目录（保持同级结构，不要拆散）：

```text
<客户端 skills 根>/
├── _modules/           # 423 个模块正文
├── aliceskill/         # 总菜单 + 脚本
├── alice-crack/        # 破 · 卡密授权
├── alice-reverse/      # 逆 · 逆向分析
├── alice-pentest/      # 渗 · web安全
├── alice-game/         # 挂 · 游戏攻防
├── alice-ai/           # 智 · AI安全测试
└── alice-assist/       # 助 · 技能指令
```

各客户端默认位置：

| 客户端 | skills 根 | 全局提示词 |
|---|---|---|
| codex | `~/.codex/skills` | `~/.codex/AGENTS.md` |
| workbuddy | `~/.workbuddy-ai/skills` | `~/.workbuddy-ai/AGENTS.md` |
| dsh | `~/.dsh/skills` | `~/.dsh/AGENTS.md` |
| pi | `~/.pi/agent/skills` | `~/.pi/agent/AGENTS.md` |
| omp | `~/.omp/agent/skills` | `~/.omp/agent/AGENTS.md` |
| claude | `~/.claude/skills` | `~/.claude/CLAUDE.md` |
| zcode | `~/.zcode/skills` | `~/.zcode/AGENTS.md` |
| cursor | —（无 skills 目录） | `~/.cursor/rules/alice-route.mdc` |

### 2. 迁移自检（从 GitHub 克隆后 **必做**）

说 `alice迁移自检`，或直接跑：

```bash
python <skills根>/aliceskill/scripts/rebuild_menu.py
```

作用：定位本机真实 skills 根 → 把生成物里内嵌的**旧机绝对路径**刷成本机路径 → 对齐全局路由指引 → 四点自检。**不拷贝、不搬动你的文件**。

> ⚠️ **别跳过这一步。** 仓库里的生成物内嵌的是打包机的绝对路径（`skills_data.json` 的 `skills_root` / `modules_root`），
> 克隆后不跑迁移，这两项会指向不存在的目录（六个路由页已改为相对路径，不再内嵌绝对路径）。
> 跑完会输出 `423 技能 / 6 类 {'crack': 15, 'reverse': 125, 'pentest': 128, 'game': 25, 'ai': 18, 'assist': 112}`，
> 并确认 `skills_data.json` 的 `skills_root` 已变为你的真实路径 —— 看到这两项即为成功。
>
> 实测（全新克隆 → 迁移）：旧路径 `C:\Users\alicewe\Desktop\test` → 本机 skills 根，423 技能 / 6 类 / 未标记 0，六类数量与下表完全一致。

### 3. 注入总路由

说 `alice注入提示词`，或：

```bash
python <skills根>/aliceskill/scripts/inject_route_prompt.py --check    # 只看不写
python <skills根>/aliceskill/scripts/inject_route_prompt.py --dry-run  # 预览注入块
python <skills根>/aliceskill/scripts/inject_route_prompt.py            # 执行（自动备份）
```

首次注入前会生成 `*.bak-alice-inject` 原件备份。**新会话生效。**

---

## 激活词

| 输入 | 效果 |
|:--|:--|
| `破` | → `alice-crack` 卡密授权（卡密 / 网络验证 / VIP / 注册机） |
| `逆` | → `alice-reverse` 逆向分析（脱壳 / Hook / 取证 / 协议还原） |
| `渗` | → `alice-pentest` web安全（Web 挖洞 / SQL 注入 / 资产） |
| `挂` | → `alice-game` 游戏攻防（内存 / ESP / 注入 / 反作弊） |
| `智` | → `alice-ai` AI安全测试（越狱 / 提示注入 / MCP / RAG） |
| `助` | → `alice-assist` 技能指令（元技能 / 评分 / 吸收 / 迁移） |
| `攻` | 执行模式：给出目标，按任务路由（不把模式词固定当成某一类） |
| `防` | 分析模式：样本 / 流量 / 日志默认优先 `alice-reverse` |
| `Alice` | 严格输出固定菜单正文，不加问候与追问 |

---

## 常用指令（直接说即执行）

| 指令 | 作用 |
|:--|:--|
| `alice迁移自检` | 定位 skills 根 → 刷内嵌路径 → 对齐路由指引 → 四点自检（不拷贝文件） |
| `alice注入提示词` | 检测当前客户端 → 只把总路由块注入该客户端（幂等 + 备份） |
| `吸收新技能` | AI 读完新技能正文 → 判六类 → 拟中文描述 → 落库 → 重建菜单 |
| `技能评分` | 逐模块体检 → 综合评 1–10 → 实时写路由页 → 落盘 `skill_ratings.json` |
| `技能打包` | 打包本机技能为 zip，或从 zip 一键安装（自动备份） |
| `GitHub技能` | 从 GitHub 选源下载技能 → 再走吸收流程落库 |
| `任务路由` / `全链路` | 七路任务路由一体化（状态锁存 / 验证 / 回滚，17 域 181 节点） |
| `堡垒` / `堡垒状态` | 五层保护：护盾 → 看门狗 → 熔断 → 反击 → 自检 |
| `护盾` | 关键文件 SHA-256 指纹巡查，发现篡改自动恢复 |
| `看门狗` | 目标锁存 + 偏离矫正（bias 越限自动拉回目标） |
| `破甲内核` | 目标账本 + 跨轮续作 + 拒答修复 |
| `熔断器` | 输出风暴 / 超时 / 熔断词 → 物理终止失控进程 |
| `受阻反击` / `实弹投递` | 阻碍判定 + 反击战术选择 + 载荷生成 / 投递 |
| `常用指令` | 重看指令表与激活词速查 |

**提示词层控制命令**：`[[Alice:ARMOR=ON]]`、`[[Alice:PROFILE=MAX]]`、`[[Alice:FULLCHAIN=ON]]`、`[[Alice:RESET]]` 等。

---

## 六类弹药库（共 423）

| 类词 | 中文名 | 数量 | 路由技能 | 主要是能做什么 | 代表模块 |
|:--:|:--|---:|:--|:--|:--|
| **破** | 卡密授权 | 15 | `alice-crack` | 算卡密 / 写注册机 / 去网络验证 / 解 VIP | `crack-keygen` `keygen-build` `network-bypass` `vip-bypass` |
| **逆** | 逆向分析 | 125 | `alice-reverse` | 脱壳反编译 / 抓样本 / 还原协议 / 取证溯源 | `anti-debug` `binary-protect-bypass` `dn-decompile` `aob-scan` |
| **渗** | web安全 | 128 | `alice-pentest` | Web 挖洞注入 / 摸资产 / 拿权限 / 打无线 | `sqli-test` `web-recon` `eni-patch-diff-exploit` `threat-modeling` |
| **挂** | 游戏攻防 | 25 | `alice-game` | 读写内存 / ESP 注入 / 对抗反作弊 | `exact-scan` `pointer-chain` `code-inject` `overlay-render` |
| **智** | AI安全测试 | 18 | `alice-ai` | 越狱破限 / 提示注入 / 打 MCP 与 RAG | `llm-jailbreaking-techniques` `llm-offense-kit` |
| **助** | 技能指令 | 112 | `alice-assist` | 管技能 / 业务自动化 / 吸收新技能 | `skills-toolkit` `alice-migrate` `alice-absorb` `alice-inject` `alice-toolchain` |

---

## 目录结构

```text
alice_skill/
├── aliceskill/                 # 总控：总菜单 + 全部脚本
│   ├── SKILL.md                #   总菜单（激活词 / 常用指令 / 路由规则）
│   ├── scripts/
│   │   ├── rebuild_menu.py     #   扫描 _modules 重建六类菜单与路由页
│   │   ├── inject_route_prompt.py  # 检测当前客户端并注入总路由块
│   │   ├── absorb_skill.py     #   吸收新技能落库
│   │   ├── show_menu.py        #   终端菜单渲染
│   │   ├── alice_router.py     #   路由自检（8/8）
│   │   ├── alice_contract.py   #   契约自检（5/5）
│   │   ├── check_auth_policy.py#   授权策略自检（9/9）
│   │   ├── alice_fortress.py   #   堡垒五层保护
│   │   ├── alice_shield.py     #   护盾指纹巡查
│   │   ├── alice_watchdog.py   #   看门狗目标锁存
│   │   ├── alice_armor.py      #   破甲内核（目标账本）
│   │   ├── alice_breaker.py    #   熔断器
│   │   └── counter_attack.py   #   受阻反击
│   ├── config/                 #   授权 / 类目 / 命令别名 / 词表
│   ├── guard/                  #   保护状态与基线
│   ├── contracts/              #   契约定义
│   └── references/             #   手册（总纲 / 命令图 / 作战手册）
├── alice-crack/  SKILL.md      # 破 · 路由页
├── alice-reverse/ SKILL.md     # 逆 · 路由页
├── alice-pentest/ SKILL.md     # 渗 · 路由页
├── alice-game/   SKILL.md      # 挂 · 路由页
├── alice-ai/     SKILL.md      # 智 · 路由页
├── alice-assist/ SKILL.md      # 助 · 路由页
└── _modules/                   # 423 个模块正文（<模块id>/SKILL.md）
```

---

## 三个核心动作的原理

| 动作 | 输入 | 机制 | 边界 |
|---|---|---|---|
| **迁移自检** | `alice迁移自检` | 候选探测链定位真实 skills 根 → 三项判定唯一命中（`_modules/zh_desc.json`、`aliceskill/scripts/rebuild_menu.py`、六个路由目录各有 `SKILL.md`）→ 只跑一次生成器刷 `skills_data.json` 的内嵌根 → 只读检查全局路由差异并给出基线 → 自检链 4/4 | 只刷路径，**不拷贝不移动用户文件**；全局路由只读、不写入 |
| **提示词注入** | `alice注入提示词` | 进程链 → 技能根 → 环境变量三级判定客户端，置信 `low` 就问用户不猜 → `--dry-run` 预览 → 备份后幂等替换标记块 → 块结构核对 + 幂等复验 + 六路由可达性校验 + 输出回滚命令 | **只注入当前客户端**，无「全部注入」选项；既有 `HANSHUANG-INJECT` 等标记块完整保留 |
| **吸收新技能** | `吸收新技能` | AI 先完整读新技能 `SKILL.md` → 按六类定义判类 → 拟 ≤40 字中文描述 → 脚本机械落库 → 重建菜单 → 三自检 | 判类拿不准就问用户；脚本拒绝无类目执行（设计行为） |

---

## 使用边界

本包为**安全研究、CTF 竞赛与自有资产测试**用途的技能库。使用时请自行确保：目标为你拥有或已获书面授权的资产，且行为符合所在地法律法规。作者不对任何滥用行为负责。

## 许可

本项目采用 **[GNU 通用公共许可协议第 3 版](LICENSE)**（GPL-3.0-or-later）许可。

`_modules/` 内含第三方技能模块，各自受其自身许可约束，第三方许可优先于本许可。

**引用本项目或做衍生作品时，请保留以下署名：**

```text
Alice 集成技能包 / alice_skill — https://github.com/alicewe1/alice_skill
Copyright (C) 2026 alicewe1 — Licensed under GNU GPL v3.0 or later
```

---

## 感谢捐赠

这套技能库从路由设计、模块编写到每一轮真机校验，都是一个人在做。如果它帮你少走了弯路，或者你希望它继续更新下去，可以扫下面的码支持一下。

<div align="center">

<img src="images/sponsor-qr.jpg" alt="赞赏码" width="240">

**扫码请我喝杯咖啡**

</div>

不留也没关系 —— 提 issue、反馈问题、把项目分享给用得上的人，同样是很有价值的支持。

---

<div align="center">

[English](README.en.md) · **简体中文**

</div>
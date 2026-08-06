# hermes-skill-gen-skill

Hermes Agent 技能生成构成规范（skill）—— 新 skill 的三件事分层、文件层级、参考分层（A 模式）、生成流程与打包整理规范。

生成新技能前加载本技能，AI 按构成规范设计文件层级与文件约束，收敛决策（能合并不新建），产出符合规范的 skill。仓库根目录即 skill 本体，用一键脚本或手动复制安装。

## 安装

本 skill 支持多 agent 目标：Hermes / Claude Code / Codex。安装脚本自动探测本机已安装的 agent，让用户选择安装目标。

```bash
# 方式 1：交互选择安装目标（推荐，先下载再执行以保留交互）
curl -fsSL https://gitee.com/GreatBigM/hermes-skill-gen-skill/raw/main/install.sh -o /tmp/install.sh && bash /tmp/install.sh

# 方式 2：指定目标（非交互）
curl -fsSL https://gitee.com/GreatBigM/hermes-skill-gen-skill/raw/main/install.sh | bash -s -- --target hermes,claude

# 方式 3：安装到全部检测到的 agent
curl -fsSL https://gitee.com/GreatBigM/hermes-skill-gen-skill/raw/main/install.sh | bash -s -- --all
```

> 脚本等价于手动复制（clone + cp），不经过安全扫描，可先审阅脚本内容再执行。
> 已安装时自动备份旧版本到 `<skill_dir>.bak.<时间戳>`，重跑即升级（含版本对比提示）。

## 安装（备选：手动复制）

> ⚠️ 注意：`hermes skills install`（tap/URL 方式）对 hermes-skill-gen 可能触发安全扫描拦截（
> 技能涉及「管理其他 skill 构成/脱敏」类命令），community 来源 + dangerous 判定不可用 --force 绕过。
> **推荐使用一键脚本或手动复制安装，不经过扫描。**

```bash
# 1. 克隆本仓库
git clone https://gitee.com/GreatBigM/hermes-skill-gen-skill.git

# 2. 复制到 Hermes 的 skills 目录（不经过安全扫描）
mkdir -p ~/.hermes/skills/hermes-skill-gen
cp hermes-skill-gen-skill/SKILL.md ~/.hermes/skills/hermes-skill-gen/
cp -r hermes-skill-gen-skill/templates ~/.hermes/skills/hermes-skill-gen/

# 3. 会话内 /reload-skills，或新开会话自动加载
```

## 快速上手

```bash
# 用户: 生成一个 skill / 新建技能
# AI:  加载本技能 → 收敛决策（量化重叠，能合并不新建）
#      → 按构成规范设计（概念分层/目录层级/文件约束/打包整理）→ 与用户确认 → 创建
```

**铁律：用户给出意图 → AI 按规范生成，不反问"你要不要先看规范"。**

## 核心概念

**一个 skill 的三件事**（不混装）：

| 概念 | 问题 | 归属 |
|------|------|------|
| SKILL.md | 怎么做（操作流程） | skill 根，唯一必需 |
| 资产侧宪法 | 做成什么样（目标态） | skill 根目录（唯一真相源，随 skill 分发）；资产内不放置实例 |
| CHANGELOG / HISTORY | 发生过什么（版本/档案） | CHANGELOG 在 skill，HISTORY 在资产 |

**文件层级**：SKILL.md（唯一入口）+ 可选 SCHEMA.md（宪法）/ templates/（可替换参考实现）/ references/（坑+决策原因）/ scripts/（可执行脚本）/ assets/。

**生成判据**：每个支持文件回答「支撑谁 / 何时查」，回答不上不建；支持文件必须被 SKILL.md 反引号引用（否则安装漏装）。

## 仓库结构

```
hermes-skill-gen-skill/
├── README.md                        ← 本文件
├── install.sh                       ← 一键安装脚本
├── SKILL.md                         ← 操作手册（触发条件/文件层级/生成流程，仓库根即 skill）
├── CHANGELOG.md                     ← 版本历史
└── templates/
    └── skill-template.md            ← 新 skill 生成蓝图（九节，含 SCHEMA 七节框架）
```

## 设计理念

- **收敛优先**：新 skill 前先量化重叠（wc -l + grep -c），能合并不新建
- **单一真相源**：version 在 SKILL.md frontmatter；支持文件清单在 SKILL.md 末尾；一个概念一个文件定义
- **参考分层（A 模式）**：坑与决策原因全量进 references/，SKILL.md 只留干净主流程
- **宪法唯一真相源在 skill 根目录**：SCHEMA.md 与 SKILL.md 物理平级，资产内不放置宪法实例
- **AI 使用哲学**：用户指挥 AI，AI 替用户执行——给意图就干不反问

## License

MIT

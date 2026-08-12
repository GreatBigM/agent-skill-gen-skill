---
name: hermes-skill-gen
description: 生成新 skill 前加载，按构成规范设计文件层级与文件约束。
version: 1.5.0
author: Hermes Agent
license: MIT
category: autonomous-ai-agents
metadata:
  hermes:
    tags: [hermes, skills, generation, architecture]
    triggers: [生成skill, skill构成, skill架构, 新建skill]
---

# SKILL 生成构成规范（hermes-skill-gen）

## ⚡ 使用方式（AI 替你完成）

```
用户: 生成一个 skill / 新建技能
AI:  加载本 skill → 收敛决策 → 按构成规范设计（概念分层/目录层级/文件约束/打包整理）→ 与用户确认 → 创建
```

**铁律：用户给出意图 → AI 按规范生成，不反问"你要不要先看规范"。**

## 触发

- 创建新 skill
- 重构已有 skill 的构成
- 收敛决策：新 skill 前先量化重叠，能合并不新建

## 概念分层（一个 skill 的三件事）

| 概念 | 问题 | 归属 |
|------|------|------|
| SKILL.md | 怎么做（操作流程） | skill 根，唯一必需 |
| 资产侧宪法 | 做成什么样（目标态） | skill 根目录（唯一真相源，随 skill 分发）；资产内不放置实例 |
| CHANGELOG / HISTORY | 发生过什么（版本/档案） | CHANGELOG 在 skill，HISTORY 在资产 |

平级互补：流程实现目标态。三件事三个文件，不混装。

## 文件层级目录

```
<skill>/
├── SKILL.md          ← 怎么做（操作流程）——唯一必需，唯一入口
├── SCHEMA.md         ← 做成什么样（可选）：与 SKILL.md 概念平级的宪法/蓝图——物理并列，init 时实例化到被管理资产
├── CHANGELOG.md      ← 发生过什么（版本历史，随安装拷贝）
├── templates/        ← 生成骨架（准入标准 = 可替换性：可替换的参考实现；不可替换的约定内容不放这里）；新 skill 骨架蓝图见 `templates/skill-template.md`
├── references/       ← 参考层：坑（遇问题排障）+ 决策原因（为什么这样）+ 接入参考——SKILL.md 保持干净主流程，坑/决策不占主文档（A 模式）
├── scripts/          ← 可执行脚本（isatty 双通道：终端直连 vs AI 调用）
└── assets/           ← 资源（可选）
```

## 参考分层（A 模式，2026-08-03 定稿）

SKILL.md 只承载主流程（怎么做），保持干净——坑与决策原因不占主文档：

- **坑（排障经验）全量进 references/**：遇问题按需查；高频坑也不例外——分层一致性 > 少翻一层
- **决策原因（为什么这样）全量进 references/**：理解需要时查；CHANGELOG 只承载版本记录，不兼任决策档案
- **references 是参考层不是档案层**：服务未来查询（遇到问题/需要理解时），记录过去归 CHANGELOG
- **发布边界**：可泛化的坑/决策原因随包发布；项目特定调试笔记/单次分析不发布（无对外价值 + 敏感重灾区）
- **生成判据**：每个支持文件回答「支撑谁 / 何时查」，回答不上不建

## 文件间约束

1. **SKILL.md 唯一入口**：所有支持文件必须被它反引号引用（否则 hermes skills install 漏装）
2. **支持文件清单 = 单一真相源**：SKILL.md 末尾清单列出全部支持文件，`_referenced_support_paths` 校验无缺失无多余
3. **version 单一真相源**：SKILL.md frontmatter，CHANGELOG/install.sh 都读它
4. **概念单一真相源**：一个概念一个文件定义，其余引用——禁止两处定义
5. **宪法唯一真相源在 skill 根目录**：SCHEMA.md 与 SKILL.md 物理平级（概念平级必须物理平级——qwiki v1.9.6 教训：宪法曾进 templates/ 表达从属，文字声明覆盖不了物理错位）；**资产内不放置宪法实例/copy**（qwiki v2.0.0 教训：实例 = 第三层 copy，双头维护漂移实锤——实例改动与 skill 快照漏同步）；init 不生成实例，宪法变更 = 改 skill 侧 + bump 版本 + 发布
6. **模板约束流程不约束细节**（模板定位哲学）；模板不做组合（基板模板=死文件），继承靠内嵌+引用
7. **命名**：name 动词-名词；选词查占用（词义契合 + 是否被其他用途占用 + 历史延续）；概念词按可变性分工——SCHEMA=不可更改的（宪法/蓝图），SPEC=可更改的（工程约束/当前规格），不混用
8. **AI 使用哲学头节必填**：SKILL.md 顶部声明「用户指挥 AI，AI 替用户执行」+ 铁律 + AI 交互约定
9. **references = 参考层（A 模式）**：坑 + 决策原因全量进 references，SKILL.md 不承载；调试笔记/单次分析不发布（项目特定、敏感重灾区）；references 引用要清理（保知识去链接）
10. **description ≤ 100 字**：frontmatter description 一句话说清「这是什么 skill + 干什么用」（≤100 字，市场卡片展示规格 + 技能索引 57 字符截断后仍可读）；细节进正文/triggers，不进 description（2026-08-12 市场卡片规格沉淀）

## 生成流程

1. **收敛决策**：量化重叠（`wc -l */SKILL.md` + `grep -c <核心命令>`），子集技能合并/删除（能合并不新建；删 A 前提取其独有知识章节并入主技能，frontmatter triggers 合并）
2. **定义三件事**：怎么做（SKILL.md）+ 做成什么样（放被管理资产，skill 侧 init 快照放根目录平级）+ 发生过什么（CHANGELOG）
3. **命名**：name 动词-名词，查占用
4. **模板**：init 类（进 init 流程）+ 生成类（产内容）
5. **references**：只放必要深度参考，标清定位（使用参考 vs 下级接入参考）
6. **支持文件清单**：反引号引用全部支持文件
7. **版本化**：version 单一真相源 + CHANGELOG
8. **打包整理**（对外分发的 skill）：脱敏 + 脚本规范 + install.sh（见下）

## 打包整理规范（2026-08-01 实践沉淀，原 packaging 并入）

### 脱敏（对外生成的 skill）

- 文档描述可泛化；**运行必需保留原值或改可配置+注释**（脚本提示符匹配列表等——批量替换会破坏功能）
- 脚本名无项目名（`device-reboot-stress.py` → `reboot-stress.py`，先确认无引用）
- **进程名/日志路径是隐藏泄露源**：killall/grep 里的 `app_main`/`media_client` 等——保留功能，docstring 加"按目标设备实际进程名修改"注释
- 脱敏后**全仓重扫**：同款标识符三处分布（SKILL.md 模式表 / 脚本 MARKERS / references 代码示例），改完脚本 SKILL.md 常漏
- 内部痕迹扫描维度：内部工具/方法论名、昵称、公司域名邮箱、内部 IP、绝对路径
- 全部脚本 `python3 -m py_compile` 验证

### 脚本规范

- **isatty 双通道**：真实终端 → 向导交互；非 TTY（AI/管道）→ 提示参数路径退出，不傻等 stdin
- 向导先收集后写盘、取消零副作用；`_ask(prompt, default, validator, hint)` 帮助函数（回车默认/q 取消/校验重输）
- agent 交互约定写入 SKILL.md：对话层问参数，`--xxx` 传入，不用管道喂 stdin
- 测试双向：管道（非 TTY 降级）+ `script -qec` 分配 pty（向导全交互）
- 脚本必须被 SKILL.md 反引号引用（否则 skills install 漏装）

### install.sh 规范（对外分发）

- **版本化三件套**：version 单一真相源 + CHANGELOG.md（随安装拷贝）+ 安装/更新一体（对比版本输出 升级/最新/覆盖 三提示 + 旧版 mv 为 .bak.时间戳）
- **get_version 必须 `|| true` 容错**：旧版无 version 字段时 `grep -m1 '^version:'` 非零，set -e 下整个脚本静默退出（2026-08-01 三仓踩坑）
- **set -e 下条件 append 必须 if 不能 `[ ] &&`**：短路返回非零直接退出
- 多技能 `SKILLS=(...)` 数组 + 单技能参数：`curl ... | bash -s -- <skill>`（`-s` 会被 bash 当选项吃掉，管道才是对的）
- 安装源 gitee 主推 + GitHub 镜像；gitee 推送必须 SSH（HTTPS 无凭证 fatal）
- 隔离 HOME 实测：安装 + 幂等备份 + `hermes skills list` 识别 + 版本升级路径（造旧版模拟）

## 相关

- `hermes-skill-review` — 审查环节（构成审查 + 发布前审查）

> 框架：生成（本技能）→ 审查 → 发布（三环节技能各自独立分发，按需获取）

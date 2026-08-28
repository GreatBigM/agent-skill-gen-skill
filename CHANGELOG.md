# Changelog

本文件记录版本历史。版本号定义在 SKILL.md frontmatter 的 `version` 字段（单一真相源）。

## 1.6.0 (2026-08-18)

### Changed（多 agent 化 + 更名）
- **技能更名 `hermes-skill-gen` → `agent-skill-gen`**：SKILL.md name / 仓库名 / 目录名 / README / install.sh 全面同步（`SKILL_NAME=agent-skill-gen`，安装到 `<agent>/skills/agent-skill-gen`）
- **不再特指 Hermes**：frontmatter `metadata.hermes` → `metadata.agent`；author 改为 GreatBigM；正文「hermes skills install / hermes skills list」泛化为「部分 agent 的 skills install / `<agent> skills list`」；配套引用更新为 agent-skill-review
- **ZCode 安装目标**：install.sh 支持 ZCode（探测 `~/.zcode` → 安装到 `~/.zcode/skills/agent-skill-gen`），README 补 `--target zcode` 示例

## 1.5.0 (2026-08-12)

### Added（description 字数约束）
- **文件间约束 #10：description ≤ 100 字**——frontmatter description 一句话说清「这是什么 skill + 干什么用」；细节进正文/triggers，不进 description（市场卡片展示规格 + 技能索引 57 字符截断后仍可读；2026-08-12 市场卡片规格沉淀，7 仓已全部达标）

## 1.4.0 (2026-08-03)

### Changed（SKILL 设计三原则 + 首次对外发布快照）
- **SKILL 设计三原则入稿**：①SKILL 不知道 SKILL——skill 自包含独立分发，不提名其他 skill；②宪法唯一真相源在 skill 根目录，资产内不放置实例/copy；③参考分层 A 模式——坑与决策原因全量进 references/，SKILL.md 只留干净主流程
- **AI 使用哲学头节**：顶部声明「用户指挥 AI，AI 替用户执行」+ 铁律（给出意图就干不反问）+ AI 交互约定
- **脱敏**（对外发布版）：示例词泛化（脚本名/进程名/内部工具名）、相关段清理未发布 skill 引用（保知识去链接）

## 1.3.0 (2026-08)

### Changed（构成规范 A 模式）
- 命名两维度：身份定位 + 同族区分度（goal→spec 重叠教训：spec 承接规格 / design 承接方案）
- templates/ 准入 = 可替换性：可替换的参考实现；不可替换的约定内容不进 templates
- references = 参考层非档案层：可泛化的坑/决策原因随包发布；项目特定调试笔记不发布
- 附 `templates/skill-template.md` 新 skill 蓝图（九节，含 SCHEMA 七节框架：定位/目录结构/命名约定/内容纯度/文档公约/演进规则/迁移规则）

## 1.0.0

### Added
- 初始创建（内部使用，构成规范逐步成型）

## 1.6.1 (2026-08-28)

### Added

- install.sh 新增 pi 安装目标（探测 `~/.pi/agent` → 安装到 `~/.pi/agent/skills/<skill>`，pi 自动发现）
- 版本 1.6.0 → 1.6.1

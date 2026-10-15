# Changelog

## 1.6.7 (2026-10-15)

### Fixed（本仓自洽：模板 ↔ SKILL ↔ README ↔ 闸门注释）

- **模板「物化位置」栏与真相源口径互斥**（`templates/skill-template.md:12`）：原写「被管理资产内（真相源）；skill 侧 init 快照放根目录平级」，与同文件 `:32`/`:44` 及 `SKILL.md`/`README.md` 的「唯一真相源随 skill 分发、资产内不放置实例」冲突 ⇒ 统一为后者。同病两处在 `SKILL.md` 一并收敛：目录结构 `SCHEMA.md` 行原写「init 时实例化到被管理资产」（与文件间约束 #5「init 不生成实例」矛盾）、生成流程第 2 步原写「放被管理资产，skill 侧 init 快照放根目录平级」。
- **CHANGELOG 例示自相矛盾**（1.6.5 条目 ③）：「中文紧贴即漏检」拿 `<项目A>` 这类占位符当正样本，而占位符恰是闸门按匹配项豁免的对象 ⇒ 换成与闸门自检同形的掩码代号（`项目hm####`）。
- **README 目标面枚举未随实现更新**（`:15`）：注释写「非交互，含 ZCode」，同行示例已是 `--target hermes,claude,zcode,pi` ⇒ 注释补 `pi`。
- **README 把两道装置写成一个**（`:24`）：原文「推送前有闸门（`scripts/publish-check.py` 脱敏 + CHANGELOG 同步）」，而 CHANGELOG 同步由 `scripts/hooks/pre-push` 实现、`publish-check.py` 无该逻辑 ⇒ 拆到各自文件上。
- **闸门名单注释比正则少一项**（`scripts/hooks/pre-push`）：注释与报错文案枚举 6 项，`grep -qE` 正则实含 `assets/` 共 7 项 ⇒ 两处补齐。该文件此前与其余 7 仓 md5 一致（qwiki 多一道 number-audit 除外），本仓先改，其余仓待同步。
- 版本 1.6.6 → 1.6.7

## 1.6.6 (2026-10-15)

### Fixed（本仓自洽：README ↔ SKILL.md ↔ CHANGELOG）

- **README 手动复制清单漏拷 `CHANGELOG.md`**：install.sh 的 `COPY_DIRS` 覆盖 SKILL.md + CHANGELOG.md + templates/，手动路径却只拷后两样 ⇒ 两套装法产出不同产物；复制清单补齐（含 pi 目标注释）。
- **README 声称「支持文件清单在 SKILL.md 末尾」而 SKILL.md 无该段**（文件间约束 #2 要求该段存在）：SKILL.md 在「## 相关」前补「## 支持文件清单」，列 `templates/skill-template.md` 与仓内闸门 `scripts/publish-check.py`（注明后者不随安装下发）；README 表述与之一致。
- **install.sh 规范里的条件 append 条目描述比实现严**：原文「必须 if 不能 `[ ] &&`」与本仓 install.sh 自身几处写法冲突 ⇒ 改为「作为整条命令最后一个单元时必须用 if；实测本仓中间位置的 `[ ] &&` 无害，但新写时一律用 if」。
- **README 安装目标清单与 `--target` 示例漏 `pi`**（install.sh 早已支持 `~/.pi/agent`）：清单、示例、手动复制注释三处补齐。
- **CHANGELOG 版本乱序**：1.6.1 段排在 1.0.0 之后（末位），按日期上移到 1.6.2 与 1.6.0 之间。
- **1.6.5 条目自身触发本版闸门**：三个洞的举例里写了真实项目代号（大小写那档），与「别把真名写进 CHANGELOG」直接冲突 ⇒ 改写为不含真名的等价表述（事实不删）。
- 版本 1.6.5 → 1.6.6

## 1.6.5 (2026-10-15)

### Fixed
- **发布闸门 publish-check.py 三个洞同批修**（脱敏反复回潮的根因之一：闸门对真实标识零防护却一直报「干净」）：
  ① 代号正则大小写不敏感（旧版只认小写，同一代号的大写写法整体漏）；
  ② 白名单改为**按匹配项豁免**（旧版 `if any(a in line for a in ALLOW): continue` 是整行豁免——一行里出现
  `<项目A>` 就给同行的真名背书；且原 7 个词对 12 条 pattern 全部零命中 = 纯死豁免）；
  ③ 词边界改用显式字符类（`\b` 在 str 模式下 `\w` 含汉字 ⇒「项目hm####」这类中文常见写法漏检（代号紧贴汉字），
  加不加空格决定报不报）。另补 PRJ0## / 裸数字代号 / MAC 连字符 / 凭据变体（`token=`、`Bearer`、中文密码赋值）/
  邮箱 六类形态。
- **`--self-test` 样本按已知缺陷类别扩容**（原 15 条只覆盖「正常裸值」一档，上述六类一条样本都没有）；
  自检与扫描共用同一份判定口径（`line_hits`），不再各写一套造成假绿。
- **`scripts/hooks/pre-push` 在推送路径上先跑 `--self-test`**（规则表被改坏时不得静默通过）；
  CHANGELOG 名单统一为超集（SKILL / SCHEMA / install.sh / templates / references / scripts，9 仓文本自此一致）；
  基点改为 `origin/main → gitee/main → 根提交` 三级回落（旧版无 `origin/main` 时回落到 `HEAD~1`，
  多笔提交一起推会只看最后一笔）。
- **新增本机私用名单 `scripts/publish-deny.local`**（gitignore、不进仓）：人名 / 昵称 / 厂商 SDK 名 /
  内部 skill 名这类**无法正则枚举、也不该写进发布仓**的标识，在本机未跟踪文件里逐行列真名，闸门自动一并扫。

### 说明
- 口径全文见 `scripts/publish-check.py` 文件头（已知边界：长 hash 折行、全角句点 IP 不识别；该文件自身在
  `SKIP_FILES` 内，改它时需人工核对）。9 仓共用同一份闸门（md5 一致）——改一处必须批量同步。
- 版本 1.6.4 → 1.6.5

## 1.6.4 (2026-10-14)

### Fixed（install.sh 两处 + 发布闸门，与 qwiki-skill 同款）

- **`--target hermes,claude`（空格形式）从未生效**：解析只认 `--target=`，空格形式把值吞成空串后落进"非交互 = 装全部"分支——而 README / 用法行 / 脚本头部**三处文档全写的空格形式**。现两种形式都支持，且 `--target` 缺参数时直接报错退出（不再静默装全部）。
- **备份落在 skills 扫描根之内**：旧版备份写成 `<skills>/<name>.bak.<时间戳>`，而 agent 按「含 SKILL.md 的目录」发现技能、不看目录名 ⇒ 备份被当成**第二个同名技能**加载（重名歧义下静默跳过其一）。现备份落 `<tool 根>/skills-backup/`（扫描根之外），README 与脚本尾注同步。
- **挂上 pre-push 闸门（脱敏 + CHANGELOG 同步）**：`scripts/publish-check.py`（扫项目代号 / 内网 IP / MAC / 长 hash / 凭据 / 本机路径，自带 `--self-test`）+ `scripts/hooks/pre-push`；启用一次 `git config core.hooksPath scripts/hooks`。理由与脱敏规则同源：**靠人记必然回潮**，检查要做成"推送时自动拦"而不是"记得手动跑"。CHANGELOG 闸门名单 = SKILL.md / install.sh / templates/ / scripts/。
- 版本 1.6.3 → 1.6.4

本文件记录版本历史。版本号定义在 SKILL.md frontmatter 的 `version` 字段（单一真相源）。

## 1.6.3 (2026-10-01)

### Added（发现层约束）
- **文件间约束 #11：发现层只有一层**——`<skills_dir>/<name>/SKILL.md` 是唯一被发现的形式；分类子目录（`<skills_dir>/<category>/<name>/`）在 **Claude Code 侧整棵子树不可见**（只扫一层），`category:` frontmatter 也不参与发现（Hermes 侧支持分类）。2026-09-30 实测：一次拍平 10 个死 skill、16 个恢复可发现。⇒ 需多 agent 共用的 skill 一律平铺在 skills 目录顶层

## 1.6.2 (2026-08-29)

### Changed（skill 归属于 agent，无终端直连通道）

- **skitem 结构描述去掉 isatty 双通道**：scripts/ 定位从「终端直连 vs AI 调用」改为「agent 专用，无终端直连通道」
- **脚本规范（SKILL.md + templates/skill-template.md）同步**：去除真人终端向导交互场景，脚本面向 AI 调用（--xxx 参数路径）
- 版本 1.6.1 → 1.6.2

## 1.6.1 (2026-08-28)

### Added

- install.sh 新增 pi 安装目标（探测 `~/.pi/agent` → 安装到 `~/.pi/agent/skills/<skill>`，pi 自动发现）
- 版本 1.6.0 → 1.6.1

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

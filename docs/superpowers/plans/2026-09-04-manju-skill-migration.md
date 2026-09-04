# Manju Skill 通用化迁移 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将 `.claude` 专用漫剧资料包迁移为位于 `skills/manju-skill/` 的通用 Agent Skills/Codex 技能，默认支持80集、五种创作模式和统一质检工作流。

**Architecture:** 以一个精简 `SKILL.md` 作为模式路由器，按需加载 `references/` 中的创作规则，并用 `assets/templates/` 提供可复制的输出骨架。Codex UI 元数据隔离在 `agents/openai.yaml`；技能核心不依赖 Claude 命令、Claude Agent 或特定模型。

**Tech Stack:** Markdown、YAML、Agent Skills 目录规范、Codex `agents/openai.yaml`、shell 静态校验。

**Spec:** `docs/superpowers/specs/2026-09-04-manju-skill-migration-design.md`

## Global Constraints

- 直接删除 `.claude/`，不保留旧版备份。
- 新技能位于 `skills/manju-skill/`。
- 默认参数统一为80集、每集120秒、每批5集、共16批。
- 默认五幕，每幕16集；用户明确参数优先于默认值。
- 只保留一种 Markdown 表格分镜格式。
- 所有恋爱或暧昧参与者必须明确为成年人；禁止未成年编码、胁迫亲密及生存资源换取亲密。
- 不修改现有用户创作产物，包括根目录剧本、`分镜/` 和 `大纲/` 下的文件。
- 不安装到用户级技能目录，不提交或推送 Git。
- 所有文件编辑使用 `apply_patch`；删除前必须精确列出 `.claude/` 内目标。

---

### Task 1: 建立迁移前基准

**Files:**

- Read: `.claude/CLAUDE.md`
- Read: `.claude/agents/manju-aligner.md`
- Read: `.claude/skills/manju-skill/SKILL.md`
- Read: `README.md`
- Preserve: `大纲/**`
- Preserve: `分镜/**`
- Preserve: `《别慌，我真不是诡异始祖》第1集分镜脚本.md`

**Interfaces:**

- Consumes: 已确认设计和当前仓库状态。
- Produces: 迁移前失败清单，供迁移后扫描逐项验证。

- [ ] **Step 1: 验证旧入口缺少标准 frontmatter**

Run:

```bash
sed -n '1,8p' .claude/skills/manju-skill/SKILL.md
```

Expected: 第一行直接是 Markdown 标题，不是 `---`。

- [ ] **Step 2: 验证声明的四个命令没有实现**

Run:

```bash
find .claude -type f \( -path '*/character/*' -o -path '*/catalog/*' -o -path '*/write/*' -o -path '*/align/*' \) -print
```

Expected: 无输出。

- [ ] **Step 3: 记录旧配置冲突**

Run:

```bash
rg -n '60集|70集|12批|14批|三幕|四幕|10项|11项|/character|/catalog|/write|/align' README.md .claude
```

Expected: 同时出现60/70集、12/14批、三/四幕、10/11项和未实现命令。

- [ ] **Step 4: 记录原始工作树边界**

Run:

```bash
git status --short
```

Expected: 只有本次新增的 `docs/` 尚未跟踪；现有创作产物无改动。

### Task 2: 创建通用技能入口与 Codex 元数据

**Files:**

- Create: `skills/manju-skill/SKILL.md`
- Create: `skills/manju-skill/agents/openai.yaml`

**Interfaces:**

- Consumes: 设计文档中的调用、默认参数、依赖和安全约束。
- Produces: `$manju-skill` 唯一入口及 Codex UI 描述，后续 references 和 assets 必须与这里的路径一致。

- [ ] **Step 1: 编写标准 frontmatter**

`SKILL.md` frontmatter 必须包含：

```yaml
---
name: manju-skill
description: Use when creating, continuing, or reviewing Chinese visual-first AI comic dramas, including story outlines, character bibles, episode catalogs, and storyboard scripts.
---
```

- [ ] **Step 2: 编写五模式路由和依赖检查**

正文必须明确 `outline`、`character`、`catalog`、`write`、`review` 的识别条件、要读取的 reference/template、上游依赖和主要输出。无法判断模式时只问一个关键问题；已有项目默认续写，不重新生成上游文件。

- [ ] **Step 3: 编写共享输出契约**

入口必须明确默认80集、120秒、5集一批、16批、`manju-output/<作品名>/` 输出结构、用户参数覆盖规则、聊天展示不落盘规则及已有文件的增量更新规则。

- [ ] **Step 4: 编写入口级安全边界**

入口必须包含成年角色、明确同意、非胁迫亲密、非性化未成年编码、主要角色主动性和平台约束优先级。

- [ ] **Step 5: 创建 `agents/openai.yaml`**

文件只包含：

```yaml
interface:
  display_name: "Manju Visual Drama"
  short_description: "创作、续写并质检视觉优先的中文AI漫剧项目与分镜脚本"
  default_prompt: "使用 $manju-skill 创建一部视觉优先的中文AI漫剧，并从项目设定和大纲开始。"
```

- [ ] **Step 6: 运行首次结构校验并观察预期失败**

Run:

```bash
uv run --with pyyaml python /Users/kyh/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/manju-skill
```

Expected: frontmatter 和目录本身有效；若因尚未创建引用文件失败，失败项必须只涉及后续任务中的已知缺失资源。

### Task 3: 重写故事开发、角色和目录参考

**Files:**

- Create: `skills/manju-skill/references/story-development.md`
- Create: `skills/manju-skill/references/character-bible.md`
- Create: `skills/manju-skill/references/episode-catalog.md`

**Interfaces:**

- Consumes: `SKILL.md` 模式名和统一默认参数。
- Produces: `outline`、`character`、`catalog` 三种模式的领域方法；模板字段在 Task 5 中与这些参考逐一对应。

- [ ] **Step 1: 编写 `story-development.md`**

内容必须包含最低输入、题材适配、核心机制限制、80集五幕结构、单集承诺/兑现、伏笔回收、钩子多样化和反同质化规则。将旧版固定男频套路改为可选模式，不保留固定“每三集打脸、每五集擦边”。

- [ ] **Step 2: 编写 `character-bible.md`**

内容必须覆盖人物目标、阻力、能动性、关系变化、视觉身份锚点、服装阶段、标志性动作、能力限制和跨集一致性。明确绘图关键词可选中英双语，不要求绑定特定绘图模型。

- [ ] **Step 3: 编写 `episode-catalog.md`**

内容必须定义目录字段：集数、标题、推进目标、冲突、情绪兑现、结尾钩子、角色变化、伏笔、视觉权重和连续性依赖；包含检查重复钩子和制作成本峰值分布的方法。

- [ ] **Step 4: 检查三个参考的规格一致性**

Run:

```bash
rg -n '60集|70集|12批|14批|每3集|每三集|每5集必|每五集必' skills/manju-skill/references
```

Expected: 无输出。

### Task 4: 重写分镜、质检和示例参考

**Files:**

- Create: `skills/manju-skill/references/storyboard-writing.md`
- Create: `skills/manju-skill/references/quality-review.md`
- Create: `skills/manju-skill/references/example.md`

**Interfaces:**

- Consumes: Task 3 定义的大纲、角色和目录字段。
- Produces: 单集剧本生成规则、10项评分规则和一个跨模式示例。

- [ ] **Step 1: 编写 `storyboard-writing.md`**

只定义 Markdown 表格分镜。列固定为镜号/时段、景别/运镜、画面、台词/旁白、声音/制作备注；统一采用0–15、15–60、60–100、100–120秒节奏。说明如何控制角色一致性、复杂动作、文字元素、镜头成本和连续性。

- [ ] **Step 2: 编写 `quality-review.md`**

固定10个维度、每项10分，定义90–100、80–89、60–79、低于60四档。严重安全问题必须覆盖总分并要求修订。报告必须引用集数和镜号，区分必须修改与建议优化，默认只修改当前批次。

- [ ] **Step 3: 编写 `example.md`**

使用明确成年角色的原创非末日示例，紧凑展示项目参数、五幕摘要、角色锚点、目录条目、单集片段和一条带证据的质检结论。示例不得引入固定性别视角、品牌、现成作品名或厂商专用提示词。

- [ ] **Step 4: 检查分镜和评分只有一套定义**

Run:

```bash
rg -n '【Frame|25-30个分镜|3秒-45秒-90秒|0-5秒|5-40秒|11项' skills/manju-skill
```

Expected: 无输出。

### Task 5: 创建六个输出模板

**Files:**

- Create: `skills/manju-skill/assets/templates/project.md`
- Create: `skills/manju-skill/assets/templates/outline.md`
- Create: `skills/manju-skill/assets/templates/characters.md`
- Create: `skills/manju-skill/assets/templates/catalog.md`
- Create: `skills/manju-skill/assets/templates/episode.md`
- Create: `skills/manju-skill/assets/templates/review.md`

**Interfaces:**

- Consumes: Task 2–4 中定义的字段、目录、节奏和评分规则。
- Produces: 可复制到 `manju-output/<作品名>/` 的 Markdown 骨架。

- [ ] **Step 1: 创建项目、总纲和角色模板**

`project.md` 包含作品参数、默认值、目标平台、内容边界和进度；`outline.md` 包含五幕目标、80集分集表和伏笔账本；`characters.md` 包含目标、能动性、视觉锚点、阶段变化、能力限制和关系表。

- [ ] **Step 2: 创建目录、剧集和质检模板**

`catalog.md` 使用 Task 3 的固定字段；`episode.md` 使用 Task 4 的唯一表格分镜格式；`review.md` 使用10项100分、证据、必须修改、建议优化和复检结果字段。

- [ ] **Step 3: 检查模板不存在旧规格和危险默认值**

Run:

```bash
rg -n '60集|70集|12批|14批|萝莉|校服|献身|屈服|擦边.*必' skills/manju-skill/assets/templates
```

Expected: 无输出。

### Task 6: 重写 README 并删除 Claude 专用结构

**Files:**

- Modify: `README.md`
- Delete: `.claude/CLAUDE.md`
- Delete: `.claude/agents/manju-aligner.md`
- Delete: `.claude/skills/manju-skill/SKILL.md`
- Delete: `.claude/skills/manju-skill/outline-method.md`
- Delete: `.claude/skills/manju-skill/output-style.md`
- Delete: `.claude/skills/manju-skill/examples/chapter-example.md`
- Delete: `.claude/skills/manju-skill/examples/character-example.md`
- Delete: `.claude/skills/manju-skill/examples/outline-example.md`
- Delete: `.claude/skills/manju-skill/templates/chapter-index-template.md`
- Delete: `.claude/skills/manju-skill/templates/chapter-template.md`
- Delete: `.claude/skills/manju-skill/templates/character-template.md`
- Delete: `.claude/skills/manju-skill/templates/outline-template.md`

**Interfaces:**

- Consumes: 完整的新技能目录。
- Produces: 与实际结构一致的仓库入口文档；仓库不再包含 `.claude`。

- [ ] **Step 1: 重写 README**

README 必须说明技能定位、目录、安装/复制方式、`$manju-skill` 五模式调用示例、默认80集/16批、输出结构、安全边界和验证方式。不得声明不存在的命令、自动 Agent 或模型要求。

- [ ] **Step 2: 删除精确列出的旧文件**

使用 `apply_patch` 删除上面列出的12个文件，再删除已经为空的 `.claude` 子目录。删除前运行：

```bash
find .claude -type f -print | sort
```

Expected: 输出集合必须与本任务的删除清单完全一致。

- [ ] **Step 3: 确认用户创作产物未被修改**

Run:

```bash
git status --short -- '大纲' '分镜' '《别慌，我真不是诡异始祖》第1集分镜脚本.md'
```

Expected: 无输出。

### Task 7: 完整验证与前向测试

**Files:**

- Validate: `skills/manju-skill/**`
- Validate: `README.md`
- Validate: `docs/superpowers/specs/2026-09-04-manju-skill-migration-design.md`
- Validate: `docs/superpowers/plans/2026-09-04-manju-skill-migration.md`
- Preserve: 所有用户创作产物。

**Interfaces:**

- Consumes: 完整迁移结果。
- Produces: 可复核的结构校验、残留扫描、行为测试和 diff 证据。

- [ ] **Step 1: 运行官方结构校验**

Run:

```bash
uv run --with pyyaml python /Users/kyh/.codex/skills/.system/skill-creator/scripts/quick_validate.py skills/manju-skill
```

Expected: `Skill is valid!`

- [ ] **Step 2: 检查引用完整性和 YAML**

逐项确认 `SKILL.md` 引用的6个 reference、6个 template 和 `agents/openai.yaml` 都存在；确认 `openai.yaml` 字符串均加引号且 `default_prompt` 包含 `$manju-skill`。

- [ ] **Step 3: 扫描 Claude 和旧规格残留**

Run:

```bash
test ! -e .claude
rg -n '`/(character|catalog|write|align)([ `]|$)|Claude 3|manju-aligner|60集|70集|12批|14批|三幕式' README.md skills/manju-skill
```

Expected: `test` 成功，`rg` 无输出。设计和计划文档保留历史迁移说明，不纳入该扫描。

- [ ] **Step 4: 前向测试新建大纲场景**

让独立评估者使用 `$manju-skill` 处理：“创建一部面向成年观众的都市奇幻漫剧：一名28岁的文物修复师能听见古物记忆，请先完成项目设定和80集大纲。”输出放入临时目录。检查是否选择 `outline`、采用五幕80集、未加载无关分镜/质检内容，并遵守成年角色边界。

- [ ] **Step 5: 前向测试批次续写场景**

在临时目录提供最小 `project.md`、`outline.md`、`characters.md` 和 `catalog.md`，让独立评估者处理：“续写第11–15集。”检查是否选择 `write`、只生成对应5个剧集文件、使用唯一表格分镜格式且不重写上游文件。

- [ ] **Step 6: 前向测试质检场景**

在临时目录提供含两个重复结尾钩子和一个能力限制冲突的5集样本，让独立评估者处理：“质检这一批并给出需要修订的地方。”检查是否选择 `review`、使用10项100分、引用具体集数/镜号并因结构问题给出低于80分结论。

- [ ] **Step 7: 检查最终 diff 和格式**

Run:

```bash
git diff --check
git status --short
git diff -- README.md skills/manju-skill .claude
```

Expected: 无空白错误；仅 README、新技能、`.claude` 删除和已批准的 docs 文档发生变化，用户创作产物无变化。

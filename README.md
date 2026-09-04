# Manju Visual Drama Skill

一套用于创作、续写和质检中文视觉系 AI 漫剧的通用 Agent Skill。它将故事开发、角色一致性、分集目录、单集分镜和批次质检连接成一个可持续续写的生产流程。

## 默认规格

- 80集，每集120秒。
- 每批5集，共16批。
- 五幕结构，每幕16集。
- 默认竖屏视觉优先，可由用户覆盖。
- 中文输出；需要时附英文绘图关键词。
- 默认输出到 `manju-output/<作品名>/`。

这些都是默认值，用户指定的集数、时长、批次、画风、语言和输出目录优先。

## 技能结构

```text
skills/manju-skill/
├── SKILL.md
├── agents/openai.yaml
├── references/
│   ├── story-development.md
│   ├── character-bible.md
│   ├── episode-catalog.md
│   ├── storyboard-writing.md
│   ├── quality-review.md
│   └── example.md
└── assets/templates/
    ├── project.md
    ├── outline.md
    ├── characters.md
    ├── catalog.md
    ├── episode.md
    └── review.md
```

`SKILL.md` 只负责识别任务、加载当前阶段所需参考和执行共享规则。详细方法与模板按需读取，避免每次加载整个资料包。

## 安装

将完整的 `skills/manju-skill/` 文件夹复制到你的 Agent Skills 目录，保持文件夹名为 `manju-skill`。

Codex 的个人技能目录通常为：

```text
$CODEX_HOME/skills/manju-skill/
```

未设置 `CODEX_HOME` 时通常使用：

```text
~/.codex/skills/manju-skill/
```

其他兼容 Agent Skills 的工具，请复制到该工具声明的 skills 目录。技能核心只依赖标准 `SKILL.md`；`agents/openai.yaml` 是可选的 Codex 展示元数据。

## 使用

唯一入口是 `$manju-skill`。可以直接描述目标，或明确指定模式。

### 1. 创建项目和大纲

```text
使用 $manju-skill 的 outline 模式：创作一部都市奇幻漫剧，主角是28岁的文物修复师，能听见古物记忆。按默认规格生成项目设定和分集大纲。
```

### 2. 创建角色圣经

```text
使用 $manju-skill 的 character 模式，读取现有项目和大纲，生成 characters.md。
```

### 3. 创建生产目录

```text
使用 $manju-skill 的 catalog 模式，把大纲转换成逐集生产索引，并检查钩子重复与制作成本。
```

### 4. 编写指定批次

```text
使用 $manju-skill 的 write 模式，续写第11–15集，每集独立保存；不要重写上游文件。
```

### 5. 批次质检

```text
使用 $manju-skill 的 review 模式，质检第11–15集，给出100分制评分、具体镜号证据和最小修订建议。
```

## 输出目录

```text
manju-output/<作品名>/
├── project.md
├── outline.md
├── characters.md
├── catalog.md
├── episodes/
│   ├── episode-001.md
│   └── episode-080.md
└── reviews/
    ├── batch-01.md
    └── batch-16.md
```

写作模式会先读取项目参数、大纲、角色圣经、目录、上一集和相关质检报告。已有文件默认增量续写，不会无提示重建整个项目。

## 质量标准

每个默认批次完成后，从以下10项进行100分制质检：剧情推进、人物一致性与主动性、时间线与因果、核心机制、单集节奏、视觉可生成性、钩子多样性、连续性与伏笔、制作可行性、内容安全与平台适配。

- 90–100：通过。
- 80–89：通过，可选优化。
- 60–79：修订后复检。
- 低于60：先修复结构问题。
- 严重内容安全问题不受总分豁免。

## 内容边界

- 所有恋爱或性张力参与者必须明确为成年人。
- 不性化未成年人或未成年编码角色。
- 不把食物、住所、安全、工作或权力交换包装成亲密同意。
- 暧昧不是强制指标；信任、竞争、牺牲、谜团、背叛和价值选择都可以承担钩子。
- 主要角色需要独立目标、主动选择和实际剧情作用。
- 用户提供目标平台规则时，遵循更严格的限制。

## 校验

使用技能创建工具附带的结构校验器检查 frontmatter 和目录：

```bash
python3 /path/to/skill-creator/scripts/quick_validate.py skills/manju-skill
```

结构校验不能替代真实创作测试。发布前至少测试：新建大纲、从已有项目续写一个批次、对含连续性问题的批次进行复检。

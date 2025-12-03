# 玄幻漫剧创作 Agent 系统使用说明

## 📁 项目结构
```
玄幻漫剧创作/
├── .claude/
│   ├── CLAUDE.md (顶层规则文件)
│   ├── agents/
│   │   └── manju-aligner.md (质检Agent配置)
│   └── skills/
│       └── manju-skill/
│           ├── SKILL.md (技能核心配置)
│           ├── outline-method.md (大纲创作方法论)
│           ├── output-style.md (写作风格要求)
│           ├── examples/
│           │   ├── outline-example.md
│           │   ├── character-example.md
│           │   └── chapter-example.md
│           └── templates/
│               ├── outline-template.md
│               ├── character-template.md
│               ├── chapter-index-template.md
│               └── chapter-template.md
└── README.md (本文件)
```

## ✅ 已完成设置
1. ✅ 创建完整的文件夹结构
2. ✅ 填充所有核心配置文件
3. ✅ 创建方法论和风格指南
4. ✅ 准备示例文件
5. ✅ 准备模板文件

## 🔧 下一步操作

### 步骤1：配置 Aligner Sub Agent
由于当前环境限制，需要你手动在 Cursor/Claude 中配置质检 Agent：

1. 在 Cursor/Claude 中打开本项目
2. 按快捷键 `Cmd+K` (macOS) 或 `Ctrl+K` (Windows/Linux)
3. 输入指令：`/Agents`
4. 选择「Create a new Agent」
5. 填写以下配置：

**Agent 配置信息：**
- **Scope**: Project（仅当前项目生效）
- **Creation Method**: Manual（人工创建）
- **Agent Name**: `manju-aligner`
- **Description**: 创作完5集剧本后，自动启动，从11个维度检查剧情一致性、爽点分布、CP糖点等核心指标
- **Tools**: Select all tools
- **Model**: Claude 3 Sonnet
- **Color**: Red

详细配置请参考 `.claude/agents/manju-aligner.md` 文件，其中包含：
- 11项质检维度（包含CP糖点与擦边张力）
- 标准化输出格式
- 完整的工作流程

### 步骤2：开始创作
配置完成后，在 Cursor/Claude 的 Code 面板中启动创作流程：

1. **启动创作向导**
   ```
   启动玄幻漫剧创作Agent
   ```
   系统会加载 `.claude/CLAUDE.md` 和 `.claude/skills/manju-skill/SKILL.md` 的设置

2. **回答三个核心问题**
   - 核心概念：[你的故事设定]
   - 金手指类型：[主角特殊能力]
   - 故事调性：[风格定位]

3. **生成大纲**（自动执行，保存为 `outline.md`）

4. **生成人物小传**
   ```
   /character
   ```
   （保存为 `character.md`）

5. **生成章节目录**
   ```
   /catalog
   ```
   （保存为 `catalog.md`）

6. **批量创作章节**
   ```
   /write 1
   ```
   （生成第1-5集，保存后自动触发质检）
   
   质检通过后继续：
   ```
   /write 2
   /write 3
   ...
   /write 12
   ```
   （共12个批次完成60集）

## 📊 创作流程图
```
启动 → 回答问题 → 生成大纲 → 生成人物 → 生成目录 
                                              ↓
质检通过 ← 修改 ← 质检 ← 写5集 ← 循环12次 ←┘
   ↓
完成60集
```

## 🎯 核心特性
- **三幕式结构**：1-20集（开端）、21-45集（对抗）、46-60集（高潮）
- **爽点密集**：每5集一个大爽点，每集至少1个小爽点
- **视觉化写作**：每段对应一个镜头，便于漫画分镜
- **自动质检**：每5集自动从10个维度检查质量
- **模板驱动**：统一格式，确保风格一致

## 🔍 故障排查
1. **Agent不调用技能包**：检查 `.claude/skills/manju-skill/SKILL.md` 文件路径
2. **Aligner不触发**：在 `.claude/CLAUDE.md` 中再次强调调用规则
3. **上下文超限**：Agent会自动读取已保存的 `.md` 文档恢复上下文

## 💡 示例参考
项目中已包含完整示例：
- 大纲示例：`manju-skill/examples/outline-example.md`（《末日重生:我囤积了百亿物资》）
- 人物示例：`manju-skill/examples/character-example.md`（萧凡、苏晴雨、林婉清、小夜）
- 章节示例：`manju-skill/examples/chapter-example.md`（第3集《女神献媚》）

## 📦 跨项目复用
如需在其他项目中使用此技能包：
1. 压缩 `.claude/skills/manju-skill` 文件夹为 `manju-skill.zip`
2. 在新项目中打开 Claude Code 面板
3. 输入 `/Skills` → 选择「Upload Skills」→ 上传压缩包

---

🎬 **现在可以开始你的漫剧创作之旅了！**

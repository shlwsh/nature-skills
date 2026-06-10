# Nature Skills 使用手册

## 1. 项目简介
`nature-skills` 是由袁一哲创建的一个专注于学术科研与论文写作的 AI 指令集集合（Skills）。它通过提供一系列可复用的、基于 `SKILL.md` 的指令包，帮助研究人员在使用 Codex、Claude Code 或其他 AI Agent 时，能够以《自然》（Nature）等顶级期刊的标准来完成绘图、润色、写作、回复审稿人等科研任务。

本项目汇聚了多元思维，旨在告别低效内耗，让顶尖的 AI 技术直接赋能学术产出。

## 2. 安装与配置

`nature-skills` 不是一个 Python 或 npm 包，而是一系列指令与配套文件的集合。核心原则是：**请复制或引用整个技能文件夹，而不仅仅是 `SKILL.md`，因为许多技能依赖于同目录下的其他参考文件和脚本。**

### 2.1 针对 Codex 的安装

**方式一：通过插件市场安装（推荐）**
```bash
codex plugin marketplace add https://github.com/Yuan1z0825/nature-skills --ref main
codex plugin add nature-skills@nature-skills
```
安装后重启 Codex，所有 `nature-*` 技能即可使用。

**方式二：手动本地安装**
```bash
git clone https://github.com/Yuan1z0825/nature-skills.git
cd nature-skills
mkdir -p ~/.codex/skills
# 复制共享文件（必须）
cp -R skills/_shared ~/.codex/skills/
# 复制所需的单一技能（例如 nature-polishing）
cp -R skills/nature-polishing ~/.codex/skills/
# 或者复制全部技能
for d in skills/nature-*; do cp -R "$d" ~/.codex/skills/; done
```

### 2.2 针对 Claude Code 的安装

**方式一：直接安装插件（推荐）**
```bash
claude plugin marketplace add Yuan1z0825/nature-skills
claude plugin install nature-skills@nature-skills
```

**方式二：使用 Subagent / Wrapper 封装本地克隆**
保持本地代码库更新，并在 Claude 中创建快捷调用：
```bash
# 1. 克隆项目
mkdir -p ~/ai-skills && cd ~/ai-skills
git clone https://github.com/Yuan1z0825/nature-skills.git

# 2. 创建 Subagent (以 nature-polishing 为例)
mkdir -p ~/.claude/agents
cat > ~/.claude/agents/nature-polishing.md <<'EOF'
---
name: nature-polishing
description: 用于 Nature 级别的学术润色、重构或中英翻译。
---
请首先阅读 `~/ai-skills/nature-skills/skills/nature-polishing/SKILL.md` 并将其作为执行标准。
仅读取该技能和 `_shared` 目录下需要的文件。
EOF
```
之后在 Claude Code 中通过提及 `nature-polishing` 来调用。

### 2.3 其他 Agent / 通用方式

如果你的 Agent 支持自定义 prompt 库或系统预设，可以直接将 `skills/nature-<topic>` 文件夹整体作为 Prompt 包引入。请确保同时保留 `SKILL.md`、`references/` 和 `skills/_shared/` 以防丢失上下文。

## 3. 使用流程示例

安装完成后，你可以在与 AI 的对话中通过自然语言直接唤起这些能力。

**示例 1：学术润色**
> "Polish this abstract in Nature style."

**示例 2：文献 PPT 制作**
> "Turn this paper into a Chinese journal-club PPT."

**示例 3：论文图表生成**
> "Generate a Nature style spatial imaging figure for this data."

AI 将会自动去读取对应的 `SKILL.md` 指令规则并严格按照《自然》期刊的标准执行任务。

## 4. 注意事项与更新

- **不要只复制单独的 `SKILL.md`**：这会导致 AI 丢失必需的背景设定、文件依赖和模板，导致输出质量下降。
- **项目更新**：直接在克隆的仓库中执行 `git pull`，然后重新拷贝文件，或在插件市场进行 `upgrade` 即可获取最新技能。

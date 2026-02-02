# My Prompts

我的 AI Agent 配置和技能集合。

## 项目结构

```
My-Prompts/
├── AGENTS.md              # Agent 行为规范配置
└── skills/                # 技能工具集
    ├── content-analysis/      # 深度内容分析框架
    ├── create-plan/           # 制定计划工具
    ├── intervals-icu/         # 运动员训练数据管理
    ├── perspective-gathering/ # 视角收集与利益相关者分析
    ├── searching-with-exa/    # 智能网页搜索
    └── zh-rewrite/            # 中文重写工具
```

## Skills

### 1. create-plan
制定计划工具，用于将用户需求转化为可执行的操作计划。

**使用场景：**
- 用户明确要求制定计划时
- 需要将复杂任务拆解为步骤时

**功能特点：**
- 快速扫描项目上下文
- 识别约束条件和技术栈
- 生成结构化的行动清单

---

### 2. intervals-icu
基于 Intervals.icu API 的运动员训练数据管理工具集。

**使用场景：**
- 查询或分析训练活动
- 管理训练计划和日程安排
- 查看运动员健康数据趋势
- 同步训练数据

**功能特点：**
- 获取活动列表和详情
- 训练日程管理（增删改查）
- 健康数据追踪（睡眠、HRV、压力等）
- 区间分析和数据流获取

**配置方式：**

```bash
# 1. 复制环境变量模板
cp .env.example .env

# 2. 编辑 .env 文件，填入你的 API Key
# - EXA_API_KEY: https://exa.ai/dashboard 获取
# - INTERVALS_API_KEY & INTERVALS_ATHLETE_ID: https://intervals.icu/settings 获取

# 3. 安装 mcpc CLI（如需要）
npm install -g @apify/mcpc

# 4. 配置 mcp.json (位置: ~/.pi/agent/mcp.json)
# 确保 mcp.json 中的 API_KEY 使用环境变量引用: "${EXA_API_KEY}"
```

> ⚠️ **重要安全提示**: 请勿将 `.env` 文件提交到版本控制系统中，`.env` 已被添加到 `.gitignore`。

---

### 3. searching-with-exa
基于 Exa AI 的智能网页搜索工具。

**使用场景：**
- 查找技术文档和 API 文档
- 搜索代码示例和最佳实践
- 查找 GitHub 仓库和开源项目
- 获取研究论文和技术文章

**功能特点：**
- 智能意图识别（概念、教程、代码、论文、新闻等）
- 深度语义搜索
- 自动内容抓取和摘要
- 类别过滤（GitHub、论文、新闻等）

**使用方式：**
```bash
# 智能搜索（推荐）
uv run ~/.pi/agent/skills/searching-with-exa/scripts/exa_fetch.py smart "查询内容"

# 代码搜索
uv run ~/.pi/agent/skills/searching-with-exa/scripts/exa_fetch.py code "Python 示例"

# GitHub 搜索
uv run ~/.pi/agent/skills/searching-with-exa/scripts/exa_fetch.py search "查询" -c github
```

### 4. content-analysis
深度内容分析框架工具。

**使用场景：**
- 深度分析文章、论文、报告的核心论点
- 理解作者的背景、立场和潜在假设
- 对观点进行批判性审视
- 提取可复用的思考框架和方法论
- 学习优秀文章的写作技巧

**功能特点：**
- 五维度系统分析（核心内容、背景语境、批判性审视、价值提取、写作技巧）
- 结构化输出分析报告
- 保持客观中立，深入具体
- 多角度思考，诚实承认信息局限

**触发词：**
- "分析"、"解读"、"评论"、"深层解析"
- "帮我看看这篇文章"
- "深度分析"、"系统解读"

**使用方式：**
直接提供需要分析的文章内容或链接，Agent 将自动按照框架进行分析。

### 5. perspective-gathering
视角收集与利益相关者分析工具。

### 6. zh-rewrite
中文重写工具。

**使用场景：**
- 将外文内容翻译成通俗的中文
- 将复杂的中文表达简化为更易懂的版本
- 保持原意但改变表达方式使内容更清晰
- 润色文稿使其更符合中文阅读习惯

**功能特点：**
- 严格尊重原文原意，不添加不删除关键信息
- 将复杂表达转化为通俗易懂的简体中文
- 优化句式使其符合中文表达习惯
- 保留必要的专业术语并添加注释
- 保持原文的逻辑结构和层次

**触发词：**
- "重写"、"翻译"、"通俗化"
- "简化"、"用中文说"
- "帮我用中文表达"
- "翻译成中文"

**使用方式：**
直接提供需要重写的原文内容，Agent 将自动进行中文重写。

**使用场景：**
- 探讨某个话题时需要了解应该听取哪些人的意见
- 分析某个问题涉及哪些利益相关者
- 了解不同群体对某个话题的可能观点
- 为会议、讨论或决策收集多元视角
- 理解某个话题中的不同立场和论点

**功能特点：**
- 系统识别五类利益相关者（受影响群体、专家、决策者、产业链参与者、公众）
- 多维度观点分析（核心论点、立场差异、潜在共识、隐性假设）
- 讨论价值评估（信息价值、代表性评估、潜在盲点）
- 结构化的综合建议输出

**触发词：**
- "最合适的人是谁"
- "他们会说些什么"
- "谁会关心这个话题"
- "利益相关者分析"
- "多元视角"
- "谁有发言权"

**使用方式：**
直接提供想要探讨的话题，Agent 将自动识别相关利益相关者并分析他们的可能观点。

---

## 快速开始

### 1. 克隆项目
```bash
git clone https://github.com/habitssss/My-Prompts.git
cd My-Prompts
```

### 2. 配置 skills
根据需求配置各 skill 的依赖和 API Key：

- **intervals-icu**: 需要 Intervals.icu 账号的 API Key
- **searching-with-exa**: 需要 Exa AI API Key

### 3. 使用
各 skill 的详细使用说明请参考对应目录下的 `SKILL.md` 文件。

---

## Agent 配置

`AGENTS.md` 文件定义了 Agent 的行为规范：

- 优先考虑代码的可维护性、可读性和开发效率
- 始终使用简体中文回复
- 搜索时使用英文搜索
- 生成代码时添加完整注释
- Web 检索使用英文搜索

---

## License

各 skill 可能使用不同的 License，具体请参考各目录下的 LICENSE 文件。

- create-plan: Apache License 2.0
- intervals-icu: 查看 SKILL.md
- searching-with-exa: 查看 SKILL.md

---

## 更新日志

### v1.1.0 (2026-02-02)
- 添加 zh-rewrite 中文重写工具

### v1.0.0 (2025-01-21)
- 初始化项目
- 添加 AGENTS.md 配置文件
- 添加三个核心 skill

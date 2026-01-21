# My Prompts

我的 AI Agent 配置和技能集合。

## 项目结构

```
My-Prompts/
├── AGENTS.md              # Agent 行为规范配置
└── skills/                # 技能工具集
    ├── create-plan/       # 制定计划工具
    ├── intervals-icu/     # 运动员训练数据管理
    └── searching-with-exa/# 智能网页搜索
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
# 安装 mcpc CLI
npm install -g @apify/mcpc

# 配置 mcp.json (位置: ~/.pi/agent/mcp.json)
# 需要设置 API_KEY 和 ATHLETE_ID
```

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

### v1.0.0 (2025-01-21)
- 初始化项目
- 添加 AGENTS.md 配置文件
- 添加三个核心 skill

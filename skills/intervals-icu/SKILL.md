---
name: intervals-icu
description: 基于 Intervals.icu API 的运动员训练数据管理工具集。提供活动数据查询、区间分析、训练日程管理、健康数据追踪（睡眠、HRV、压力等）功能。适用于查询训练活动、分析运动数据、管理训练计划、监测恢复状态等场景。
---

# Intervals.icu - 运动员训练数据管理

## Overview

基于 Intervals.icu API 的 MCP 工具集，提供运动员训练数据的完整管理能力：

- **活动数据** - 获取训练活动详情、区间分析、数据流
- **日程管理** - 查看、创建、更新、删除训练事件
- **健康追踪** - 获取每日健康指标（睡眠、HRV、压力等）
- **智能分析** - 自动识别活动类型、格式化输出

## When to Use This Skill

使用此 skill 当用户需要：
- 查询或分析 Intervals.icu 上的训练活动
- 管理训练计划和日程安排
- 查看运动员的健康数据趋势
- 同步训练数据到 Intervals.icu 平台

## MCP CLI 使用方法

本工具使用 [apify/mcp-cli](https://github.com/apify/mcp-cli) (`mcpc`) 作为通用 CLI 来调用 MCP 服务器。

### 1. 安装 mcpc CLI

```bash
npm install -g @apify/mcpc
```

### 2. 配置 MCP 服务器

在 `~/.pi/agent/mcp.json` 中配置 Intervals.icu MCP 服务器：

**文件位置**: `~/.pi/agent/mcp.json` (或 `/Users/habits/.pi/agent/mcp.json`)

```json
{
  "mcpServers": {
    "intervals-icu": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/Users/habits/code/intervals-mcp-server",
        "python",
        "src/intervals_mcp_server/server.py"
      ],
      "env": {
        "INTERVALS_API_BASE_URL": "https://intervals.icu/api/v1",
        "API_KEY": "your_api_key_here",
        "ATHLETE_ID": "your_athlete_id_here",
        "LOG_LEVEL": "INFO"
      }
    }
  }
}
```

**环境变量获取方式：**
1. 登录 [Intervals.icu](https://intervals.icu)
2. 进入 Settings → API
3. 复制 Athlete ID 和 API Key

**快速设置脚本：**
```bash
# 从 .env 文件读取并创建 mcp.json
cat > ~/.pi/agent/mcp.json << 'EOF'
{
  "mcpServers": {
    "intervals-icu": {
      "command": "uv",
      "args": [
        "run",
        "--directory",
        "/Users/habits/code/intervals-mcp-server",
        "python",
        "src/intervals_mcp_server/server.py"
      ],
      "env": {
        "INTERVALS_API_BASE_URL": "https://intervals.icu/api/v1",
        "API_KEY": "your_api_key_here",
        "ATHLETE_ID": "your_athlete_id_here",
        "LOG_LEVEL": "INFO"
      }
    }
  }
}
EOF
```

### 3. 查看可用工具

```bash
# 查看所有可用工具 (mcp.json 位置: ~/.pi/agent/mcp.json)
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-list
```

**Intervals.icu MCP 工具列表：**

| 工具名称 | 功能 | 参数 |
|----------|------|------|
| `get_activities` | 获取活动列表 | athlete_id, start_date, end_date, limit, include_unnamed |
| `get_activity_details` | 获取活动详情 | activity_id, api_key |
| `get_activity_intervals` | 获取活动区间数据 | activity_id, api_key |
| `get_activity_streams` | 获取活动数据流 | activity_id, api_key, stream_types |
| `get_events` | 获取训练事件 | athlete_id, start_date, end_date |
| `get_event_by_id` | 获取特定事件详情 | event_id, athlete_id |
| `add_or_update_event` | 添加/更新训练事件 | athlete_id, name, workout_type, start_date, moving_time, distance, workout_doc, event_id |
| `delete_event` | 删除训练事件 | event_id, athlete_id |
| `delete_events_by_date_range` | 按日期范围删除事件 | start_date, end_date, athlete_id |
| `get_wellness_data` | 获取健康数据 | athlete_id, start_date, end_date |

### 4. 调用工具的基本语法

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json <server-name> tools-call <tool-name> <args-json>
```

或者在 `~/.pi/agent` 目录中直接使用相对路径：

```bash
mcpc -c mcp.json <server-name> tools-call <tool-name> <args-json>
```

**参数格式：**
- JSON 对象：`{"key": "value"}`
- 或者从文件读取：`mcpc -c mcp.json intervals-icu tools-call get_activities @args.json`

---

## Commands

### 1. latest - 获取最新活动

获取最近的活动数据。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"limit": 3, "include_unnamed": false}'
```

**参数**:
- `limit` (默认: 3): 获取活动数量
- `include_unnamed` (默认: false): 是否包含未命名活动

**示例**:
```bash
# 获取最新3个活动
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"limit": 3}'

# 获取最新5个活动
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"limit": 5}'
```

### 2. activities - 活动列表查询

按日期范围查询活动列表。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"start_date": "2024-12-01", "end_date": "2024-12-31", "limit": 20}'
```

**参数**:
- `start_date` (默认: 30天前): 开始日期 YYYY-MM-DD
- `end_date` (默认: 今天): 结束日期 YYYY-MM-DD
- `limit` (默认: 10): 最大活动数

**示例**:
```bash
# 查询最近30天的活动
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{}'

# 查询特定日期范围的活动
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"start_date": "2024-12-01", "end_date": "2024-12-31", "limit": 20}'

# 包含未命名活动
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"start_date": "2024-12-01", "end_date": "2024-12-31", "limit": 20, "include_unnamed": true}'
```

### 3. activity - 获取活动详情

获取单个活动的详细信息。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activity_details' '{"activity_id": "abc123xyz"}'
```

**参数**:
- `activity_id` (必需): Intervals.icu 活动ID

**示例**:
```bash
# 获取活动详情
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activity_details' '{"activity_id": "i116909609"}'

# 获取完整数据流
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activity_streams' '{"activity_id": "i116909609", "stream_types": "time,watts,heartrate,cadence,altitude,distance,velocity_smooth"}'

# 获取区间数据
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activity_intervals' '{"activity_id": "i116909609"}'
```

### 4. events - 查看训练日程

查看即将到来的训练事件。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_events' '{"start_date": "2025-01-11", "end_date": "2025-02-10"}'
```

**参数**:
- `start_date` (默认: 今天): 开始日期 YYYY-MM-DD
- `end_date` (默认: 30天后): 结束日期 YYYY-MM-DD

**示例**:
```bash
# 查看未来30天的训练计划
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_events' '{}'

# 查看特定日期范围的事件
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_events' '{"start_date": "2025-01-01", "end_date": "2025-01-31"}'

# 查看特定事件详情
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_event_by_id' '{"event_id": "abc123", "athlete_id": "i431726"}'
```

### 5. add-event - 添加训练事件

创建新的训练事件或训练计划。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'add_or_update_event' '{"name": "Easy Ride", "workout_type": "Ride", "start_date": "2025-01-15", "moving_time": 3600}'
```

**参数**:
- `name` (必需): 训练名称
- `workout_type` (必需): 训练类型（Ride, Run, Swim, Walk, Row）
- `start_date` (默认: 今天): 开始日期 YYYY-MM-DD
- `moving_time` (可选): 预计运动时间（秒）
- `distance` (可选): 预计距离（米）
- `workout_doc` (可选): 训练步骤 JSON 对象
- `event_id` (可选): 如果提供则更新现有事件

**示例**:
```bash
# 创建简单的骑行训练
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'add_or_update_event' '{"name": "Easy Ride", "workout_type": "Ride", "moving_time": 3600}'

# 创建间歇训练（带步骤）
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'add_or_update_event' '{"name": "VO2 Max Intervals", "workout_type": "Run", "start_date": "2025-01-15", "moving_time": 5400, "workout_doc": {"steps": [{"power": {"value": "80", "units": "%ftp"}, "duration": "900", "warmup": true}, {"reps": 4, "steps": [{"power": {"value": "120", "units": "%ftp"}, "duration": "300"}, {"power": {"value": "60", "units": "%ftp"}, "duration": "60"}]}]}}'

# 更新现有事件
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'add_or_update_event' '{"event_id": "abc123", "name": "Updated Ride", "workout_type": "Ride", "moving_time": 7200}'
```

### 6. delete-event - 删除训练事件

删除训练事件。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'delete_event' '{"event_id": "abc123"}'
```

**参数**:
- `event_id` (必需): 事件ID
- `athlete_id` (可选): 运动员ID

**示例**:
```bash
# 删除特定事件
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'delete_event' '{"event_id": "abc123"}'

# 按日期范围删除（删除12月所有事件）
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'delete_events_by_date_range' '{"start_date": "2024-12-01", "end_date": "2024-12-31"}'
```

### 7. wellness - 查看健康数据

获取健康指标数据（睡眠、HRV、体重、压力等）。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_wellness_data' '{"start_date": "2024-12-12", "end_date": "2025-01-11"}'
```

**参数**:
- `start_date` (默认: 30天前): 开始日期 YYYY-MM-DD
- `end_date` (默认: 今天): 结束日期 YYYY-MM-DD

**示例**:
```bash
# 获取最近30天的健康数据
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_wellness_data' '{}'

# 查看特定日期范围
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_wellness_data' '{"start_date": "2024-12-01", "end_date": "2024-12-31"}'
```

### 8. summary - 综合数据摘要

获取训练活动的综合摘要报告（需要多次调用工具）。

```bash
# mcp.json 位置: ~/.pi/agent/mcp.json
# 获取30天综合报告
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"start_date": "2024-12-12", "end_date": "2025-01-11", "limit": 50}'
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_wellness_data' '{"start_date": "2024-12-12", "end_date": "2025-01-11"}'
```

---

## Quick Reference

| 需求 | 命令 |
|------|------|
| 查看可用工具 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-list` |
| 查看最近活动 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"limit": 3}'` |
| 按日期查询活动 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"start_date": "2025-01-06", "end_date": "2025-01-12"}'` |
| 查看活动详情 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activity_details' '{"activity_id": "abc123"}'` |
| 训练日程 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_events' '{}'` |
| 添加训练计划 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'add_or_update_event' '{"name": "Easy Ride", "workout_type": "Ride"}'` |
| 删除事件 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'delete_event' '{"event_id": "abc123"}'` |
| 健康数据 | `mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_wellness_data' '{}'` |

**mcp.json 位置**: `~/.pi/agent/mcp.json` (绝对路径: `/Users/habits/.pi/agent/mcp.json`)

---

## 高级用法

### 从文件传递参数

将参数保存到 JSON 文件中：

```bash
# 创建参数文件
cat > /tmp/get_activities_args.json << 'EOF'
{
  "limit": 10,
  "start_date": "2025-01-01",
  "end_date": "2025-01-31",
  "include_unnamed": false
}
EOF

# 使用参数文件调用 (mcp.json 位置: ~/.pi/agent/mcp.json)
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' @/tmp/get_activities_args.json
```

### 使用环境变量中的认证信息

如果已在 `.env` 或 `mcp.json` 中配置了 `ATHLETE_ID` 和 `API_KEY`，可以省略这些参数：

```bash
# 简化调用（使用默认认证，mcp.json 位置: ~/.pi/agent/mcp.json）
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{}'
```

### 批量查询脚本

```bash
#!/bin/bash
# 获取本周训练摘要
# mcp.json 位置: ~/.pi/agent/mcp.json

echo "=== 本周活动 ==="
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_activities' '{"limit": 20, "start_date": "2025-01-06", "end_date": "2025-01-12"}'

echo -e "\n=== 本周健康数据 ==="
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_wellness_data' '{"start_date": "2025-01-06", "end_date": "2025-01-12"}'

echo -e "\n=== 未来训练计划 ==="
mcpc -c ~/.pi/agent/mcp.json intervals-icu tools-call 'get_events' '{"start_date": "2025-01-13", "end_date": "2025-01-19"}'
```

---

## Step 配置参考

创建训练事件时，可使用以下 step 配置：

| 属性 | 说明 | 示例 |
|------|------|------|
| `duration` | 持续时间（秒） | `{"duration": "1800"}` |
| `distance` | 距离（米） | `{"distance": "5000"}` |
| `power` | 功率设置 | `{"power": {"value": "80", "units": "%ftp"}}` |
| `hr` | 心率设置 | `{"hr": {"value": "75", "units": "%hr"}}` |
| `warmup` | 是否为热身 | `{"warmup": true}` |
| `cooldown` | 是否为冷身 | `{"cooldown": true}` |
| `reps` | 重复次数 | `{"reps": 4}` |
| `steps` | 嵌套步骤 | `{"steps": [...]}`

**完整训练步骤示例：**

```json
{
  "steps": [
    {"power": {"value": "80", "units": "%ftp"}, "duration": "900", "warmup": true},
    {"reps": 4, "steps": [
      {"power": {"value": "110", "units": "%ftp"}, "distance": "500", "text": "高强度"},
      {"power": {"value": "80", "units": "%ftp"}, "duration": "90", "text": "恢复"}
    ]},
    {"power": {"value": "80", "units": "%ftp"}, "duration": "600", "cooldown": true},
    {"text": "训练完成"}
  ]
}
```

更多 step 配置说明，请参考 [Intervals.icu API 文档](https://intervals.icu/documentation.html)。

---

## 输出示例

### 活动输出示例

```markdown
Activity: Morning Ride
ID: abc123xyz
Type: Ride
Date: 2025-01-10 08:30:00
Distance: 45.2 km
Duration: 1:45:32
Average Power: 185 W
Weighted Avg Power: 192 W
Training Load: 320 TSS
Intensity Factor: 0.85
Heart Rate: 152 bpm (Zone 3)
```

### 健康数据输出示例

```markdown
Wellness Data:
Date: 2025-01-10
Sleep: 7h 23m | HRV: 45ms | Resting HR: 52 bpm
Weight: 72.5 kg | Stress: 28 | Recovery: 85%
Fitness (CTL): 45.2 | Fatigue (ATL): 38.1
```

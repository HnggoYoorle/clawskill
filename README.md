🦞 项目名称：OpenClaw Session Memory - 跨会话记忆连续性系统
1. 项目解决的核心痛点
核心痛点：AI Agent 的「跨会话失忆症」
当前主流 Agent 框架（OpenClaw、Hermes、Claude Code 等）普遍存在一个严重的体验缺陷：
新会话启动后，完全遗忘上一轮对话的所有上下文
用户被迫每次重复「上次我们聊到...」「那个事情进展怎样了」
已达成的共识、待办的事项、约定的执行计划，全部丢失
严重限制了 Agent 作为「长期助手」的可用性，用户被迫把 Agent 当成一次性问答工具
现有方案的问题：
全量历史塞入上下文 → Token 浪费严重，成本呈线性增长
手动标记重要信息 → 增加用户负担，违背「自动化」初衷
完全不处理 → 体验断裂，Agent 显得很蠢
2. 核心逻辑流
本项目采用「零感知 + 分层提取 + 原生注入」的设计：
📩 用户发送新会话第一条消息
    ↓
🔍 触发检测（Skill 层静默触发，用户无感知）
    ↓
    ├─ 检查 MEMORY.md 是否已存在？
    ├─ 存在 → 直接跳过（幂等设计）
    └─ 不存在 → 启动后台记忆恢复线程 🧵
              ↓
📊 检索层：SQLite 查询 state.db
    ├─ 查找同平台最近的已结束会话
    ├─ 排除当前会话（避免自引用）
    └─ 读取该会话最后 15 条消息（兼顾完整性与效率）
              ↓
🧠 处理层：多维度语义提取
    ├─ 待办事项识别 → 匹配「稍后、下次、记得、别忘了」等关键词
    ├─ 关键结论提取 → 匹配「决定、确定、就这样、搞定」等决策型话术
    └─ 对话脉络摘要 → 保留最近 8 轮核心交互
              ↓
📝 生成层：结构化摘要组装
    ├─ 时间信息 + 消息轮数统计
    ├─ 待办事项清单（最多 5 条）
    ├─ 已达成结论（最多 5 条）
    ├─ 最近对话摘要
    └─ 限制总长度 ≤ 2200 字符（Token 友好）
              ↓
💾 注入层：写入 ~/.openclaw/MEMORY.md
    └─ 利用 OpenClaw 原生的「目录文件自动加载到上下文」机制
       无需修改核心，下一条消息自动带上记忆 ✨
              ↓
🤖 Agent 自然回复：「上次我们聊到...，这次继续对吧？」
关键设计亮点：
✅ 零侵入：基于 Skill 机制实现，不修改 Agent 核心代码
✅ 无感知：后台线程异步执行，用户完全感觉不到延迟
✅ 高效率：仅注入关键摘要，比全量历史节省 80%+ Token
✅ 容错强：Skill 层出错不影响主对话流程
✅ 可交互：提供 /memory 查看、/memory refresh 手动刷新命令
3. 实际成果与落地情况
| 指标 | 成果 |
|------|------|
| 部署方式 | 零配置安装，复制 3 个文件到技能目录即可生效 |
| 兼容平台 | 微信 / Telegram / 飞书 / Discord / CLI 全通道支持 |
| Token 效率 | 平均 1500-2000 Token/会话，对比全量历史恢复方案节省 80%+ |
| 用户体验 | 完全无感知，Agent 自然延续话题，无需用户提示 |
| 代码量 | 核心逻辑约 300 行 Python，轻量可维护 |
这个项目解决了 AI Agent 从「一次性工具」走向「长期助手」的关键体验断点，也是我们在探索 Agent 记忆连续性方向的第一个落地成果。后续计划扩展到多会话合并、长期记忆向量库、跨设备记忆同步等方向。
====================================================================================
# 🦞 Project Name: OpenClaw Session Memory – Cross-Session Memory Continuity System

## Core Pain Point Solved

**Core Pain Point: AI Agent's "Cross-Session Amnesia"**

Current mainstream Agent frameworks (OpenClaw, Hermes, Claude Code, etc.) universally suffer from a severe experience flaw:

- After a new session starts, the Agent completely forgets all context from previous conversations
- Users are forced to repeat "last time we talked about...", "how is that thing going..."
- Previously agreed conclusions, pending tasks, and established execution plans are all lost
- This severely limits the Agent's usability as a "long-term assistant", forcing users to treat the Agent as a one-shot Q&A tool

**Problems with existing solutions:**

- Stuffing full history into context → Severe token waste, linear cost growth
- Manually marking important information → Increases user burden, violates the principle of automation
- No handling at all → Broken experience, Agent appears unintelligent

## Core Logic Flow

This project adopts a **zero-perception + hierarchical extraction + native injection** design:
📩 User sends first message of new session
↓
🔍 Trigger detection (Skill layer triggers silently, user unaware)
↓
├─ Check if MEMORY.md already exists?
├─ Exists → Skip directly (idempotent design)
└─ Does not exist → Start background memory recovery thread 🧵
↓
📊 Retrieval layer: SQLite query on state.db
├─ Find recent concluded sessions on the same platform
├─ Exclude current session (avoid self-reference)
└─ Read last 15 messages of that session (balances completeness and efficiency)
↓
🧠 Processing layer: Multi-dimensional semantic extraction
├─ To-do item recognition → Match keywords like "later", "next time", "remember", "don't forget"
├─ Key conclusion extraction → Match decision cues like "decided", "confirmed", "agreed", "done"
└─ Conversation context summarization → Keep last 8 rounds of core interaction
↓
📝 Generation layer: Structured summary assembly
├─ Timestamp + message count statistics
├─ To-do list (max 5 items)
├─ Reached conclusions (max 5 items)
├─ Recent conversation summary
└─ Limit total length ≤ 2200 characters (token-friendly)
↓
💾 Injection layer: Write to ~/.openclaw/MEMORY.md
└─ Leverage OpenClaw's native "directory files auto-loaded into context" mechanism
No core modification needed, next message automatically carries memory ✨
↓
🤖 Agent natural reply: "Last time we talked about..., let's continue, right?"

## Key Design Highlights

- ✅ **Zero intrusion**: Implemented via Skill mechanism, no modification to Agent core code
- ✅ **Zero perception**: Background thread executes asynchronously, user feels no latency
- ✅ **High efficiency**: Only injects key summary, saves 80%+ tokens compared to full history
- ✅ **Fault tolerant**: Errors in Skill layer do not affect main conversation flow
- ✅ **Interactive**: Provides `/memory` view, `/memory refresh` manual refresh commands

## Actual Results & Implementation

| Metric | Result |
|--------|--------|
| Deployment | Zero-config installation, copy 3 files to skills directory and it works |
| Platform compatibility | Full channel support: WeChat / Telegram / Feishu / Discord / CLI |
| Token efficiency | Average 1500–2000 tokens/session, saves 80%+ compared to full history recovery |
| User experience | Completely imperceptible, Agent naturally continues topic without user prompting |
| Code size | Core logic ~300 lines of Python, lightweight and maintainable |

## Summary

This project solves the key experience gap that prevents AI Agents from evolving from "one-shot tools" into "long-term assistants". It is our first practical result in exploring Agent memory continuity. Future directions include multi-session merging, long-term memory vector stores, and cross-device memory synchronization.

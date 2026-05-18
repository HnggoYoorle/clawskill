# 🐚 OpenClaw & Hermes Agent: Skills Engineering & Memory Continuity

> **Project Vision**: This project is not just a "memory plugin." It is a systematic exploration of the **Skills Ecosystem** for **OpenClaw** and **Hermes Agent**.  
> We aim to solve the core challenges of **Skill Creation, Evolution, and Governance** as AI agents transition from "stateless tools" to "stateful long-term assistants."

---

## 🚨 The Problem: Why Reinvent Skills?

Current agent frameworks (OpenClaw, Hermes, Claude Code, etc.) face three major bottlenecks:

| Bottleneck | Manifestation |
| :--- | :--- |
| **🔄 Cross-Session Amnesia** | Agents lose all context upon restart. Users must repeat themselves, and pending tasks vanish. |
| **💸 Token Inefficiency** | Injecting full history causes linear cost explosion; manual state management breaks automation. |
| **📉 Skill Fragmentation** | Static skills lack evolution capabilities; they cannot learn from experience like humans. |

---

## 🎯 Core Contributions

We propose and implement a **"Zero-Invasive + Hierarchical Extraction + Native Injection"** general-purpose skill architecture.

### 1. Unified Skill Abstraction Model
Defining a cross-framework standard compatible with both OpenClaw (explicit) and Hermes (implicit):
Skill = Trigger + InputSchema + Runtime + Output + Lifecycle
纯文本
### 2. Flagship Achievement: Session Memory Continuity
A production-grade solution to "Cross-Session Amnesia."

#### ⚙️ Core Logic Flow
mermaid
flowchart LR
A[📩 First Message in New Session] --> B{🔍 Check MEMORY.md}
B -- Exists --> C[✅ Skip]
B -- Not Exist --> D[🧠 Async Recovery Thread]
D --> E[📊 SQLite State Retrieval]
E --> F[📝 Structured Summary Generation]
F --> G[💾 Write ~/.openclaw/MEMORY.md]
G --> H[🤖 Agent Continues Seamlessly]

#### ✨ Key Features
- **Zero-Invasive:** Implemented via the Skill mechanism; no core agent code modified.
- **Zero-Awareness:** Background async execution; zero perceived latency for the user.
- **High Efficiency:** Reduces Token consumption by **80%+** (injects summaries only).
- **Fault-Tolerant:** Skill-layer errors never block the main conversation flow.

---

## 📊 Implementation Metrics

| Metric | Result |
| :--- | :--- |
| **Deployment** | ✅ Zero-config; copy 3 files to the skills directory. |
| **Platform Support** | 🌐 WeChat / Telegram / Feishu / Discord / CLI |
| **Token Efficiency** | ⚡ Avg. 1,500–2,000 Tokens / Session |
| **Codebase Size** | 🧩 ~300 lines of Python |

---

## 🔬 Skills Evolution Research (OpenClaw vs. Hermes)

This project provides deep insights into two paradigms of Skill engineering:

| Dimension | OpenClaw (Static Skills) | Hermes (Self-Evolving Skills) |
| :--- | :--- | :--- |
| **Origin** | Developer-written | AI-generated from traces |
| **Format** | Python / JSON | `SKILL.md` (Markdown) |
| **Strength** | Stable, Controllable | Personalized, Adaptive |
| **Role in Project** | Infrastructure | Evolution Target |

---

## 🚀 Quick Start

Give your agent long-term memory in 3 steps:
bash
1. Clone the repository
git clone https://github.com/HnggoYoorle/clawskill.git
2. Copy the skill to your OpenClaw skills directory
cp -r openclaw-skills/memory ~/.openclaw/skills/
3. Restart OpenClaw
openclaw restart

**Verify:**
/memory status
---
## 🛣️ Roadmap
- [ ] **Phase 2:** Build a Skill Registry (Community Hub).
- [ ] **Phase 3:** Integrate Vector Databases (ChromaDB/FAISS) for long-term semantic memory.
- [ ] **Phase 4:** Enable cross-framework Skill interoperability (OpenClaw ↔ Hermes).
---

## 📜 License

MIT License

---

> 💡 **One-Line Summary**:  
> **We are not just building a Skill; we are defining how next-generation Agent Skills should be designed and built.**
====================================================================================
# 🐚 OpenClaw & Hermes Agent：技能体系研究与工程实践

> **核心定位**：本项目并非单一的“记忆插件”，而是对 **OpenClaw** 与 **Hermes Agent** 技能（Skills）生态的系统性探索。  
> 我们致力于解决 AI Agent 从“无状态工具”向“有状态长期助手”演进过程中的 **技能构建、演化与治理** 难题。

---

## 🚨 核心痛点：为什么我们需要重新思考 Skills？

当前主流 Agent 框架（OpenClaw, Hermes, Claude Code 等）普遍面临三大瓶颈：

| 瓶颈 | 表现 |
| :--- | :--- |
| **🔄 跨会话失忆** | 新会话启动即清零，用户被迫重复上下文，待办事项与结论全部丢失。 |
| **💸 Token 低效** | 全量历史注入导致成本线性爆炸，手动管理又违背自动化原则。 |
| **📉 技能割裂** | 静态 Skill 缺乏演化能力，无法像人类一样从经验中学习。 |

---

## 🎯 项目核心贡献

我们提出并实现了 **“零侵入 + 分层提取 + 原生注入”** 的通用技能架构。

### 1. 通用技能抽象模型 (Unified Skill Model)
定义了跨框架的技能标准，兼容 OpenClaw 的显式调用与 Hermes 的隐式生成：
Skill = Trigger + InputSchema + Runtime + Output + Lifecycle
纯文本
### 2. 旗舰成果：会话记忆连续性系统 (Session Memory Skill)
这是本项目的首个落地范例，彻底解决了“跨会话失忆症”。

#### ⚙️ 核心逻辑流
mermaid
flowchart LR
A[📩 新会话首条消息] --> B{🔍 检查 MEMORY.md}
B -- 存在 --> C[✅ 直接跳过]
B -- 不存在 --> D[🧠 后台异步提取]
D --> E[📊 SQLite 检索历史]
E --> F[📝 生成结构化摘要]
F --> G[💾 写入 ~/.openclaw/MEMORY.md]
G --> H[🤖 代理自然延续话题]
纯文本
#### ✨ 设计亮点
- **零侵入**：基于 Skill 机制实现，不修改 Agent 核心代码。
- **零感知**：后台线程异步执行，用户无任何延迟感。
- **高效率**：Token 消耗降低 **80%+**（仅注入关键摘要）。
- **强容错**：Skill 层错误绝不阻塞主对话流程。

---

## 📊 落地成果与指标

| 指标 | 成果 |
| :--- | :--- |
| **部署方式** | ✅ 零配置，复制 3 个文件到技能目录即可生效 |
| **平台兼容** | 🌐 微信 / Telegram / 飞书 / Discord / CLI |
| **Token 效率** | ⚡ 平均 1,500–2,000 Tokens / 会话 |
| **代码体量** | 🧩 核心逻辑约 300 行 Python |

---

## 🔬 技能演化研究 (OpenClaw vs Hermes)

本项目深入对比了两种技能范式的差异：

| 维度 | OpenClaw (静态技能) | Hermes (自演化技能) |
| :--- | :--- | :--- |
| **来源** | 开发者编写 | AI 从任务轨迹中生成 |
| **形态** | Python / JSON | `SKILL.md` (Markdown) |
| **优势** | 稳定、可控、安全 | 个性化、自适应 |
| **本项目角色** | 基础设施 | 演化实验对象 |

---

## 🚀 快速开始

只需三步，赋予你的 Agent 长期记忆：
bash
1. 克隆本仓库到本地
git clone https://github.com/HnggoYoorle/clawskill.git
2. 复制技能文件到 OpenClaw 技能目录
cp -r openclaw-skills/memory ~/.openclaw/skills/
3. 重启 OpenClaw，开始新会话
openclaw restart
纯文本
**手动验证：**
/memory status
纯文本
---

## 🛣️ 未来路线图

- [ ] **Phase 2：** 构建 Skill Registry（类似 ClawHub 的技能市场）。
- [ ] **Phase 3：** 引入向量数据库（Vector DB），实现长期语义记忆。
- [ ] **Phase 4：** 实现 OpenClaw 与 Hermes 之间的跨框架技能互操作。

---

## 📜 License

MIT License

---

> 💡 **一句话总结**：  
> **这不是在做一个 Skill，而是在定义下一代 Agent Skill 应该如何被设计与建造。**




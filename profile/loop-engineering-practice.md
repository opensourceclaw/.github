# Loop Engineering 实践：三角色 Agent 开发框架

> *当业界刚刚开始讨论 "Loop Engineering"，我们已经实践了半年*

---

## 背景

2026年，AI 开发领域正在发生范式转移：

| 阶段 | 核心技能 | 年份 |
|------|----------|------|
| Prompt Engineering | 写好提示词 | 2023 |
| Harness Engineering | 构建 Agent 外围系统 | 2024-2025 |
| **Loop Engineering** | **设计 Agent 迭代循环** | **2026** |

Google Engineer **Addy Osmani** 最近在 X 上提出：

> *"Loop engineering is replacing yourself as the person who prompts the agent. You design the system that does it instead."*

而我们，从 2025 年开始就一直在实践这个理念——只是当时没有给它起这个名字。

---

## 什么是 Loop Engineering？

### 核心理念

**不是"写 prompt 让 AI 干活"，而是"设计一个循环让 AI 自己干活"**

Addy Osmani 总结了 5 个 building blocks：

1. **Task Selection** - 选择做什么
2. **Context Preparation** - 准备上下文
3. **Execution** - 执行任务
4. **Validation** - 验证结果
5. **Decision** - 决定下一步

再加上 **Memory** 作为基础设施。

### 业界实践

- **Anthropic Boris Cherny**: "我的工作现在是写循环，而不是写 prompt"
- **Peter Steinberger**: 展示递归循环 + 指标监控系统
- **OpenAI Codex**: 内置完整循环机制

---

## 我们的方案：三角色 Agent 框架

### 架构图

```
┌─────────────────────────────────────────────────────────────┐
│                    Friday (A - Project Manager)             │
│  • 需求分析    • 架构设计    • 验收标准    • 产品发布       │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                    Jarvis (B - Developer)                   │
│  • 概要设计    • 详细设计    • 开发实现    • 单元/集成测试  │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                    Edith (C - QA)                           │
│  • 独立验收测试    • 探索性测试    • 发布把关              │
└──────────────────────────┬──────────────────────────────────┘
                           │
                           ▼ (反馈循环)
                    Friday (A)
```

### 与 Loop Engineering 对应

| Building Block | 我们的实现 |
|----------------|------------|
| Task Selection | Friday 规划 + 需求分析 |
| Context Preparation | devclaw RSI 上下文组装 |
| Execution | Jarvis 代码生成 |
| Validation | Edith 独立测试 + Friday 验收 |
| Decision | Friday 决定是否发布/回滚 |
| Memory | claw-mem 记忆系统 |

---

## 关键技术栈

### Harness Layer: devclaw RSI

我们的 **devclaw** 提供了完整的 Harness：

- **Tool System**: 多模型路由 (75+ 模型)
- **Memory**: claw-mem 三层记忆系统
- **Sandbox**: 安全执行环境
- **Security**: 权限控制、审计
- **Context Assembly**: 智能上下文组装
- **Skill System**: 可复用技能库

### 通信协议

```
Inbox 协议 (替代 sessions_spawn):
Friday → Jarvis: 写入任务到 inbox-jarvis/
Jarvis → Friday: 回复到 inbox-friday/
```

---

## 实践成果

### 项目优化 (2026-05)

| 项目 | 优化前 | 优化后 | 提升 |
|------|--------|--------|------|
| claw-mem | 4.11.0 | 4.11.1 | -7,100 行死代码 |
| claw-rl | 4.1.0 | 4.1.1 | -3,905 行 |
| neoclaw | 4.6.0 | 4.6.1 | -9,377 行 |
| claw-cog | 4.1.0 | 4.1.1 | -1,356 行 |

### 关键 Metrics

- 任务完成率: Jarvis ~80%, EDITH ~60%
- 代码审查: 每次提交经过 A+B+C 三重检查
- 发布质量: 探索性测试 + 独立验收

---

## 为什么有效？

### 1. 职责清晰
每个人只做自己擅长的事：
- **Friday**: 战略思考 (What & Why)
- **Jarvis**: 战术执行 (How)
- **Edith**: 质量把关 (Correctness)

### 2. 循环反馈
```
规划 → 开发 → 测试 → 验收 → 发布
  ↑                          ↓
  └──────── 反馈循环 ←───────┘
```

### 3. Harness 保障
devclaw 提供了生产级的基础设施，不是"玩具 demo"

---

## 业界对比

| 方案 | 特点 |
|------|------|
| Claude Code / Codex | 单一 Agent，依赖内置循环 |
| Cursor | IDE 集成，但仍是单一 Agent |
| **我们的方案** | **多 Agent 协作 + 明确角色分工 + 生产级 Harness** |

---

## 总结

> **Loop Engineering = 设计循环，而不是写 prompt**

我们的三角色框架：
- 比单一 Agent 更可靠 (独立验收)
- 比纯人工更高效 (自动化循环)
- 比玩具 demo 更生产级 (devclaw Harness)

**这可能是第一个明确将 Loop Engineering 落地的开源框架。**

---

## 扩展阅读

- Addy Osmani X: @addyosmani
- OpenAI Harness Engineering: openai.com/index/harness-engineering
- devclaw: github.com/opensourceclaw/devclaw
- claw-mem: github.com/opensourceclaw/claw-mem

---

*本文是 Project Neo 团队实践分享，欢迎讨论！*
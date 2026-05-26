

## 📖 项目简介

**SmartTrip-Agent** 是一款面向企业差旅场景的智能出行助手，采用 **Plan-and-Execute** 多智能体架构，结合大语言模型的语义理解能力，为用户提供端到端的差旅规划与决策支持服务。

系统通过意图识别、知识库检索、记忆管理等核心模块的协同工作，实现从用户需求理解到行程方案输出的全链路自动化，显著降低差旅管理成本，提升出行体验。

---

## ✨ 核心特性

### 🧠 智能意图理解
- 基于 LLM 语义理解的 **6 大类意图识别**
- 意图识别准确率达 **90%+**
- 支持模糊表达、多轮对话等复杂场景

### 🗂️ 双层记忆架构
- **短期记忆**：Redis 缓存，毫秒级响应，支持会话上下文保持
- **长期记忆**：PostgreSQL 持久化存储，跨会话用户偏好沉淀

### 📚 RAG 知识库检索
- 集成 **Milvus** 向量数据库 + **BGE Embedding** 模型
- 知识库检索准确率 **95%**
- 覆盖交通、住宿、报销政策等差旅核心知识领域

### ⚡ 高性能调度
- 优先级并行调度策略，响应时间从 **30 秒优化至 15 秒**（提升 50%）
- 插件化懒加载架构，系统冷启动仅需 **3 秒**

---

## 🏗️ 系统架构

```
用户请求
   │
   ▼
┌─────────────────────────────────────┐
│         意图识别模块 (6类)            │
│  机票预订 / 酒店预订 / 行程规划 /     │
│  费用报销 / 政策查询 / 通用问答       │
└──────────────┬──────────────────────┘
               │
               ▼
┌─────────────────────────────────────┐
│       Plan-and-Execute 调度器        │
│    任务分解 → 优先级排序 → 并行执行   │
└──────┬──────────────┬───────────────┘
       │              │
       ▼              ▼
┌────────────┐  ┌─────────────────────┐
│  记忆管理  │  │    RAG 知识检索      │
│ Redis (短) │  │ Milvus + BGE Embed  │
│  PG  (长) │  │   准确率 95%         │
└────────────┘  └─────────────────────┘
       │              │
       └──────┬───────┘
              ▼
┌─────────────────────────────────────┐
│         豆包大模型 (LLM)             │
│       推理 / 生成 / 总结             │
└─────────────────────────────────────┘
              │
              ▼
          结构化输出
```

---

## 🛠️ 技术栈

| 类别 | 技术 |
|------|------|
| 多智能体框架 | [AgentScope](https://github.com/modelscope/agentscope) |
| 大语言模型 | 硅基流动|
| 关系型数据库 | PostgreSQL |
| 缓存层 | Redis |
| 向量数据库 | Milvus |
| Embedding 模型 | BGE（BAAI/bge-large-zh）|
| 后端语言 | Python 3.10+ |

---

## 🚀 快速开始

### 环境要求

- Python 3.10+
- Redis 7.x
- PostgreSQL 14+
- Milvus 2.x

### 安装依赖

```bash
git clone https://github.com/your-username/SmartTrip-Agent.git
cd SmartTrip-Agent
pip install -r requirements.txt
```

### 配置环境变量

```bash
cp .env.example .env
```

编辑 `.env` 文件，填写以下配置：

```env
# 大模型 API
DOUBAO_API_KEY=your_api_key
DOUBAO_MODEL=your_model_endpoint

# 数据库
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=smarttrip
POSTGRES_USER=your_user
POSTGRES_PASSWORD=your_password

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

# Milvus
MILVUS_HOST=localhost
MILVUS_PORT=19530
```

### 初始化数据库

```bash
python scripts/init_db.py
python scripts/build_knowledge_base.py
```

### 启动服务

```bash
python main.py
```

---

## 📊 性能指标

| 指标 | 数值 |
|------|------|
| 意图识别准确率 | 90%+ |
| 知识库检索准确率 | 95% |
| 平均响应时间 | ~15 秒 |
| 优化前响应时间 | ~30 秒 |
| 系统冷启动时间 | 3 秒 |

---

## 📁 项目结构

```
SmartTrip-Agent/
├── agents/                 # 智能体模块
│   ├── planner.py          # 任务规划器
│   ├── executor.py         # 执行器
│   └── intent_classifier.py# 意图分类器
├── memory/                 # 记忆模块
│   ├── short_term.py       # Redis 短期记忆
│   └── long_term.py        # PostgreSQL 长期记忆
├── rag/                    # 知识检索模块
│   ├── retriever.py        # 向量检索
│   └── embedder.py         # BGE 向量化
├── plugins/                # 插件（懒加载）
├── scripts/                # 初始化脚本
├── config/                 # 配置文件
├── main.py                 # 入口文件
├── requirements.txt
└── README.md
```

---

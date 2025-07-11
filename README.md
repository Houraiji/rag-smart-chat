# RAG智能文档问答系统

基于LangChain开发的RAG（检索增强生成）系统，支持多轮对话记忆和混合检索策略。

## 功能特点

- **文档处理与向量化**：支持PDF、TXT等多种格式文档的处理和向量化存储
- **混合检索策略**：结合向量相似度(70%)和关键词权重(30%)的混合检索策略
- **多轮对话记忆**：基于Redis实现的对话历史记忆，支持多会话隔离
- **自动摘要**：自动摘要长对话历史，减少token消耗
- **异步处理**：采用异步处理管道，优先查询缓存再触发RAG检索

## 系统架构

```
+-------------------+    +-------------------+    +-------------------+
|                   |    |                   |    |                   |
|   文档处理模块     |    |   混合检索模块     |    |   对话记忆模块     |
|                   |    |                   |    |                   |
+--------+----------+    +--------+----------+    +--------+----------+
         |                        |                        |
         v                        v                        v
+-------------------+    +-------------------+    +-------------------+
|                   |    |                   |    |                   |
|   向量数据库       |    |   问答核心模块     |    |   Redis缓存       |
|   (Chroma)        |    |                   |    |                   |
+-------------------+    +-------------------+    +-------------------+
                                  |
                                  v
                         +-------------------+
                         |                   |
                         |   FastAPI接口     |
                         |                   |
                         +-------------------+
```


配置环境变量

```bash
cp .env.example .env
# 编辑.env文件，填入你的OpenAI API密钥
```

### 运行

```bash
python main.py
```

服务将在 http://localhost:8000 启动。

### 使用Docker

1. 构建并启动容器

```bash
docker-compose up -d
```

## API接口

### 上传文档

```
POST /upload
```

上传PDF、TXT等文档进行处理和索引。

### 问答接口

```
POST /ask
```

发送问题并获取回答，支持多轮对话。

### 会话管理

```
GET /sessions/
DELETE /sessions/{session_id}
```

管理对话会话。

## 性能优化

- 使用Redis缓存对话历史，减少数据库访问
- 混合检索策略提高召回率
- 异步处理文件上传和索引
- 自动摘要长对话历史，减少token消耗

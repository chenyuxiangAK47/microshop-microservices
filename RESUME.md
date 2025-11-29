# 简历项目描述

## 中文版

### Microshop 微服务系统（个人项目）

**项目描述：**
设计并实现了一个基于 Python/FastAPI 的三服务微服务系统（用户服务、商品服务、订单服务），每个服务拥有独立的 PostgreSQL 数据库，实现数据自治。

**核心技术：**
- 使用 FastAPI 构建 RESTful API，SQLAlchemy 进行数据访问
- 每个服务独立数据库（users_db、products_db、orders_db），避免跨服务直接访问数据
- 通过 HTTP 调用实现服务间同步通信，使用 RabbitMQ 实现异步事件驱动
- 订单服务发布订单创建事件，商品服务异步消费事件并扣减库存，体现最终一致性

**工程化实践：**
- 使用 Docker Compose 一键启动完整微服务环境（3 个业务服务 + PostgreSQL + RabbitMQ + Redis）
- 为跨服务 HTTP 调用添加容错机制：超时保护（3 秒）、自动重试（最多 3 次）、结构化日志
- 区分业务错误（404）和系统错误（500/超时），返回合适的 HTTP 状态码（400 vs 503）
- 结构化日志记录每次服务间调用的详细信息（URL、耗时、状态码），便于快速定位问题

**技术亮点：**
- **数据自治**：每个服务只访问自己的数据库，通过 API 和事件进行服务间通信
- **事件驱动**：订单创建后异步扣减库存，提高响应速度，支持最终一致性
- **容错设计**：网络抖动时自动重试，下游服务不可用时返回 503，避免请求被卡住
- **可观测性**：日志记录调用链路，便于调试和监控

**GitHub：** https://github.com/chenyuxiangAK47/microshop-microservices

---

## English Version

### Microshop Microservices – Python Microservices Demo (Personal Project)

**Project Description:**
Designed and implemented a 3-service microservice system (user-service, product-service, order-service) using FastAPI and PostgreSQL, each with its own database to enforce data autonomy.

**Core Technologies:**
- Built RESTful APIs with FastAPI and SQLAlchemy for data access
- Each service has an independent PostgreSQL database (users_db, products_db, orders_db), preventing direct cross-service data access
- Implemented synchronous inter-service communication via HTTP calls and asynchronous event-driven architecture using RabbitMQ
- Order service publishes order-created events; product service asynchronously consumes events to update stock, demonstrating eventual consistency

**Engineering Practices:**
- Containerized all services (APIs, databases, RabbitMQ, Redis) and orchestrated them with Docker Compose for one-command local deployment
- Added resilience patterns to cross-service HTTP calls: timeout protection (3s), automatic retries (max 3 attempts), structured logging
- Distinguished business errors (404) from system errors (500/timeout), returning appropriate HTTP status codes (400 vs 503)
- Structured logging records detailed information for each inter-service call (URL, duration, status code) for quick troubleshooting

**Key Highlights:**
- **Data Autonomy**: Each service only accesses its own database, communicating via APIs and events
- **Event-Driven**: Asynchronous stock deduction after order creation, improving response time and supporting eventual consistency
- **Fault Tolerance**: Automatic retries on network jitter, returns 503 when downstream services are unavailable, preventing request blocking
- **Observability**: Logging records call chains for debugging and monitoring

**GitHub:** https://github.com/chenyuxiangAK47/microshop-microservices

---

## 面试话术（3 分钟版本）

### 一句话定位
"这是一个为了作品集做的电商微服务 demo，用 Python + FastAPI 拆成用户、商品、订单三个服务，每个服务有自己的数据库，通过 HTTP + RabbitMQ 通信。"

### 为什么做成微服务（而不是一个大 monolith）
- 想练习数据自治、服务间 API 调用、事件驱动
- 每个服务可以独立开发与部署，边界清晰
- 符合微服务"单一职责"原则

### 一次下单到底发生了什么？（用故事讲流程）
1. **客户端调用订单服务**：`POST /api/orders`
2. **订单服务同步校验**：
   - HTTP 调用用户服务确认用户存在（带超时、重试）
   - HTTP 调用商品服务看库存够不够（带超时、重试）
3. **订单服务写入本地数据库**：`orders_db`
4. **订单服务发布事件**：通过 RabbitMQ 发布 `ORDER_CREATED` 事件
5. **商品服务异步消费**：监听 RabbitMQ，收到事件后给对应商品减库存

### 你特别想 highlight 的点
- **容错设计**：跨服务调用时的超时 + 重试，网络抖动时自动恢复
- **事件驱动**：订单创建和库存扣减是异步的，提高响应速度
- **数据自治**：每个服务只访问自己的数据库，通过 API/事件交流
- **可观测性**：日志记录调用链路、耗时、状态码，方便 debug
- **Docker Compose**：一键启动所有依赖，方便本地开发和演示

---

## LinkedIn 描述（简短版）

**Microshop Microservices** – Python/FastAPI microservices demo showcasing:
- 3 services with independent databases (data autonomy)
- Event-driven architecture with RabbitMQ (eventual consistency)
- Fault-tolerant HTTP calls (timeouts, retries, structured logging)
- Docker Compose orchestration

Built to practice microservices patterns and engineering best practices.

🔗 https://github.com/chenyuxiangAK47/microshop-microservices

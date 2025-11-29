# 简历项目描述 - Microshop Microservices

## 中文版本

### 项目名称
Microshop Microservices - Python 微服务架构演示项目

### 项目描述（简历用，约 150 字）

设计并实现了一个基于 FastAPI 的 3 服务微服务系统（用户服务、商品服务、订单服务），每个服务拥有独立的 PostgreSQL 数据库，实现数据自治。

构建了事件驱动的订单流程：订单服务通过 RabbitMQ 发布订单创建事件，商品服务异步消费事件并扣减库存，体现了最终一致性设计。

使用 Docker Compose 容器化并编排所有服务（API、数据库、RabbitMQ、Redis），实现一键本地部署。

为跨服务 HTTP 调用添加了容错机制（超时、重试、结构化日志），提高了系统的容错性和可调试性。

### 技术栈
- **后端框架**：FastAPI、SQLAlchemy
- **数据库**：PostgreSQL（每个服务独立数据库）
- **消息队列**：RabbitMQ（事件驱动）
- **容器化**：Docker、Docker Compose
- **Python 库**：httpx（HTTP 客户端）、tenacity（重试）、pika（RabbitMQ）

### 核心亮点
1. **微服务数据自治**：每个服务只访问自己的数据库，通过 API/事件通信
2. **事件驱动架构**：订单创建和库存扣减异步处理，提高系统响应速度
3. **容错设计**：HTTP 调用带超时、重试、错误分类，提高系统健壮性
4. **可观测性**：结构化日志记录调用耗时、状态码，便于问题定位

---

## English Version

### Project Name
Microshop Microservices - Python Microservices Architecture Demo

### Project Description (Resume, ~150 words)

Designed and implemented a 3-service microservice system (user-service, product-service, order-service) using FastAPI and PostgreSQL, each with its own database to enforce data autonomy.

Built an event-driven order flow: order service publishes order-created events to RabbitMQ, and product service asynchronously consumes events to update stock, demonstrating eventual consistency.

Containerized all services (APIs, databases, RabbitMQ, Redis) and orchestrated them with Docker Compose for one-command local deployment.

Added resilience patterns (timeouts, retries, structured logging) to cross-service HTTP calls, improving fault tolerance and debuggability.

### Tech Stack
- **Backend Framework**: FastAPI, SQLAlchemy
- **Database**: PostgreSQL (database per service)
- **Message Queue**: RabbitMQ (event-driven)
- **Containerization**: Docker, Docker Compose
- **Python Libraries**: httpx (HTTP client), tenacity (retry), pika (RabbitMQ)

### Key Highlights
1. **Microservice Data Autonomy**: Each service owns its database, communicates via API/events
2. **Event-Driven Architecture**: Asynchronous order creation and stock deduction, improving response time
3. **Fault Tolerance**: HTTP calls with timeouts, retries, and error classification for robustness
4. **Observability**: Structured logging with call duration and status codes for quick troubleshooting

---

## LinkedIn 版本（英文，更简洁）

**Microshop Microservices** | Python Microservices Demo

Built a 3-service microservice system (user, product, order) with FastAPI, each with its own PostgreSQL database. Implemented event-driven architecture using RabbitMQ for asynchronous stock updates. Added resilience patterns (timeouts, retries, logging) to cross-service calls. Containerized with Docker Compose for easy local deployment.

**Tech**: FastAPI, PostgreSQL, RabbitMQ, Docker, Python

---

## 面试话术（3 分钟版本）

### 1. 一句话定位
"这是一个为了作品集做的电商微服务 demo，用 Python + FastAPI 拆成用户、商品、订单三个服务，每个服务有自己的数据库，通过 HTTP + RabbitMQ 通信。"

### 2. 为什么做成微服务（而不是一个大 monolith）
- **数据自治**：每个服务只访问自己的数据库，避免跨服务直接访问数据
- **独立部署**：可以单独升级、扩容某个服务，不影响其他服务
- **技术栈灵活**：未来可以给不同服务选择不同的技术栈

### 3. 一次下单到底发生了什么？（用故事讲流程）
1. **客户端调用订单服务**：`POST /api/orders`
2. **订单服务同步校验**：
   - HTTP 调用用户服务验证用户存在（带超时、重试）
   - HTTP 调用商品服务验证库存充足（带超时、重试）
3. **订单服务写入本地数据库**：`orders_db`
4. **订单服务发布事件**：通过 RabbitMQ 发布 `ORDER_CREATED` 事件
5. **商品服务异步消费**：监听 RabbitMQ，收到事件后扣减库存

**为什么这样设计？**
- **解耦**：订单服务不需要等待库存扣减完成，提高响应速度
- **最终一致性**：即使商品服务暂时不可用，订单已创建，库存稍后扣减

### 4. 你特别想 highlight 的点
- **容错设计**：HTTP 调用带超时（3 秒）、自动重试（最多 3 次）、错误分类（404 vs 500 vs 超时）
- **可观测性**：结构化日志记录每次调用的 URL、耗时、状态码，便于快速定位问题
- **Docker Compose**：一键启动所有依赖（数据库、消息队列、业务服务）

---

## GitHub 项目链接

https://github.com/chenyuxiangAK47/microshop-microservices


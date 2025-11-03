# 🚀 System Design Patterns

<div align="center">

```
  ____            _                   ____            _             
 / ___| _   _ ___| |_ ___ _ __ ___   |  _ \  ___  ___(_) __ _ _ __  
 \___ \| | | / __| __/ _ \ '_ ` _ \  | | | |/ _ \/ __| |/ _` | '_ \ 
  ___) | |_| \__ \ ||  __/ | | | | | | |_| |  __/\__ \ | (_| | | | |
 |____/ \__, |___/\__\___|_| |_| |_| |____/ \___||___/_|\__, |_| |_|
        |___/                                            |___/        
```

**Build Scalable, Reliable, and High-Performance Systems**

[![Scalability](https://img.shields.io/badge/Scalability-Patterns-green?style=for-the-badge)](.)
[![Reliability](https://img.shields.io/badge/Reliability-Patterns-blue?style=for-the-badge)](.)
[![Performance](https://img.shields.io/badge/Performance-Patterns-orange?style=for-the-badge)](.)

</div>

---

## 📖 What is System Design?

System Design is the process of defining the **architecture**, **components**, **modules**, **interfaces**, and **data** for a system to satisfy specified requirements.

### Why Learn System Design?

- ✅ **Build Scalable Systems**: Handle millions of users
- ✅ **High Availability**: Keep systems running 24/7
- ✅ **Performance**: Fast response times
- ✅ **Career Growth**: Essential for senior/architect roles
- ✅ **Interview Success**: Common in tech interviews

---

## 🎯 Core Concepts

```mermaid
mindmap
  root((System<br/>Design))
    Scalability
      Horizontal Scaling
      Vertical Scaling
      Load Balancing
      Sharding
    Reliability
      Redundancy
      Replication
      Failover
      Backup
    Performance
      Caching
      CDN
      Database Optimization
      Async Processing
    Security
      Authentication
      Authorization
      Encryption
      Rate Limiting
    Data
      SQL vs NoSQL
      CAP Theorem
      ACID vs BASE
      Data Partitioning
```

---

## 🏗️ System Design Patterns

### 1. Load Balancing

**Problem**: Single server can't handle all traffic
**Solution**: Distribute requests across multiple servers

```
Client → Load Balancer → Server 1
                      → Server 2
                      → Server 3
```

**Algorithms**:
- Round Robin
- Least Connections
- IP Hash
- Weighted Round Robin

---

### 2. Caching

**Problem**: Database queries are slow
**Solution**: Store frequently accessed data in memory

```
Client → Cache (Redis/Memcached) → Database
         ↓ Hit                      ↑ Miss
      Response                   Fetch & Cache
```

**Strategies**:
- Cache-Aside
- Write-Through
- Write-Behind
- Refresh-Ahead

---

### 3. Database Sharding

**Problem**: Single database can't handle all data
**Solution**: Split data across multiple databases

```
User ID 1-1000    → Shard 1
User ID 1001-2000 → Shard 2
User ID 2001-3000 → Shard 3
```

---

### 4. Microservices

**Problem**: Monolithic app is hard to scale
**Solution**: Break into independent services

```
Monolith → User Service
        → Order Service
        → Payment Service
        → Notification Service
```

---

### 5. Message Queue

**Problem**: Synchronous processing is slow
**Solution**: Async processing with queues

```
Producer → Queue (RabbitMQ/Kafka) → Consumer
```

---

## 📊 System Architecture Patterns

```mermaid
graph TB
    subgraph Client Layer
        Web[Web Browser]
        Mobile[Mobile App]
    end
    
    subgraph CDN Layer
        CDN[CDN<br/>Static Content]
    end
    
    subgraph Load Balancer
        LB[Load Balancer<br/>Nginx/HAProxy]
    end
    
    subgraph Application Layer
        API1[API Server 1]
        API2[API Server 2]
        API3[API Server 3]
    end
    
    subgraph Cache Layer
        Redis[Redis Cache]
    end
    
    subgraph Database Layer
        Master[(Master DB<br/>Write)]
        Slave1[(Slave DB 1<br/>Read)]
        Slave2[(Slave DB 2<br/>Read)]
    end
    
    subgraph Message Queue
        MQ[Message Queue<br/>Kafka/RabbitMQ]
    end
    
    subgraph Worker Layer
        W1[Worker 1]
        W2[Worker 2]
    end
    
    subgraph Storage
        S3[Object Storage<br/>S3/Blob]
    end
    
    Web --> CDN
    Mobile --> CDN
    Web --> LB
    Mobile --> LB
    
    LB --> API1
    LB --> API2
    LB --> API3
    
    API1 --> Redis
    API2 --> Redis
    API3 --> Redis
    
    Redis -.->|Miss| Master
    API1 --> Master
    API2 --> Slave1
    API3 --> Slave2
    
    Master -.->|Replicate| Slave1
    Master -.->|Replicate| Slave2
    
    API1 --> MQ
    MQ --> W1
    MQ --> W2
    
    W1 --> S3
    W2 --> S3
    
    style Client fill:#ff6b6b,stroke:#c92a2a,stroke-width:2px
    style CDN fill:#4ecdc4,stroke:#0a8a8a,stroke-width:2px
    style LB fill:#ffd43b,stroke:#fab005,stroke-width:2px
    style Application fill:#74b9ff,stroke:#0984e3,stroke-width:2px
    style Cache fill:#a29bfe,stroke:#6c5ce7,stroke-width:2px
    style Database fill:#55efc4,stroke:#00b894,stroke-width:2px
    style Message fill:#fd79a8,stroke:#e84393,stroke-width:2px
    style Worker fill:#fdcb6e,stroke:#e17055,stroke-width:2px
    style Storage fill:#81ecec,stroke:#00b894,stroke-width:2px
```

---

## 🔄 Data Flow Patterns

### Read-Heavy System

```mermaid
sequenceDiagram
    participant C as Client
    participant LB as Load Balancer
    participant API as API Server
    participant Cache as Redis Cache
    participant DB as Database
    
    C->>LB: Request Data
    LB->>API: Forward Request
    API->>Cache: Check Cache
    alt Cache Hit
        Cache-->>API: Return Data
        API-->>C: Response (Fast)
    else Cache Miss
        Cache-->>API: Not Found
        API->>DB: Query Database
        DB-->>API: Return Data
        API->>Cache: Update Cache
        API-->>C: Response
    end
```

### Write-Heavy System

```mermaid
sequenceDiagram
    participant C as Client
    participant API as API Server
    participant MQ as Message Queue
    participant Worker as Worker
    participant DB as Database
    
    C->>API: Write Request
    API->>MQ: Enqueue Task
    API-->>C: Accepted (202)
    
    MQ->>Worker: Dequeue Task
    Worker->>DB: Write Data
    DB-->>Worker: Success
    Worker->>MQ: Acknowledge
```

---

## 🎯 Design Principles

### CAP Theorem

```
Pick 2 out of 3:
┌─────────────┐
│ Consistency │ ← All nodes see same data
├─────────────┤
│ Availability│ ← System always responds
├─────────────┤
│ Partition   │ ← Works despite network failures
│ Tolerance   │
└─────────────┘
```

### ACID vs BASE

| ACID (SQL) | BASE (NoSQL) |
|------------|--------------|
| Atomicity | Basically Available |
| Consistency | Soft state |
| Isolation | Eventually consistent |
| Durability | - |

---

## 📈 Scalability Patterns

```mermaid
graph LR
    A[Traffic Growth] --> B{Scale?}
    
    B -->|Vertical| C[Bigger Server<br/>More CPU/RAM]
    B -->|Horizontal| D[More Servers<br/>Load Balancer]
    
    C --> E[Limits:<br/>Hardware Max]
    D --> F[Unlimited:<br/>Add More Servers]
    
    style A fill:#ff6b6b,stroke:#c92a2a,stroke-width:2px,color:#fff
    style C fill:#ffd43b,stroke:#fab005,stroke-width:2px
    style D fill:#51cf66,stroke:#2f9e44,stroke-width:2px
    style E fill:#ff8787,stroke:#fa5252,stroke-width:2px
    style F fill:#51cf66,stroke:#2f9e44,stroke-width:2px
```

---

## 🛠️ Technology Stack

| Layer | Technologies |
|-------|-------------|
| **Load Balancer** | Nginx, HAProxy, AWS ELB |
| **Cache** | Redis, Memcached |
| **Database** | MySQL, PostgreSQL, MongoDB, Cassandra |
| **Message Queue** | Kafka, RabbitMQ, AWS SQS |
| **Search** | Elasticsearch, Solr |
| **Storage** | AWS S3, Azure Blob, Google Cloud Storage |
| **CDN** | Cloudflare, AWS CloudFront, Akamai |
| **Monitoring** | Prometheus, Grafana, ELK Stack |

---

## 📚 Common System Design Questions

1. **Design URL Shortener** (like bit.ly)
2. **Design Twitter** (social media feed)
3. **Design Instagram** (photo sharing)
4. **Design Uber** (ride-sharing)
5. **Design Netflix** (video streaming)
6. **Design WhatsApp** (messaging)
7. **Design YouTube** (video platform)
8. **Design Amazon** (e-commerce)

---

## 🚀 Learning Path

```mermaid
graph TB
    A[Start] --> B[Learn Basics<br/>Scalability, Availability]
    B --> C[Study Components<br/>LB, Cache, DB]
    C --> D[Understand Patterns<br/>Sharding, Replication]
    D --> E[Practice Designs<br/>Real Systems]
    E --> F[Mock Interviews]
    F --> G[System Design<br/>Expert]
    
    style A fill:#ff6b6b,stroke:#c92a2a,stroke-width:2px,color:#fff
    style G fill:#00d2d3,stroke:#01a3a4,stroke-width:3px,color:#fff
```

---

<div align="center">

**🔜 Coming Soon: Detailed system design examples!**

[![Back to Main](https://img.shields.io/badge/←_Back_to-Main_README-blue?style=for-the-badge)](../README.md)

**Happy Designing! 🚀**

</div>

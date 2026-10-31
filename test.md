# System Design Documentation

---

## Table of Contents

- [Core System Concepts](#core-system-concepts)
  - [What is System Design?](#what-is-system-design)
  - [What is High-Level Structure?](#what-is-high-level-structure)
  - [What is Architecture?](#what-is-architecture)
  - [Single Responsibility](#single-responsibility)
- [System Requirements & Quality Attributes](#system-requirements--quality-attributes)
  - [What is Scalability?](#what-is-scalability)
  - [What is Reliability and Availability?](#what-is-reliability-and-availability)
  - [What is Performance?](#what-is-performance)
  - [What is Security?](#what-is-security)
- [Speed & Performance Metrics](#speed--performance-metrics)
  - [Speed Overview](#speed-overview)
  - [What is Latency?](#what-is-latency)
  - [What is Throughput?](#what-is-throughput)
- [Communication & Networking](#communication--networking)
  - [What is Communication?](#what-is-communication)
  - [What is Client and Server?](#what-is-client-and-server)
  - [What is Protocol?](#what-is-protocol)
  - [What is Web Server?](#what-is-web-server)
  - [What is Application Server?](#what-is-application-server)
- [Software Architectural Patterns](#software-architectural-patterns)
  - [Layered Architecture](#layered-architecture)
  - [Client-Server Architecture](#client-server-architecture)
  - [Monolith Architecture](#monolith-architecture)
  - [Microservices Architecture](#microservices-architecture)
  - [Event-Driven Architecture](#event-driven-architecture)
- [Execution & Processing Models](#execution--processing-models)
  - [Process vs Thread vs RAM](#process-vs-thread-vs-ram)
  - [Concurrency](#concurrency)
  - [Asynchronous Processing](#asynchronous-processing)
  - [What is Message Queue?](#what-is-message-queue)
- [Traffic Control & System Reliability](#traffic-control--system-reliability)
  - [Traffic Control Overview](#traffic-control-overview)
  - [What is Load Balancing?](#what-is-load-balancing)
  - [What is Throttling?](#what-is-throttling)
  - [Circuit Breaker](#circuit-breaker)
  - [Priority-Based Routing](#priority-based-routing)
  - [What is Redundancy?](#what-is-redundancy)
  - [What is Caching?](#what-is-caching)
- [Database Scaling & Capacity](#database-scaling--capacity)
  - [Database Scaling Overview](#database-scaling-overview)
  - [Vertical Scaling](#vertical-scaling)
  - [Horizontal Scaling](#horizontal-scaling)
  - [Sharding](#sharding)
  - [Replication](#replication)
  - [Indexing](#indexing)

---

## Core System Concepts

### What is System Design?
- System design is the process of defining how a software system as a whole will be structured so it can meet its requirements

### What is High-Level Structure?
- It means the big picture view of a system where the main building blocks and how they connect, without going into the code-level details

### What is Architecture?
- It is the overall blueprint of a system, what ist main parts are, what each part is responsible for and how they talk to each other

### Single Responsibility
Single Responsibility is a design principle: each component (class, function, service, or agent) should do one job and have only one reason to change.

#### Example (RAG System)
- **Bad:** One giant function that parses documents, chunks them, creates embeddings, searches the vector DB, builds the prompt, and calls the LLM. A change to any step risks breaking the others.
- **Good:** Separate components, each with one job:
  - **Retriever:** searches the vector DB
  - **Prompt builder:** assembles context and question
  - **Generator:** calls the LLM
  - **Ingestion pipeline:** parses, chunks, and embeds documents

#### Application in Multi-Agent Systems
Instead of one “do everything” agent, use specialized agents: a research agent, a coding agent, a reviewer agent. Each has a focused prompt and toolset, which makes it easier to test, debug, and improve.

> **Restaurant Analogy:** The grill station only grills and the dessert station only makes dessert. Mixing them makes both messy.

#### Benefits
- Easier to test and debug
- Change one part without breaking others
- Reuse components (swap the vector DB without touching the LLM code)

*Note: It’s the “S” in the SOLID principles of software design.*

---

## System Requirements & Quality Attributes

### What is Scalability?
- The system handles growth from 1,000 users to 10 million, through load balancers, caching, and horizontal scaling

### What is Reliability and Availability?
- Keeping the system running when servers crash, using redundancy, failover, and backups.

### What is Performance?
- It is meeting latency and throughput targets.

### What is Security?
- Authentication, authorization, and data protection.

---

## Speed & Performance Metrics

### Speed Overview
- Latency
- Throughput

### What is Latency?
- It is the time delay between a request being made and the response being received.
- Lower latency means a faster, smoother experience.
- When you click a link and the page takes 200 milliseconds to start loading, the latency is 200 ms.

### What is Throughput?
- It is the amount of work a system can handle in a given time, such as requests per second or data transferred per second.
- If a server processes 1,000 requests every second, its throughput is 1,000 requests/sec.

---

## Communication & Networking

### What is Communication?

### What is Client and Server?
A client is the side that sends requests. A server is the side that receives them, does the work, and sends back a response.

#### Example (RAG System)
- **Client:** the chat UI in your browser or mobile app. The user types a question and it sends the request.
- **Server:** your backend, which embeds the query, searches the vector database, calls the LLM, and returns the answer. 

**Flow:** `Client` $\rightarrow$ `request` $\rightarrow$ `Server` $\rightarrow$ `processes` $\rightarrow$ `response` $\rightarrow$ `Client`

### What is Protocol?
A protocol is a set of rules that defines how two systems communicate: the format of messages, the order of exchange, and how to handle errors.

#### Example (RAG System)
Your frontend sends a question to the backend over HTTP. The backend calls the LLM provider’s API, also over HTTP. Both sides follow the agreed rules, so each knows how to read the other’s messages.

#### Common Protocols
- **HTTP/HTTPS:** standard request-response communication between client and server (REST APIs)
- **WebSocket:** a persistent two-way connection, useful for real-time chat
- **SSE (Server-Sent Events):** server streams data to the client, commonly used to stream LLM tokens as they’re generated
- **gRPC:** fast communication between internal services, such as between agent workers and model servers
- **MCP (Model Context Protocol):** a standard way for AI agents to connect to tools and data sources
- **A2A (Agent2Agent):** a protocol for agents to communicate with each other in multi-agent systems

### What is Web Server?
A web server is software that receives HTTP requests from clients and sends back responses. It’s the front door of your backend: it handles incoming traffic and passes requests to your application logic.

#### Example (RAG System)
The user’s chat UI sends a question to `/api/chat`. The web server receives it, hands it to your app code (which does retrieval and calls the LLM), then returns the answer to the user.

#### Core Responsibilities
- Accepts and routes HTTP requests
- Serves static files (HTML, JS, CSS)
- Handles HTTPS/TLS, authentication checks, and rate limiting
- Streams responses (e.g., LLM tokens via SSE)

### What is Application Server?
An application server is the part of your backend that runs your business logic: the code that decides what to do with a request and produces the result.

#### Example (RAG System)
The web server passes the user’s question to your application server (a FastAPI or Express app). It then:
1. Embeds the query
2. Searches the vector database
3. Builds the prompt with retrieved context
4. Calls the LLM
5. Returns the answer

#### In Agent Systems
The application server is where the agent loop runs: deciding which tool to call, calling it, and combining results into a final response.

#### Web Server vs. Application Server
- **Web Server:** Handles raw HTTP traffic (routing, TLS, static files).
- **Application Server:** Handles what to do with the request (business logic).
*(Many frameworks, like FastAPI with Uvicorn, combine both roles).*

#### Common Examples
- **Python:** FastAPI, Flask, Django
- **Node.js:** Express, NestJS
- **Java:** Spring Boot

---

## Software Architectural Patterns

### Layered Architecture
- Layered Architecture

### Client-Server Architecture
Client-server architecture is a design where the system is split into two roles: clients that send requests, and servers that process them and send back responses. The server holds the logic and data; the client is the interface.

#### Example (RAG System)
- **Client:** chat UI in the browser or mobile app
- **Server:** backend that embeds the query, searches the vector DB, calls the LLM, and returns the answer
- *Note:* The client never talks to the vector DB or LLM directly; it only talks to your server.

#### In AI Systems
- **Agent + tool servers:** an AI agent (client) calls tools exposed by MCP servers
- **App + model server:** your backend (client) sends prompts to an LLM inference server (e.g., vLLM) or a provider API
- **Multi-agent systems:** one agent can act as a client to another agent that acts as the server

### Monolith Architecture
A monolith is an application built as one codebase and deployed as one single unit, where all features run together in the same process.

#### Example (RAG System)
One FastAPI app contains everything: document ingestion, retrieval, prompt building, LLM calls, auth, and chat history. You deploy it as one thing, and if you change any part, you redeploy the whole app.

#### In AI Systems
- A single script or app that runs the whole agent loop, retrieval, and generation
- Most prototypes and early-stage RAG apps start this way

> **Restaurant Analogy:** One kitchen where one team does everything: grill, pasta, dessert, all in the same room.

### Microservices Architecture
Microservices architecture is designing a system as a set of small, independent services, each handling one specific job, deployed separately, and communicating over APIs or messages.

#### Example (RAG System)
- **Ingestion service:** parses, chunks, and embeds documents
- **Retrieval service:** searches the vector DB
- **Generation service:** builds prompts and calls the LLM
- **Auth service:** handles users and permissions
- **Chat history service:** stores conversations

Each runs on its own, so you can scale, update, or restart one without touching the others.

#### In AI Systems
- **Model serving:** the LLM or embedding model runs as its own service on GPU machines, scaled separately from the CPU-based app servers
- **Multi-agent systems:** each agent (research, coding, reviewer) can be its own service, talking via A2A or queues
- **Tool servers:** each tool exposed as a separate service via MCP

#### Key Relationships
- Microservices apply **Single Responsibility** at the service level, making horizontal scaling easier.
- **Client-server architecture** defines *who talks to whom* (client requests, server responds). **Microservices** defines *how the server side is built* (many small, independent services instead of one big unit).
- **Monolith vs. Microservices:** A monolith is one unit (simple, but harder to scale parts independently); microservices split it into independent services (flexible, but more complex). Common advice: start with a monolith, split into microservices when real scaling or team needs appear.

### Event-Driven Architecture
Event-driven architecture is a design where components communicate by producing and reacting to events (something happened) instead of calling each other directly. A producer emits an event, and any interested consumer picks it up and acts on it, usually through a message broker.

#### Example (RAG System)
A user uploads a PDF. The upload service publishes a `document.uploaded` event. Separate consumers react on their own:
- **Parser service:** extracts text, then publishes `document.parsed`
- **Embedding service:** chunks and embeds it, then publishes `document.embedded`
- **Indexer service:** writes vectors to the vector DB
- **Notification service:** tells the user “your document is ready”

*(The upload service doesn’t know or care who is listening).*

#### In AI Systems
- **Ingestion pipelines:** each stage triggered by the previous stage’s event
- **Multi-agent systems:** agents publish results (`research.completed`) and other agents react, instead of a central controller calling each one
- **Long-running agent tasks:** queued as events and processed by background workers
- **Re-indexing:** a `source.updated` event triggers re-embedding of changed documents

#### Common Tools
Kafka, RabbitMQ, AWS SQS/SNS, Redis Streams.

#### Benefits
- **Loose coupling:** services don’t depend on each other directly
- **Easy to scale:** add more consumers for a busy event type
- **Resilience:** if a consumer is down, events wait in the queue
- **Easy to extend:** add a new consumer without changing the producer

*Event-driven vs. Client-server:* Client-server is request-then-immediate-response (caller waits). Event-driven is fire-an-event-and-move-on (producer doesn’t wait). It’s often used together with microservices.

---

## Execution & Processing Models

### Process vs Thread vs RAM
- **Process:** An independent running program with its own memory.
- **Thread:** A smaller unit of execution inside a process that shares the process’s memory with other threads.

#### Example (RAG System)
Your FastAPI app runs as a process. Inside it, threads handle individual user requests. They all share the same loaded embedding model and cache in memory, so nothing is duplicated.

### Concurrency
Concurrency is a system’s ability to handle multiple tasks at the same time, by overlapping their execution so that one task doesn’t have to finish before another starts.

#### Example (RAG System)
Your chatbot gets 100 users asking questions at once. While one request waits for the LLM to respond, the server doesn’t sit idle. It starts processing other users’ requests. Each request spends most of its time waiting on I/O (vector DB, LLM API), so one server can juggle many of them.

#### In Agent Systems
A multi-agent system can run a research agent, a code agent, and a search agent concurrently, then combine their results, instead of running them one after another.

#### Common Ways to Achieve It
- **Async/await:** a single thread switches between tasks while they wait on I/O (e.g., Python asyncio in FastAPI)
- **Threads:** multiple threads run tasks within one process
- **Multiple processes/workers:** several copies of the app run side by side
- **Message queues:** background workers pick up tasks like document embedding

*Concurrency vs. Parallelism:* Concurrency is managing many tasks at once (possibly interleaved on one core); parallelism is actually running them at the same instant on multiple cores or machines.

### Asynchronous Processing
Asynchronous processing means starting a task and moving on without waiting for it to finish. The result is handled later, when it’s ready.

#### Synchronous vs. Asynchronous
- **Synchronous:** You send a request and wait until the response comes back. Nothing else happens in that flow meanwhile.
- **Asynchronous:** You start the task, keep doing other work, and get notified (or check back) when it’s done.

#### Example (RAG System)
A user uploads a 500-page PDF. Parsing, chunking, and embedding it takes 3 minutes.
- **Sync:** The user’s browser hangs for 3 minutes waiting.
- **Async:** The server instantly replies “received, processing,” puts the job in a queue, and a background worker handles it. The user gets notified when it’s done, or checks a status endpoint.

#### In AI Systems
- **Async code (async/await):** one server handles hundreds of in-flight LLM and vector DB calls without blocking on each
- **Background workers:** document ingestion, batch embedding, re-indexing
- **Parallel agent calls:** a multi-agent system fires off research, search, and code agents at once, then gathers results
- **Long-running agent tasks:** the user submits a task and gets the result later

### What is Message Queue?
A message queue is a buffer between a sender and a receiver. The sender (producer) puts a message in the queue and moves on. The receiver (consumer) picks it up and processes it when ready. Messages are usually handled in order, and each one is processed by one consumer.

#### Example (RAG System)
Users upload 200 PDFs at once. The upload service drops 200 “process this document” messages into a queue. A pool of 5 embedding workers pulls messages one by one and processes them at a safe pace. If a worker crashes, the message goes back in the queue and another worker picks it up.

#### In AI Systems
- **Ingestion pipelines:** queue documents for parsing, chunking, and embedding
- **LLM request buffering:** hold requests during spikes so you stay within provider rate limits (works with throttling)
- **Agent task queues:** workers pull tasks like “research this topic” or “run this tool”
- **Multi-agent systems:** agents hand work to each other through queues

#### Other Ways to Get Async (Without a Queue)
- **async/await:** one server handles many in-flight LLM and vector DB calls without blocking
- **Threads or background tasks:** run work in the same app, but jobs are lost if the app crashes
- **Callbacks or webhooks:** the other system notifies you when it’s done

#### Common Tools
RabbitMQ, AWS SQS, Redis queues, Celery (task queue), Kafka.

> **Key Takeaway:** Asynchronous processing is the *goal* (don’t wait for the task). A message queue is a *tool* that achieves it reliably.

---

## Traffic Control & System Reliability

### Traffic Control Overview
- Load Balancing
- Circuit Breaker
- Rate-Based Throttling
- Queue-based processing
- Priority-Based Routing

### What is Load Balancing?
Load balancing is distributing incoming requests across multiple servers so no single server gets overloaded. 

#### Example (RAG System)
Your chatbot gets 500 queries per second, but one LLM server can only handle 100. You run 5 LLM servers and put a load balancer in front. It sends each request to a server that has capacity, so users get fast answers.

#### In AI Systems
- Spreading requests across multiple LLM or embedding model servers
- Distributing traffic across several agent workers
- Routing across API providers or API keys to avoid rate limits

#### Benefits
Higher throughput, lower latency, and better reliability, since if one server crashes the others keep serving.

### What is Throttling?
Throttling is deliberately limiting how many requests a client or system can make in a given time, to protect the system from being overloaded.

#### Example (RAG System)
Your chatbot allows each user 10 requests per minute. If one user (or a buggy script) sends 500 per minute, the server throttles them and returns an HTTP `429 Too Many Requests` error, so the LLM servers stay available for everyone else.

*Throttling vs. Load Balancing:* Load balancing spreads traffic across servers; throttling caps how much traffic is allowed in.

### Circuit Breaker
A circuit breaker is a protection mechanism that stops calling a failing service for a while, instead of repeatedly hitting it and making things worse. It fails fast and gives the broken service time to recover.

#### Three States
1. **Closed:** Everything is normal, requests flow through.
2. **Open:** Too many failures happened, so requests are blocked immediately and a fallback is returned.
3. **Half-open:** After a cooldown, a few test requests are allowed. If they succeed, the breaker closes; if not, it opens again.

#### Example (RAG System)
Your primary LLM provider starts returning errors or timing out. Without a breaker, every user request waits 30 seconds, then fails, and your servers pile up with stuck requests. With a breaker, after 5 failures it opens, and requests instantly switch to a backup LLM provider or return a friendly “try again shortly” message.

> **Restaurant Analogy:** If the oven breaks, the kitchen stops taking oven orders and tells customers right away, instead of letting tickets pile up. Every so often, a cook checks whether the oven is fixed.

#### In AI Systems
- **LLM provider outages:** switch to a fallback model or cached answer
- **Vector DB or reranker down:** skip reranking or return a degraded answer
- **Agent tool calls:** stop an agent from repeatedly calling a broken tool (and burning tokens in a retry loop)
- **Multi-agent systems:** prevent one failing agent from stalling every agent waiting on it (cascading failure)

#### Benefits
- Prevents cascading failures
- Faster failure, so users aren’t left hanging
- Saves cost and resources
- Gives the failing service room to recover

*Circuit Breaker vs. Retry:* Retries try again hoping it works; a circuit breaker knows when to stop trying. They’re often combined: retry a few times with backoff, and if failures keep happening, open the breaker.

### Priority-Based Routing
- Priority-Based Routing

### What is Redundancy?
Redundancy is keeping extra copies of components so that if one fails, another takes over and the system keeps running.

#### Example (RAG System)
You run 3 copies of your app server, 2 vector database replicas, and 2 LLM providers. If one server crashes or the primary LLM API goes down, traffic shifts to the backups and users notice nothing.

#### What You Can Make Redundant in AI Systems
- **App servers:** multiple instances behind a load balancer
- **Vector database:** replicas of your index, so a node failure doesn’t lose search
- **LLM providers:** a fallback model (e.g., switch to a secondary provider if the first returns errors)
- **Data:** backups of your documents and embeddings
- **Agent state:** conversation memory stored in a shared database, so another worker can resume if one dies

#### Trade-off
More cost and complexity, since you pay for extra machines and must keep copies in sync.

### What is Caching?
Caching is storing the result of an expensive operation so that next time the same request comes in, you return the saved result instead of recomputing it.

#### Benefits & Trade-offs
- **Benefits:** lower latency, lower API cost, less load on the LLM and database.
- **Trade-off:** cached data can go stale, so you set an expiry time (TTL) or clear the cache when the source documents change.

---

## Database Scaling & Capacity

### Database Scaling Overview
- Sharding
- Replication
- Indexing

### Vertical Scaling
Vertical scaling (scaling up) means making a single machine more powerful by adding more CPU, RAM, or a bigger GPU, instead of adding more machines.

#### Example (RAG System)
Your app server is slow because the embedding model and traffic are too much for it. You upgrade it from 4 cores / 16 GB RAM to 16 cores / 64 GB RAM. Same one server, just bigger.

#### In AI Systems
- Moving your self-hosted LLM from a 24 GB GPU to an 80 GB GPU so a larger model fits
- Giving the vector DB node more RAM so the whole index stays in memory
- Upgrading a single agent worker to handle heavier tasks

### Horizontal Scaling
Horizontal scaling (scaling out) means adding more machines to share the load, instead of making one machine bigger.

#### Example (RAG System)
Your app server can handle 50 requests/sec, but you need 200. Instead of upgrading it, you run 4 identical app servers behind a load balancer, and each takes a share of the traffic.

#### In AI Systems
- Multiple LLM inference servers (GPU nodes) serving requests in parallel
- Vector DB split across nodes using sharding, with replicas for reads
- Many agent workers pulling tasks from a queue
- Autoscaling: Kubernetes adds more app instances when traffic spikes and removes them when it drops

*Vertical vs. Horizontal Scaling:* Vertical makes one machine bigger; horizontal adds more machines behind a load balancer. Most systems start vertical for simplicity, then go horizontal as they grow.

### Sharding
Sharding is splitting a large dataset into smaller pieces (shards), each stored on a different server, so no single machine has to hold or handle everything.

#### Example (RAG System)
You have 100 million document chunks and their embeddings. One vector database node can’t hold them or search them fast enough. So you split them into 5 shards of 20 million each, on 5 nodes. A query is sent to all shards, each returns its top matches, and the results are merged into the final top results.

#### Benefits
Scales storage and throughput beyond one machine, and each shard is faster to search.

### Replication
- Replication

### Indexing
- Indexing

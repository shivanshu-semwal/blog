---
title: "A Complete Guide to System Design: Learning Path, Resources, and Interview Preparation"
layout: post
tags:
  - system-design
  - distributed-systems
  - software-engineering
  - interview-preparation
description: "A detailed learning roadmap for system design interviews and real-world architecture, including networking, databases, streaming, cloud services, and serverless systems."
---

System design is the study of turning product requirements into a reliable, scalable, maintainable system. A system-design interview is less about drawing boxes and more about demonstrating how you reason about tradeoffs. There is rarely one correct answer.

A strong answer usually follows this process:

1. Clarify the requirements and constraints.
2. Estimate traffic, storage, and latency.
3. Define the APIs and core data flows.
4. Choose a basic architecture.
5. Identify bottlenecks and failure modes.
6. Add caching, partitioning, replication, queues, and observability.
7. Explain tradeoffs and how the design would evolve.

This article provides a learning roadmap for both interviews and real-world engineering. It brings together the areas that are often taught separately: software engineering, operating systems, networking, databases, distributed systems, cloud infrastructure, media streaming, and operational practices.

## What System Design Means

System design has multiple levels:

- **Application design** describes classes, modules, interfaces, and request lifecycles inside one application.
- **Infrastructure design** describes servers, containers, networks, load balancers, databases, caches, and managed cloud services.
- **Distributed-systems design** describes systems made of independent components that communicate over unreliable networks.
- **Data-system design** describes how data is ingested, stored, indexed, transformed, replicated, and queried.
- **Operational design** describes deployment, monitoring, incident response, disaster recovery, and cost.

Most interview prompts combine these levels. For example, "Design YouTube" is not only a storage problem. It requires upload processing, content delivery, metadata, search, recommendations, authorization, analytics, moderation, and handling failures at each stage.

## Prerequisites

You do not need to learn every distributed-systems topic before starting, but several fundamentals make system design much easier.

### Programming and Application Fundamentals

Be comfortable with at least one backend language and its web framework. Know how to:

- Implement HTTP APIs.
- Parse and validate JSON.
- Use concurrency and asynchronous operations.
- Work with files and streams.
- Handle retries, timeouts, cancellation, and backpressure.
- Log useful request context.
- Write tests at unit, integration, and end-to-end levels.

The language matters less than understanding that application code spends much of its time waiting for network calls, disks, queues, and databases.

### Operating Systems

Review these topics:

- Processes and threads
- Virtual memory
- Filesystems
- File descriptors
- Scheduling
- Synchronization, locks, and atomics
- CPU, memory, disk, and network bottlenecks
- Containers and Linux namespaces

Many scaling problems become easier to diagnose when you know whether work is blocked on CPU, memory, disk I/O, or network I/O.

### Networking

Networking is the foundation of distributed systems. Learn the following terms rather than only memorizing protocol acronyms:

| Concept | What to understand |
| --- | --- |
| DNS | Resolves names to addresses and can return policies such as weighted or geographic routing |
| TCP | Provides a reliable ordered byte stream over an unreliable network |
| UDP | Sends datagrams without TCP-style delivery guarantees |
| TLS | Encrypts connections and authenticates endpoints |
| HTTP/1.1 | Simple request/response protocol with connection reuse |
| HTTP/2 | Multiplexed streams over one connection with header compression |
| HTTP/3 | Runs HTTP over QUIC, which uses UDP |
| QUIC | Reduces connection setup latency and isolates lost streams |
| WebSocket | Provides a persistent bidirectional connection |
| gRPC | RPC framework based on HTTP/2, protocol buffers, and typed interfaces |
| Load balancing | Distributes traffic across instances at DNS, network, or application layers |
| Content delivery networks | Cache and deliver content from locations near users |

Understanding [latency](https://hpbn.co/) is especially valuable. A packet can be lost, a DNS lookup can fail, a connection pool can be exhausted, or a server behind a load balancer can become slow. Systems must treat every remote call as fallible.

### Databases

Study the basic tradeoffs between:

- SQL and NoSQL
- Relational transactions and eventual consistency
- Row-oriented and column-oriented storage
- OLTP and OLAP
- Primary/replica databases
- Partitioning and replication
- Indexes and query plans
- Point reads, range scans, and full-text search

You do not need to memorize every database implementation, but you should be able to explain why a relational database is a poor fit for a particular workload and why a specialized store might help.

## Core Distributed-Systems Concepts

### Availability and Reliability

**Availability** is the fraction of time a system successfully serves requests. It is not enough to say "we use replication."

- Define availability in measurable terms.
- Decide whether reads, writes, uploads, or all operations must remain available.
- Distinguish average uptime from degraded behavior.
- Measure success rates and latency with observability tools.
- Plan for partial failure.

Reliability means the system continues to perform correctly within agreed conditions. Reliability is strengthened by retries only when operations are safe to retry, and by avoiding retries that multiply load during an outage.

### Latency and Throughput

Latency is the time required to complete an operation. Throughput is the amount of work completed per unit of time.

For each operation, identify:

- Median latency
- Tail latency, such as p95 or p99
- Upload and download sizes
- Read-to-write ratio
- Peak traffic
- Regional and seasonal spikes

Back-of-the-envelope estimates should use round numbers and state assumptions. For example, a service receiving 10,000 requests per second may need to handle 30,000 during a peak. The exact answer matters less than showing how you derived it.

### Horizontal and Vertical Scaling

**Vertical scaling** adds CPU, memory, or faster storage to one machine. It is simple but has an upper limit.

**Horizontal scaling** adds more machines. It is more flexible but introduces load distribution, state management, and consistency questions.

A stateless API tier can scale horizontally behind a load balancer. A stateful database is harder because copies must agree on writes and recover safely from failures.

### Consistency Models

Distributed replicas may not immediately agree.

- **Strong consistency** gives operations a well-defined global order.
- **Linearizability** makes a read appear to observe the latest completed write.
- **Read-after-write consistency** guarantees that a writer sees their own update.
- **Session consistency** maintains consistency within a user's session.
- **Eventual consistency** allows replicas to converge after an initial disagreement.

The right model depends on the product. A bank transfer may require stronger guarantees. A view counter or recommendation feed may tolerate a delay. System designs should identify what must be strongly consistent and what can converge later.

### CAP and the Reality of Tradeoffs

The CAP theorem is sometimes summarized as "choose two," but that framing hides the real distinction. In the presence of a network partition, a system must choose either availability or strong consistency for affected operations.

A useful design discussion asks:

- Which operations can continue during a partition?
- Which operations should reject requests or queue them?
- How are conflicts resolved after connectivity returns?
- Can users observe stale or duplicate records?

For many applications, the important answer is not "consistency vs. availability" but "which consistency guarantees do we need for this data?"

### Replication

Replication improves availability and read capacity. Common models include:

- **Primary-replica:** Writes go to one primary; replicas serve reads or become primaries after failover.
- **Leaderless:** Writes can go to several replicas, with quorum rules determining success.
- **Multi-leader:** Several regions accept writes, requiring conflict resolution.
- **Synchronous:** A write waits for replicas to confirm it.
- **Asynchronous:** A primary acknowledges writes before replicas catch up.

Replication does not create a correct backup. A backup must survive accidental deletes and data corruption, and it must be tested with recovery exercises.

### Partitioning

Partitioning, or sharding, divides data across machines. Partition keys determine which records live together and how evenly work is distributed.

A good partition key:

- Supports the system's common queries.
- Distributes traffic relatively evenly.
- Avoids permanently placing a hot tenant or date range on one partition.
- Allows records that are read together to be colocated.

A bad partition key causes hotspots. `country=US` may be a meaningful field but a weak partition key if it receives most of the traffic. Hashing can distribute writes but may make range queries or sequential reads more difficult.

### Caching

Caching reduces latency and database load. Typical cache patterns include:

- **Cache-aside:** The application reads the cache, then loads and stores missing entries.
- **Read-through:** The cache provider loads missing values.
- **Write-through:** Writes update cache and storage together.
- **Write-back:** Writes update cache first and storage asynchronously.

Important concerns include eviction, TTL, stale data, cache stampedes, thundering herds, and cache invalidation. Cache invalidation is difficult because it includes both stale values and inconsistent update ordering.

A cache should make a fast path more efficient, not become the system's only source of truth unless the data has explicitly been designed that way.

## Communication Patterns

### Synchronous HTTP and RPC

Synchronous calls are easy to understand, but they multiply latency and can propagate failures. Every call needs:

- A bounded timeout
- Retry rules
- An identity or request ID for tracing
- Circuit breaking or overload protection
- A clear error contract

Retries are appropriate for transient failures on idempotent operations. They are dangerous for operations whose outcome is unknown, for example a payment request that timed out after the payment may have succeeded.

### Message Queues

Queues decouple producers and consumers. They smooth traffic spikes and allow work to continue when downstream systems are unavailable.

A queue does not guarantee exactly-once work by itself. Consumers should use:

- **Idempotency keys** for operations that must not repeat
- **Acknowledgments** only after processing succeeds
- **Dead-letter queues** for invalid or repeatedly failing work
- **Backpressure** for consumers that cannot keep up
- **Ordering strategies** appropriate to the application

Examples include Kafka, RabbitMQ, Amazon SQS, Google Pub/Sub, Azure Service Bus, Redis Streams, and managed equivalents. Kafka-style distributed logs are often useful for replay and event-driven processing. Traditional queues are often better for task distribution.

### Events and Streams

An event is a fact or signal describing something that happened. Streaming platforms can process these events continuously or in bounded windows.

Use events when:

- Several independent services need to react to the same action.
- Processing can happen asynchronously.
- Data must be replayed or rebuilt.
- Multiple consumers need different views of the same facts.

Be careful not to turn every database update into an ambiguous event. Define the schema, ownership, delivery semantics, retention, and replay behavior.

### Search and Analytics

Search engines provide inverted indexes, ranking, filtering, and proximity queries. Analytics engines are often column-oriented and optimized for aggregation over large datasets.

Both systems commonly consume changes from the operational database. This introduces a synchronization concern: the database, search index, and analytics warehouse may not update atomically. Designing a durable change log and reconciliation process is often more reliable than duplicating writes directly.

## Storage and Data Modeling

Choose storage by access pattern, not fashion.

Ask:

- Which queries must be fast?
- Which data must be durable?
- How large is the object or record?
- How often is it written?
- How often is it read?
- Which fields need indexes?
- How long is data retained?
- Which region or jurisdiction owns the data?

### Blob and Object Storage

Object storage is ideal for large immutable files such as images, videos, backups, exports, and attachments. Objects are addressed by a key and stored independently of application servers.

For media, it is common to upload directly to object storage using a pre-signed URL. This avoids sending large files through application servers. The server instead creates authorization, receives upload completion events, and starts processing.

### Transactional Data

Relational databases are useful for complex records, constraints, transactions, and relationships. They are often the best default for core business state.

Document databases can be useful for records with flexible structure, while key-value stores are useful for caches and simple, high-throughput lookups. The correct choice depends on access patterns and operations that require atomic updates.

### Data Warehouses and Lakehouses

Operational systems record facts needed to run applications. Analytical systems aggregate those facts for reporting and machine learning.

Common patterns include:

- Change Data Capture
- Event logs
- Batch ETL or ELT
- Data warehouses
- Data lakes
- Lakehouse storage formats
- Stream processing

Analytics is commonly designed for high-throughput append operations and reads across many records, rather than small transactional updates.

## Common System-Design Building Blocks

### API Gateway

An API gateway or edge proxy can handle routing, authentication, TLS termination, rate limiting, request validation, and observability. It can also become a bottleneck, so its size and failure behavior must be included in the design.

### Load Balancer

Load balancers distribute traffic across servers. They may operate with layer 4 TCP routing or layer 7 HTTP routing.

Health checks must distinguish "the process is alive" from "the service can process requests." A shallow health check can keep broken instances in service, while a deep check can remove them unnecessarily.

### Stateless Services

Stateless services keep no required user or session state in local memory. This makes them easier to replace and scale.

Session state can live in a cache or database. Local memory can still optimize reads, but it should not contain the only copy of required state.

### Content Delivery Networks

A CDN caches content near users. It reduces origin traffic and latency for static and media content.

Cache behavior should define:

- Cache-control headers
- Object versioning
- Geographic replication
- Cache invalidation
- Signed or expiring URLs

For immutable objects, including the content hash in the file name is one of the simplest cache strategies.

### Rate Limiting

Rate limits protect services and enforce quotas. They can be based on user ID, API key, IP address, tenant, endpoint, or a combination.

Distributed rate limiting often uses a cache or specialized limiter service. Sliding windows provide smoother behavior but require more storage than fixed windows. Limits must have clear responses and headers so clients know how to retry.

### Idempotency

An operation is idempotent if repeating it has the same intended result. HTTP `GET`, `PUT`, and `DELETE` are commonly designed as idempotent operations, although actual implementations vary.

For payment, upload, and delivery workflows, clients can send an idempotency key. The server records the key and original result so retries return the previous outcome rather than create duplicate work.

### Observability

A production design needs:

- **Metrics** for rates, latency, errors, saturation, and queues
- **Structured logs** for high-cardinality request context
- **Distributed traces** for following calls across services
- **Alerts** based on user impact
- **Dashboards** that show the component and dependency state

Avoid alert fatigue. An alert should indicate that a human needs to act and include a useful runbook or initial diagnosis.

## Microservices, Modular Monoliths, and Serverless

Microservices are not automatically more scalable or reliable. They add network calls, deployment units, schema contracts, security boundaries, and distributed failures.

### Modular Monolith

A modular monolith keeps modules in one deployable process while enforcing clear internal boundaries. It is often the best starting point because it reduces operational and networking complexity.

### Microservices

Microservices are useful when parts of a system have different scaling, availability, ownership, or release requirements. A social network may have separate services for posts, authentication, relationships, notifications, search, and feeds.

Each service should own its data and expose a deliberate interface. Directly sharing a database across services creates coupling and makes independent evolution difficult.

### Serverless

Serverless functions run code without an application team managing server instances continuously. The provider handles scheduling and infrastructure scaling.

Serverless is useful for:

- Event processing
- Scheduled jobs
- Webhooks
- Image or document transformation
- Lightweight APIs
- Glue between managed services

It has tradeoffs:

- Cold starts
- Short execution limits
- Limited control of networking and runtime
- Hidden costs for high-throughput sustained workloads
- Harder debugging across providers
- Vendor-specific APIs

Serverless is an execution model, not an architectural free pass. It still requires idempotency, observability, security boundaries, timeouts, and data ownership.

### Containers

Containers provide repeatable application packaging. Kubernetes or another orchestrator can schedule containers, roll out deployments, restart failed instances, and manage service discovery.

Managed container platforms are often a reasonable middle ground between virtual machines and serverless functions. They provide more runtime control than functions while reducing operating-system management.

## Media-Streaming Architecture

Platforms such as YouTube, Twitch, and Netflix share important patterns, but their delivery models differ.

### Video-on-Demand

A typical video pipeline is:

1. The client requests an upload.
2. The service creates an object key and signed upload URL.
3. The video is stored in object storage.
4. A queue triggers transcoding.
5. Encoders produce multiple resolutions and codecs.
6. The output is split into segments.
7. A manifest describes available formats and segments.
8. A CDN caches and serves segments near viewers.
9. Metadata and processing state are stored in a database.

**HTTP Live Streaming (HLS)** uses manifests and media segments. It works through standard HTTP/CDNs and is supported broadly across Apple and many web platforms.

**Dynamic Adaptive Streaming over HTTP (DASH)** is an adaptive streaming standard using an MPD manifest and segments. It is codec-independent and commonly used by browsers and applications.

Most streaming systems support adaptive bitrate selection: the player chooses a segment variant based on bandwidth and device capability. Lower-quality segments require less bandwidth, while higher-quality segments improve the viewing experience.

### Live Streaming

Live delivery is a lower-latency pipeline:

- An encoder sends video using protocols such as RTMP, SRT, or WebRTC.
- An ingest service validates the stream and identifies the channel.
- The stream is transcoded into multiple qualities.
- Segments are published to storage or an origin.
- CDNs distribute the stream.
- Players consume HLS or DASH, or WebRTC for very low-latency interactive use.

Protocol choice depends on the requirement:

| Protocol | Typical role |
| --- | --- |
| RTMP | Common encoder-to-ingest protocol, especially in older workflows |
| SRT | Resilient contribution transport over unreliable networks |
| WebRTC | Interactive real-time streaming with sub-second latency |
| HLS/DASH | Playback through HTTP and CDNs |

For conventional live streams, HTTP-based HLS or DASH may add several seconds of latency. WebRTC can reduce latency but is harder to scale through standard CDN cache models and is commonly used for calls and real-time interactions.

### Media Processing

Video processing is computationally expensive and has bursty demand. A queue-based architecture is useful:

```text
Upload -> Object Storage -> Processing Queue -> Workers
                                      |
                                      +-> Transcode
                                      +-> Generate thumbnails
                                      +-> Extract captions
                                      +-> Detect unsafe content
                                      +-> Create analytics events
```

Workers should be separately scalable from API servers. Object keys can make a failed job retryable. Every stage should record progress and allow resumable or idempotent processing.

### Metadata and Recommendations

Video files are not the only data. The system stores creators, titles, descriptions, visibility, licenses, comments, watch history, playback sessions, recommendations, and moderation decisions.

Recommendations are usually a separate data and machine-learning pipeline. Offline systems process watch and engagement data, generate models or candidate indexes, and serve a low-latency retrieval layer. They also need constraints for relevance, freshness, abuse, and policy.

## Worked Example: URL Shortener

A URL shortener demonstrates many distributed-systems concepts in a small design.

### Requirements

- Create short links.
- Redirect short links to the original URL.
- Links may expire.
- Resolve links quickly.
- Provide basic usage metrics.
- Support custom links and abuse prevention.

### Data Model

The central mapping can contain:

```text
short_key
long_url
owner_id
created_at
expires_at
status
```

A relational database or key-value store can store this mapping. A generated short key can be a base-62 encoding of a unique numeric ID or a random token with collision checks.

### Request Flow

```text
Client -> API -> Database
             -> Redirect Cache
```

The read path is simple:

1. Extract the short key.
2. Check the cache.
3. On a miss, read the database.
4. Return a 301 or 302 redirect.
5. Enforce expiration and authorization rules.

A 301 redirect can be cached heavily by browsers, making analytics and changes harder. A 302 is easier to control but may move every request through the application.

### Scaling

- Cache popular mappings.
- Partition the database by short key.
- Use read replicas or a distributed key-value store.
- Use object or queue systems for analytics events.
- Separate abuse detection from redirects.
- Add regional serving for latency.

The hard part is not the redirect. It is choosing key generation, collision handling, lifecycle behavior, observability, and abuse controls.

## Worked Example: News Feed

A feed is useful for comparing fan-out strategies.

### Fan-out on Write

When a post is created:

1. Store the post.
2. Add its ID to each follower's precomputed feed.

This makes reads fast but can require enormous write fan-out for popular accounts. It also needs feed eviction and ranking updates.

### Fan-out on Read

When a follower opens the feed:

1. Retrieve followed account IDs.
2. Fetch recent posts.
3. Merge and rank them.

Reads are more expensive, but normal accounts avoid work until someone opens the feed.

### Hybrid Approach

Many large feeds use a hybrid:

- Precompute feeds for normal users.
- Fetch high-follower and live posts separately.
- Merge and rank the results.
- Cache the final or intermediate feed.
- Rebuild feeds from durable post data after failures.

The design must define ordering, stale posts, deleted content, privacy changes, and ranking. A cache copy cannot preserve authorization if permissions change.

## Security and Privacy

System design must include security from the beginning.

- Encrypt secrets and rotate credentials.
- Use identity-based service authentication.
- Apply least-privilege access.
- Validate and constrain user input.
- Use authorization checks on every resource.
- Encrypt traffic in transit.
- Encrypt sensitive stored data.
- Audit administrative and data-access operations.
- Minimize retention of personal data.
- Encrypt backups and test restoration.
- Plan for compromised endpoints and leaked credentials.

Security failures should be modeled as system failures. For example, a CDN cache must not serve private objects, and a queue containing personal data must have a retention and access policy.

## Capacity Estimation

Practice converting a product requirement into numbers.

If 1 million users are active per day and each user makes 20 requests, daily traffic is 20 million requests. Dividing by 86,400 seconds gives roughly 232 requests per second on average.

A peak-to-average ratio of 5 gives approximately 1,160 requests per second. Storage, replication, and cache factors should be added explicitly.

For a media service, estimate:

- Concurrent viewers
- Average video duration
- Storage per minute
- Transcoding CPU
- Segment count
- CDN egress
- Peak upload volume

The goal is not exact prediction. The goal is to reveal the largest cost or bottleneck and show how it changes when traffic increases.

## Disaster Recovery

Every design should discuss:

- Recovery point objective: how much data loss is acceptable
- Recovery time objective: how long recovery may take
- Regional outages
- Database corruption
- Accidental deletion
- Provider account failures
- Key and secret loss
- Data restore procedures
- Failover testing

An asynchronous replica may reduce loss, but it can still lose recent writes during a disaster. A backup may preserve data but take hours to restore.

Regular game days and restore tests are more valuable than claims that a system is "highly available."

## Essential Books

### Designing Data-Intensive Applications

Martin Kleppmann's [*Designing Data-Intensive Applications*](https://dataintensive.net/) is one of the best resources for data models, transactions, replication, partitioning, stream processing, and distributed-system concepts.

Read it as a conceptual book, not a checklist. Its greatest value is helping you ask which guarantees a database or storage system actually provides.

### System Design Interview

Alex Xu's [*System Design Interview*](https://www.amazon.com/System-Design-Interview-insiders-Second/dp/B08CMF2CQF) is popular for structured examples. Use it to practice organization and estimation. Do not memorize answers. The interview will probably change assumptions or add a bottleneck.

Completing the example should lead to independent exercises:

- Remove a component.
- Change the read/write ratio.
- Make a region unavailable.
- Add a new consistency requirement.
- Double the traffic.
- Reduce the storage budget.

## More Excellent Resources

- [High Performance Browser Networking](https://hpbn.co/) by Ilya Grigorik
- [The TCP/IP Guide](https://www.tcpipguide.com/) for networking fundamentals
- [The Kubernetes documentation](https://kubernetes.io/docs/) for containers and orchestration concepts
- [Kafka documentation](https://kafka.apache.org/documentation/) for distributed logs and stream processing
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [Google Cloud Architecture Center](https://cloud.google.com/architecture)
- [Azure Architecture Center](https://learn.microsoft.com/en-us/azure/architecture/)
- [Google SRE Workbook](https://sre.google/workbook/table-of-contents/)
- [Google SRE Book](https://sre.google/sre-book/table-of-contents/)
- [Site Reliability Engineering topics](https://sre.google/)
- [The Twelve-Factor App](https://12factor.net/)
- [RFC 9110: HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110)
- [WebRTC overview](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API)
- [HLS overview](https://developer.apple.com/streaming/)
- [MPEG-DASH overview](https://www.bitmovin.com/dash-video-streaming-guide/)

Cloud-specific documentation is useful, but learn the underlying concept before the managed-service name. It is easier to understand DynamoDB, Cosmos DB, and Firestore after learning partitioning and consistency.

## Interview Framework

Use this structure when answering an interview question.

### 1. Clarify

Ask about:

- Core users and operations
- Read/write ratio
- Scale
- Data retention
- Geographic coverage
- Consistency requirements
- Availability requirements
- Security or privacy constraints

Do not ask every possible question. Ask the assumptions that materially change the design.

### 2. Estimate

Calculate at least:

- Requests per second
- Storage growth
- Bandwidth
- Cache size
- Database partition count

Use rough calculations and state unit conversions.

### 3. Define APIs

Give only the operations needed for the core workflow. Examples:

```text
POST /uploads
GET /videos/{id}
POST /comments
GET /feed
POST /events
```

Describe authentication and important response codes.

### 4. Draw the Architecture

Start with a few components:

```text
Clients
  -> DNS / CDN
  -> Load Balancer
  -> API Services
  -> Database / Cache / Queue
```

Separate data and control planes if the system is complex.

### 5. Explain the Data Model

For each important record, explain:

- Primary key
- Partition key
- Indexes
- Replication
- Consistency
- Retention

### 6. Identify the Bottleneck

Name it explicitly:

- "Writes to the primary database are the bottleneck."
- "Popular feeds are the hot-key problem."
- "Object storage is cheap, but CDN egress is the major cost."
- "Transcoding capacity will be bursty."

Then explain the mitigation.

### 7. Discuss Failures

Address:

- Instance failure
- Database failure
- Queue failure
- Cache failure
- Region failure
- Slow dependency
- Retries and duplicate events

### 8. Iterate

The interviewer may add constraints. Be ready to move components or weaken guarantees. A strong answer adapts rather than defending a diagram.

## Practice Topics

Do not only practice the famous prompts. Train each building block independently.

### Core interview practice

- URL shortener
- Rate limiter
- Notification system
- Search autocomplete
- Chat application
- News feed
- Pastebin
- Web crawler
- Distributed lock
- Unique ID generator
- Metrics system
- Key-value store

### Storage and streaming practice

- Image-sharing service
- Video-on-demand platform
- Live-streaming platform
- File synchronization service
- Object upload service
- Log aggregation
- Analytics pipeline
- Location tracking
- Ride matching
- Stock exchange feed

For every prompt, write down the assumptions and compare at least two architectures.

## A Practical 12-Week Learning Plan

### Weeks 1-2: Foundations

- HTTP, DNS, TCP, TLS, and load balancing
- Threads, processes, I/O, and application backpressure
- SQL, indexes, transactions, and query plans

Build a small API and place a cache and database behind it.

### Weeks 3-4: Data

- Replication and partitioning
- Consistency models
- Object storage
- Queues and streams

Build a producer and a consumer that handle duplicates safely.

### Weeks 5-6: Reliability

- Retries and idempotency
- Circuit breakers
- Timeouts
- Metrics and tracing
- Failure testing

Inject latency and failures into a local system.

### Weeks 7-8: Cloud and Operations

- Managed databases
- Load balancing
- Containers
- Serverless functions
- Infrastructure as code

Deploy a small service and examine logs, metrics, and cost.

### Weeks 9-10: Specialized Systems

- Search
- Analytics
- Real-time messaging
- Media ingest and delivery
- CDN behavior

Implement a segment-based fake streaming pipeline or a message fan-out system.

### Weeks 11-12: Interview Practice

- Practice one design per day.
- Set a 35-minute timer.
- State assumptions and estimates.
- Draw the smallest useful architecture.
- Explain failure modes and scaling changes.
- Review answers aloud.

Use feedback from mock interviews. Correctness is less important than whether the interviewer can follow your reasoning and see that you understand tradeoffs.

## Final Checklist

Before finishing a system-design answer, confirm that it includes:

- Functional and non-functional requirements
- Traffic and storage estimates
- APIs and data model
- Request flow
- Replication and partitioning
- Caching
- Queues or events where useful
- Consistency and availability decisions
- Security and privacy
- Monitoring and alerting
- Bottleneck and failure analysis
- Cost and operational implications

System design becomes much easier when you stop searching for a universal architecture. Instead, learn the guarantees and costs of individual components. A CDN is not always a cache; a queue is not always a database; serverless is not always cheaper; and replication is not always availability.

The strongest designs are explicit: they define assumptions, explain which guarantees matter, identify the bottleneck, and describe how the architecture can evolve.

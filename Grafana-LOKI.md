## What is Loki ?
- Grafana Loki is a set of components that can be composed into a fully featured logging stack.
- Unlike other logging systems, Loki is built around the idea of only indexing metadata about your logs: labels (just like Prometheus labels). Log data itself is then compressed and stored in chunks in object stores such as Amazon Simple Storage Service (S3) or Google Cloud Storage (GCS), or even locally on the filesystem
- A small index and highly compressed chunks simplifies the operation and significantly lowers the cost of Loki.
- **Loki is a horizontally-scalable, highly-available, multi-tenant log aggregation system inspired by Prometheus. Loki differs from Prometheus by focusing on logs instead of metrics, and collecting logs via push, instead of pull**
- Loki is designed to be very cost effective and highly scalable. Unlike other logging systems, Loki does not index the contents of the logs, but only indexes metadata about your logs as a set of labels for each log stream

## What are the Loki features ?
1. Scalability
2. Multi-tenancy
3. Third-party integrations
4. Efficient storage
5. LogQL, Loki’s query language
6. Alerting
7. Grafana integration

## Loki Architecture
<img width="750" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/8e33f305-07e3-45bf-922b-49a1ee60b12c">

## What are the components of Loki ?
1. Distributor
    - Validation
    - Pre-Processing
    - Rate Limiting
    - Forwarding
    - Hashing
    - Quorum Consistency
2. Ingestor
    - TimeStamp Ordering
    - Handoff
    - FileSystem Support
3. Query Frontend
    - Queueing
    - Splitting
    - Caching
4. Query Scheduler
5. Querier
6. Index Gateway
7. Compactor
8. Ruler
9. Bloom Compactor
10. Bloom Gateway

## What are the LOKI Deployment modes ?
1. Simple Scalable
2. Monolithic Mode
3. Microservices Mode

# LogQL: Log query language
## What is LogQL ?
- LogQL is Grafana Loki’s PromQL-inspired query language. Queries act as if they are a distributed `grep` to aggregate log sources. LogQL uses labels and operators for filtering.
- There are two types of LogQL queries:
  1. `Log queries` return the contents of log lines.
  2. `Metric queries` extend log queries to calculate values based on query results.

1. Binary Operators
    <img width="815" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/fcc3e025-8dc9-4090-b930-44d13f107678">
    <img width="811" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/aef1703e-91b5-4ff5-8935-d6c5dbce6151">
    <img width="801" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/90f79797-c97b-46e7-b8c2-f23fa6ba6674">
    <img width="805" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/26cfdae2-e868-4b32-ad22-0ca75406b2ae">
    







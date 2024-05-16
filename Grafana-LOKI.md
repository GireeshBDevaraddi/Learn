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



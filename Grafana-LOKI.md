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

## Log Queries
- All LogQL queries contain a log stream selector.
  <img width="765" alt="image" src="https://github.com/GireeshBDevaraddi/Learn/assets/135546164/1d77a990-4313-42d5-9491-b74c0b8bcd32">
- A log pipeline can be appended to a log stream selector to further process and filter log streams. It is composed of a set of expressions. Each expression is executed in left to right sequence for each log line. If an expression filters out a log line, the pipeline will stop processing the current log line and start processing the next log line.
- Line filter expressions support stripping ANSI sequences (color codes) from the line: `{job="example"} | decolorize`
- Label filter expressions are the only expression allowed after the unwrap expression. This is mainly to allow filtering errors from the metric extraction.
- Parser expression can parse and extract labels from the log content. Those extracted labels can then be used for filtering using `label filter expressions` or `for metric aggregations`.
- It’s easier to use the predefined parsers `json` and `logfmt` when you can. If you can’t, the `pattern` and `regexp` parsers can be used for log lines with an unusual structure
- Regex Expression which extract implicitly all values and takes no parameters, the regexp parser takes a single parameter `| regexp "<re>"` which is the regular expression using the Golang RE2 syntax.
- The line format expression can rewrite the log line content by using the text/template format. It takes a single string parameter `| line_format "{{.label_name}}"`, which is the template format. All labels are injected variables into the template and are available to use with the {{.label_name}} notation.
- The `| label_format` expression can rename, modify or add labels. It takes as parameter a comma separated list of equality operations, enabling multiple operations at once
- The `| drop` expression will drop the given labels in the pipeline.
- The `| keep` expression will keep only the specified labels in the pipeline and drop all the other labels.

## Metric Queries
- Metric queries extend log queries by applying a function to log query results. This powerful feature creates metrics from logs.
- Loki supports two types of range vector aggregations: `log range aggregations` and `unwrapped range aggregations`.

### Log range aggregations
- A log range aggregation is a query followed by a duration. A function is applied to aggregate the query over the duration. The duration can be placed after the log stream selector or at end of the log pipeline.
- `rate(log-range)` : calculates the number of entries per second
- `count_over_time(log-range)` : counts the entries for each log stream within the given range.
- `bytes_rate(log-range)` : calculates the number of bytes per second for each stream.
- `bytes_over_time(log-range)` : counts the amount of bytes used by each log stream for a given range.
- `absent_over_time(log-range)` : returns an empty vector if the range vector passed to it has any elements and a 1-element vector with the value 1 if the range vector passed to it has no elements

ex: `count_over_time({job="mysql"}[5m])`
- The offset modifier allows changing the time offset for individual range vectors in a query
ex: `count_over_time({job="mysql"}[5m] offset 5m)`

### Unwrapped range aggregations
- Unwrapped ranges uses extracted labels as sample values instead of log lines. However to select which label will be used within the aggregation, the log query must end with an unwrap expression and optionally a label filter expression to discard errors.
- The unwrap expression is noted `| unwrap label_identifier` where the label identifier is the label name to use for extracting sample values
- Supported function for operating over unwrapped ranges are:
    - `rate(unwrapped-range)` : calculates per second rate of the sum of all values in the specified interval.
    - `rate_counter(unwrapped-range)` : calculates per second rate of the values in the specified interval and treating them as “counter metric”
    - `sum_over_time(unwrapped-range)` : the sum of all values in the specified interval.
    - `avg_over_time(unwrapped-range)` : the average value of all points in the specified interval.
    - `max_over_time(unwrapped-range)` : the maximum value of all points in the specified interval.
    - `min_over_time(unwrapped-range)` : the minimum value of all points in the specified interval
    - `first_over_time(unwrapped-range)`: the first value of all points in the specified interval
    - `last_over_time(unwrapped-range)`: the last value of all points in the specified interval
    - `stdvar_over_time(unwrapped-range)`: the population standard variance of the values in the specified interval.
    - `stddev_over_time(unwrapped-range)`: the population standard deviation of the values in the specified interval.
    - `quantile_over_time(scalar,unwrapped-range)`: the φ-quantile (0 ≤ φ ≤ 1) of the values in the specified interval.
    - `absent_over_time(unwrapped-range)`: returns an empty vector if the range vector passed to it has any elements and a 1-element vector with the value 1 if the range vector passed to it has no elements
 
- Except for `sum_over_time`,`absent_over_time`, `rate` and `rate_counter`, unwrapped range aggregations support grouping. Which can be used to aggregate over distinct labels dimensions by including a `without` or `by` clause.
  - `without` removes the listed labels from the result vector, while all other labels are preserved the output.
  - `by` does the opposite and drops labels that are not listed in the by clause, even if their label values are identical between all elements of the vector

## Built-in aggregation operators
- LogQL supports a subset of built-in aggregation operators that can be used to aggregate the element of a single vector, resulting in a new vector of fewer elements but with aggregated values
    - `sum`: Calculate sum over labels
    - `avg`: Calculate the average over labels
    - `min`: Select minimum over labels
    - `max`: Select maximum over labels
    - `stddev`: Calculate the population standard deviation over labels
    - `stdvar`: Calculate the population standard variance over labels
    - `count`: Count number of elements in the vector
    - `topk`: Select largest k elements by sample value
    - `bottomk`: Select smallest k elements by sample value
    - `sort`: returns vector elements sorted by their sample values, in ascending order.
    - `sort_desc`: Same as sort, but sorts in descending order
      
- [ ] [Template Functions](https://grafana.com/docs/loki/latest/query/template_functions/)
- 

    







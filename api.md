# List of API metrics

 Certainly! Here's an updated table grouping the metrics by category:

 **HTTP Metrics:**

 | Metric Name | Description | Metric Type |
 |----------------------------------|------------------------------------------------------|-------------|
 | `http_requests_total` | Total number of HTTP requests received | Counter |
 | `http_request_duration_seconds` | Duration of HTTP requests in seconds | Histogram |
 | `http_response_status` | HTTP response status codes | Counter |
 | `http_requests_inprogress` | Number of HTTP requests currently being processed | Gauge |
 | `http_request_errors_total` | Total number of HTTP request errors | Counter |

 **System Metrics:**

 | Metric Name | Description | Metric Type |
 |----------------------------|------------------------------------------------------|-------------|
 | `cpu_usage_percentage` | CPU usage percentage of a system or process | Gauge |
 | `memory_usage_bytes` | Memory usage in bytes | Gauge |
 | `disk_usage_bytes` | Disk usage in bytes | Gauge |
 | `network_bytes_transmitted_total` | Total number of bytes transmitted over the network | Counter |
 | `database_connections` | Number of active database connections | Gauge |

 **Performance Metrics:**

 | Metric Name | Description | Metric Type |
 |---------------------------|------------------------------------------------------|-------------|
 | `latency_seconds` | Latency of a specific operation or service | Summary |
 | `response_time_seconds` | Response time of a service or API | Summary |
 | `queue_size` | Size of a queue or message backlog | Gauge |

 **Error Metrics:**

 | Metric Name | Description | Metric Type |
 |----------------------------|------------------------------------------------------|-------------|
 | `error_rate` | Rate of errors occurring in a system or process | Gauge |

 Please note that these groupings are not exhaustive, and you can categorize metrics based on your specific monitoring requirements.



 ## Background Jobs/Outbox Pattern


 **System Metrics:**

| Metric Name                       | Description                                        | Metric Type |
|-----------------------------------|----------------------------------------------------|-------------|
| `outbox_size`                     | Number of pending outbox message to be delivered   | Gauge       |
| `inbox_size`                      | Number of pending inbox message to be processed    | Gauge       |

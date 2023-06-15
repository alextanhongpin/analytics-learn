 Certainly! Here's an updated table with an additional column indicating the metric type to which the PromQL query can be applied:

 | PromQL Query | Description | Value Provided | Applicable Metric Type |
 |----------------------------------------------------------|----------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
 | `sum(metric_name)` | Calculates the sum of a metric over a given time range. | Provides the total aggregated value of the metric over the specified time range. | Counter, Gauge |
 | `avg(metric_name)` | Calculates the average of a metric over a given time range. | Provides the average value of the metric over the specified time range. | Gauge |
 | `rate(metric_name[5m])` | Calculates the per-second average rate of change of a metric over a 5-minute window. | Provides the rate of change of the metric, indicating how the metric is increasing or decreasing over time. | Counter |
 | `max(metric_name)` | Finds the maximum value of a metric over a given time range. | Provides the highest value observed for the metric over the specified time range. | Gauge |
 | `min(metric_name)` | Finds the minimum value of a metric over a given time range. | Provides the lowest value observed for the metric over the specified time range. | Gauge |
 | `quantile(0.95, metric_name)` | Calculates the 95th percentile value of a metric over a given time range. | Provides a measure of the metric value below which 95% of the data points fall, indicating the overall distribution and tail behavior of the metric. | Histogram |
 | `histogram_quantile(0.95, sum(rate(metric_name[5m])) by (le))` | Calculates the 95th percentile value for a histogram metric over a 5-minute window. | Useful for analyzing histogram metrics, providing a quantile value based on the distribution of the metric's buckets. | Histogram |
 | `delta(metric_name[1h])` | Calculates the difference between the first and last value of a metric over a 1-hour window. | Useful for analyzing cumulative counters or calculating the change in a metric's value over a specific time period. | Counter |
 | `increase(metric_name[1h])` | Calculates the total increase in a metric's value over a 1-hour window. | Provides the net increase in the metric's value over the specified time range. | Counter |
 | `topk(5, metric_name)` | Returns the top 5 time series with the highest values for a given metric. | Useful for identifying the top contributors or outliers in a metric's values. | Gauge |
 | `bottomk(5, metric_name)` | Returns the bottom 5 time series with the lowest values for a given metric. | Useful for identifying the least performing or least utilized entities based on a metric. | Gauge |

 Please note that the applicable metric types mentioned here are general guidelines, and the suitability of a PromQL query may vary based on your specific metric setup and requirements.

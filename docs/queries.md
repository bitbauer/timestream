-- Find the average, p90, p95, and p99 CPU utilization for a specific EC2 host over the past 2 hours.  
SELECT region, host, BIN(time, 15s) AS binned_timestamp,
    ROUND(AVG(measure_value::double), 2) AS avg_cpu_utilization,
    ROUND(APPROX_PERCENTILE(measure_value::double, 0.9), 2) AS p90_cpu_utilization,
    ROUND(APPROX_PERCENTILE(measure_value::double, 0.95), 2) AS p95_cpu_utilization,
    ROUND(APPROX_PERCENTILE(measure_value::double, 0.99), 2) AS p99_cpu_utilization
FROM "ec2Metrics".cpu
WHERE measure_name = 'usage_user'
    AND time > ago(2h)
GROUP BY region, host, BIN(time, 15s)
ORDER BY binned_timestamp ASC

-- Find the average CPU utilization binned at 30 second intervals for a specific EC2 host over the past 2 hours, filling in the missing values using interpolation based on the last observation carried forward.  
WITH binned_timeseries AS (
    SELECT host, BIN(time, 30s) AS binned_timestamp, ROUND(AVG(measure_value::double), 2) AS avg_cpu_utilization
    FROM "ec2Metrics".cpu
    WHERE measure_name = 'usage_user'
        AND host = 'ip-10-10-10-161.eu-west-1.compute.internal'
        AND time > ago(2h)
    GROUP BY host, BIN(time, 30s)
), interpolated_timeseries AS (
    SELECT host,
        INTERPOLATE_LOCF(
            CREATE_TIME_SERIES(binned_timestamp, avg_cpu_utilization),
                SEQUENCE(min(binned_timestamp), max(binned_timestamp), 15s)) AS interpolated_avg_cpu_utilization
    FROM binned_timeseries
    GROUP BY host
)
SELECT time, ROUND(value, 2) AS interpolated_cpu
FROM interpolated_timeseries
CROSS JOIN UNNEST(interpolated_avg_cpu_utilization)

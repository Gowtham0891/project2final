# API Service

| Category     | SLI | SLO                                                                                                         |
|--------------|-----|-------------------------------------------------------------------------------------------------------------|
| Availability |     | 99%                                                                                                         |
| Latency      |     | 90% of requests below 100ms                                                                                 |
| Error Budget |     | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput   |     | 5 RPS indicates the application is functioning                                                              |

Availability:
sum (rate(apiserver_request_total{job="apiserver",code=~"2.."}[5m])) / sum (rate(apiserver_request_total{job="apiserver"}[5m])) 
99% of successful 200 web requests per 5 minute interval  


Latency:
histogram_quantile(0.90, sum(rate(apiserver_request_duration_seconds_bucket{job="apiserver"}[5m])) by (le, verb))
90% of requests response is achieved with in 100 ms and the above one shows for last 5 mins

Error Budget:
1 - ((1 - (sum(increase(apiserver_request_total{job="apiserver", code="200"}[5m])) by (verb)) / sum(increase(apiserver_request_total{job="apiserver"}[5m])) by (verb)) / (1 - .80)) 
20% of the total request can fail but still in the budget radar. aboe one shows 80% of the requests us with 200 response for the last 5 mins.


Throughput:
sum(rate(apiserver_request_total{job="apiserver",code=~"2.."}[5m]))
Total number of successful requests over 5 minute interval

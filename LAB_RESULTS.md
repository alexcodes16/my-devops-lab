# DevOps Lab Results

## Exercise 1: Finding the Knee of the Curve

| RPS | Success | Throughput | P99 Latency |
|---:|---:|---:|---:|
| 1000 | 100.00% | 1000.01 req/s | 2.187 ms |
| 2000 | 100.00% | 1999.96 req/s | 5.705 ms |
| 3000 | 39.26% | 254.64 req/s | 30.395 s |
| 4000 | 23.32% | 184.13 req/s | 32.733 s |

Baseline P99 at 1000 RPS:

2.187 ms

Latency wall threshold:

5 x 2.187 ms = 10.935 ms

### Saturation Point

The saturation point is 3000 RPS because this is the first tested rate where the success ratio dropped below 100%.

### Latency Wall

The latency wall is also 3000 RPS because the P99 latency increased to 30.395 seconds, which is far above the 10.935 ms threshold.

## Exercise 2: Remote Telemetry

During the 3000 RPS telemetry test, Vegeta reported:

- Success ratio: 29.31%
- HTTP 200 responses: 7992
- Status code 0 failures: 19275
- P99 latency: 31.994 s
- Errors included EOF, timeouts, and closed connections.

The NGINX worker peaked at approximately 40% CPU usage during the test. Since it did not approach 100% CPU while the success ratio dropped significantly, the primary bottleneck was not NGINX computation.

The bottleneck was more likely in the TCP/network/kernel path, such as connection backlog exhaustion, socket handling, or connection timeouts.

The last 100 lines of the NGINX access log contained 100 HTTP 200 responses.

Requests reported by Vegeta with status code 0 did not successfully complete at the HTTP layer. These requests failed because of TCP/network-level problems such as EOFs, timeouts, or closed connections, so they did not appear as successful HTTP 200 requests in the NGINX access log.

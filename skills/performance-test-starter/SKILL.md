---
name: performance-test-starter
description: Design and script a first performance test (load, stress, spike, soak) with k6, JMeter, or Locust, with SLO-based thresholds and result interpretation.
---
# Performance test starter
1. Define the goal before writing a script: which user journey or endpoint, the expected traffic (RPS or concurrent users), and the SLOs (e.g. p95 < 500 ms, error rate < 1%). Never load-test production or third-party systems without written approval.
2. Pick the test type: smoke (1–2 VUs), load (expected peak), stress (ramp past peak), spike, or soak (hours).
3. k6 (JS, CLI-friendly, good in CI):
```js
import http from 'k6/http';
import { check, sleep } from 'k6';
export const options = {
  stages: [{ duration: '2m', target: 50 }, { duration: '5m', target: 50 }, { duration: '1m', target: 0 }],
  thresholds: { http_req_failed: ['rate<0.01'], http_req_duration: ['p(95)<500'] },
};
export default function () {
  const res = http.get(`${__ENV.BASE_URL}/api/products`);
  check(res, { 'status 200': (r) => r.status === 200 });
  sleep(1);
}
```
   Run `k6 run -e BASE_URL=https://staging.example.com load.js`. A breached threshold gives a non-zero exit code, which fails CI.
4. JMeter (GUI for building, CLI for running): `jmeter -n -t plan.jmx -l results.jtl -e -o report/`. Use a Thread Group with ramp-up, CSV Data Set Config for data, and Response Assertions. Never run load from the GUI.
5. Locust (Python):
```python
from locust import HttpUser, task, between
class Shopper(HttpUser):
    wait_time = between(1, 3)
    @task
    def browse(self): self.client.get("/api/products")
```
   Run `locust -f locustfile.py --headless -u 50 -r 5 -t 5m --host https://staging.example.com --csv out`.
6. Realism: use think time, varied test data, correlation of tokens, and a warm-up period. Exclude static assets unless they are the target.
7. Interpret: compare p50/p95/p99, throughput and errors against the SLOs. Find the knee point where latency climbs, and correlate with server metrics (CPU, DB, GC). Report the bottleneck hypothesis, not just the numbers.

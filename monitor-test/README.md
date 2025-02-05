# Monitoring test from playwright
* Prometheus
* Grafana

## 1. Create playwright project
```
$npm init playwright@latest
```

## 2. Install report test plugin
* [Playwright Prometheus Reporter](https://www.npmjs.com/package/@pumpo5.dev/playwright-prometheus-reporter)
```
%npm install --save-dev @pumpo5.dev/playwright-prometheus-reporter
```

## 3. Create monitoring test
* Prometheus
* Prometheus pushgateway
* Grafana

```
$docker compose up -d pushgateway
$docker compose up -d prometheus
$docker compose up -d grafana

$docker compose ps
NAME                         IMAGE              COMMAND                  SERVICE       CREATED          STATUS          PORTS
monitor-test-grafana-1       grafana/grafana    "/run.sh"                grafana       4 seconds ago    Up 4 seconds    0.0.0.0:3000->3000/tcp
monitor-test-prometheus-1    prom/prometheus    "/bin/prometheus --w…"   prometheus    16 seconds ago   Up 15 seconds   0.0.0.0:9090->9090/tcp
monitor-test-pushgateway-1   prom/pushgateway   "/bin/pushgateway --…"   pushgateway   42 seconds ago   Up 41 seconds   9091/tcp, 0.0.0.0:9191->9191/tcp
```

### Open Prometheus server
* http://localhost:9090

### Open Grafana server
* http://localhost:3000
  * user=admin
  * password=admin

## 6. Config to keep test report
* File `config.conf`

```
pn5.reporting.enabled="true"
pn5.reporting.prometheus.enabled="true"
pn5.reporting.prometheus.testcases_track="true"
pn5.reporting.prometheus.suite="todo"
pn5.reporting.prometheus.endpoint="http://localhost:9191"
pn5.reporting.prometheus.labels="environment=ppe,type=end-to-end"
```

## 5. Run test
```
$npx playwright test
[2025-02-05 14:09:43.147 +0700] DEBUG: [PumpoEvents] Loading configuration file from resolved config path /workshop-playwright-2025/monitor-test/config.conf
[2025-02-05 14:09:43.147 +0700] INFO: Adding custom label: environment=ppe
[2025-02-05 14:09:43.147 +0700] INFO: Adding custom label: type=end-to-end
[2025-02-05 14:09:43.156 +0700] INFO: Starting the run with 2 tests.
[2025-02-05 14:09:43.407 +0700] INFO: Starting test "has title_chromium_example.spec.ts"
[2025-02-05 14:09:43.407 +0700] INFO: Starting test "get started link_chromium_example.spec.ts"
[2025-02-05 14:09:44.514 +0700] INFO: Test case name: has title_chromium_example.spec.ts job name undefined
[2025-02-05 14:09:44.514 +0700] INFO: Finished test "has title_chromium_example.spec.ts" with result passed
[2025-02-05 14:09:44.514 +0700] DEBUG: Pushing metric test_execution_total with labels todo,has title,,chromium,ppe,end-to-end,passed to Prometheus
[2025-02-05 14:09:44.514 +0700] DEBUG: Pushing metric test_execution_finish_time with labels todo,has title,,chromium,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.515 +0700] DEBUG: Pushing metric test_execution_duration_seconds with labels todo,has title,,chromium,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.670 +0700] INFO: Test case name: get started link_chromium_example.spec.ts job name undefined
[2025-02-05 14:09:44.670 +0700] INFO: Finished test "get started link_chromium_example.spec.ts" with result passed
[2025-02-05 14:09:44.670 +0700] DEBUG: Pushing metric test_execution_total with labels todo,get started link,,chromium,ppe,end-to-end,passed to Prometheus
[2025-02-05 14:09:44.670 +0700] DEBUG: Pushing metric test_execution_finish_time with labels todo,get started link,,chromium,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.670 +0700] DEBUG: Pushing metric test_execution_duration_seconds with labels todo,get started link,,chromium,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.688 +0700] INFO: Finished the run: passed
[2025-02-05 14:09:44.689 +0700] DEBUG: Pushing metric suite_execution_finish_time with labels todo,,,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.689 +0700] DEBUG: Pushing metric suite_execution_duration_seconds with labels todo,,,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.689 +0700] DEBUG: Pushing metric suite_execution_total with labels todo,,,ppe,end-to-end to Prometheus
[2025-02-05 14:09:44.689 +0700] DEBUG: Pushing metric suite_execution_failed with labels todo,,,ppe,end-to-end to Prometheus
```

# Windows Monitoring
SkyWalking leverages Prometheus windows_exporter to collect metrics data from the Windows and leverages OpenTelemetry Collector to transfer the metrics to
[OpenTelemetry receiver](opentelemetry-receiver.md) and into the [Meter System](./../../concepts-and-designs/meter.md).
Windows entity as a `Service` in OAP and on the `Layer: OS_WINDOWS`.

## Data flow
**For OpenTelemetry receiver:**
1. The Prometheus windows_exporter collects metrics data from the VMs.
2. The OpenTelemetry Collector fetches metrics from windows_exporter via Prometheus Receiver and pushes metrics to the SkyWalking OAP Server via the OpenCensus gRPC Exporter or OpenTelemetry gRPC exporter.
3. The SkyWalking OAP Server parses the expression with [MAL](../../concepts-and-designs/mal.md) to filter/calculate/aggregate and store the results.

## Setup
**For OpenTelemetry receiver:**
1. Setup [Prometheus node-exporter](https://prometheus.io/docs/guides/node-exporter/).
2. Setup [OpenTelemetry Collector ](https://opentelemetry.io/docs/collector/). This is an example for OpenTelemetry Collector configuration [otel-collector-config.yaml](../../../../test/e2e-v2/cases/vm/prometheus-node-exporter/otel-collector-config.yaml).
3. Config SkyWalking [OpenTelemetry receiver](opentelemetry-receiver.md).

## Supported Metrics

| Monitoring Panel             | Unit | Metric Name                                                                                                             | Description                                                                                    | Data Source                                         |
|------------------------------|------|-------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------|-----------------------------------------------------|
| CPU Usage                    | %    | meter_win_cpu_total_percentage                                                                                           | The total percentage usage of the CPU core. If there are 2 cores, the maximum usage is 200%.   | Prometheus node-exporter |
| Memory RAM Usage             | MB   | meter_win_memory_used                                                                                                  | The total RAM usage                                                                            | Prometheus node-exporter |
| Memory Swap Usage            | %    | meter_win_memory_swap_percentage                                                                                    | The percentage usage of swap memory                                                            | Prometheus node-exporter |
| CPU Average Used             | %    | meter_win_cpu_average_used                                                                                              | The percentage usage of the CPU core in each mode                                              | Prometheus node-exporter |
| Memory RAM                   | MB   | meter_win_memory_total<br />meter_win_memory_available<br />meter_win_memory_used                                          | The RAM statistics, including Total / Available / Used                                         | Prometheus node-exporter |
| Memory Swap                  | MB   | meter_win_memory_swap_free<br />meter_win_memory_swap_total                                                               | Swap memory statistics, including Free / Total                                                 | Prometheus node-exporter |                                                                                         | The percentage usage of the file system at each mount point                                    | Prometheus node-exporter |
| Disk R/W                     | KB/s | meter_win_disk_read,meter_win_disk_written                                                                                | The disk read and written                                                                      | Prometheus node-exporter |
| Network Bandwidth Usage      | KB/s | meter_win_network_receive<br />meter_win_network_transmit                                                                 | The network receive and transmit                                                               | Prometheus node-exporter |                                                                                     | The number of file descriptors allocated                                                       | Prometheus node-exporter |

## Customizing
You can customize your own metrics/expression/dashboard panel.
The metrics definition and expression rules are found in `/config/otel-rules/windows.yaml`.
The dashboard panel confirmations are found in `/config/ui-initialized-templates/os_windows`.
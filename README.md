# DogStatsd and Telegraf

## General info
This is created for testing purpose. Update <API_KEY> in docker compose files first.

## telegraf.conf
```
[[inputs.statsd]]
  protocol = "udp"
  max_tcp_connections = 250
  service_address = ":8125"
  delete_gauges = true
  delete_counters = true
  delete_sets = true
  delete_timings = true
  percentiles = [95.00]
  metric_separator = "."
  #parse_data_dog_tags = true
  datadog_extensions = true
  allowed_pending_messages = 10000
  percentile_limit = 1000
  namedrop = ["datadog*"]
  [inputs.statsd.tags]
    plugin = "statsd"
[[outputs.file]]
  ## Files to write to, "stdout" is a specially handled file.
 files = [ "stdout" ]
 data_format = "influx"
```
## Test using datadog agent and telegraf

```
$ docker-compose -f dd-agent-telegraf.yaml up -d
```

### check telegraf logs

```
$ docker logs telegraf

2023-09-27T13:54:27Z I! Loading config: /etc/telegraf/telegraf.conf
2023-09-27T13:54:27Z I! Starting Telegraf 1.26.3
2023-09-27T13:54:27Z I! Available plugins: 235 inputs, 9 aggregators, 27 processors, 22 parsers, 57 outputs, 2 secret-stores
2023-09-27T13:54:27Z I! Loaded inputs: statsd
2023-09-27T13:54:27Z I! Loaded aggregators:
2023-09-27T13:54:27Z I! Loaded processors:
2023-09-27T13:54:27Z I! Loaded secretstores:
2023-09-27T13:54:27Z I! Loaded outputs: file
2023-09-27T13:54:27Z I! Tags enabled: host=dfc5db35613f
2023-09-27T13:54:27Z I! [agent] Config: Interval:10s, Quiet:false, Hostname:"dfc5db35613f", Flush Interval:10s
2023-09-27T13:54:27Z I! [inputs.statsd] UDP listening on "[::]:8125"
2023-09-27T13:54:27Z I! [inputs.statsd] Started the statsd service on ":8125"
2023-09-27T13:54:33Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.process.containers.host_count:4|g|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:33Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.process.processes.host_count:66|g|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics_by_type:2|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context:2|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context_by_type:2|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.aggregated_context_by_type:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metrics:2|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.metric_dropped_on_receive:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.packets_dropped:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.bytes_dropped:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
2023-09-27T13:54:39Z E! [inputs.statsd] Splitting '|', unable to parse metric: datadog.dogstatsd.client.events:0|c|c:550550692a0a1a56f56103b4fc2625c374c3e965b867efc69428d9bb05d425ee
```

## Test using standalone DogStatsd and telegraf

```
$ docker-compose -f dd-dogstatsd-telegraf.yaml up -d
```
### check telegraf logs

```
$ docker logs telegraf

2023-09-27T14:03:24Z I! Loading config: /etc/telegraf/telegraf.conf
2023-09-27T14:03:24Z I! Starting Telegraf 1.26.3
2023-09-27T14:03:24Z I! Available plugins: 235 inputs, 9 aggregators, 27 processors, 22 parsers, 57 outputs, 2 secret-stores
2023-09-27T14:03:24Z I! Loaded inputs: statsd
2023-09-27T14:03:24Z I! Loaded aggregators:
2023-09-27T14:03:24Z I! Loaded processors:
2023-09-27T14:03:24Z I! Loaded secretstores:
2023-09-27T14:03:24Z I! Loaded outputs: file
2023-09-27T14:03:24Z I! Tags enabled: host=796330abf650
2023-09-27T14:03:24Z I! [agent] Config: Interval:10s, Quiet:false, Hostname:"796330abf650", Flush Interval:10s
2023-09-27T14:03:24Z I! [inputs.statsd] UDP listening on "[::]:8125"
2023-09-27T14:03:24Z I! [inputs.statsd] Started the statsd service on ":8125"
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823430000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823440000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823450000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823460000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823470000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823480000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823490000000000
mytest.requests.incr.one.count,host=796330abf650,metric_type=counter,plugin=statsd value=1i 1695823500000000000
```

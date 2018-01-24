# Logstash Input Plugin

This plugin reads metrics exposed by [Logstash Monitoring API](https://www.elastic.co/guide/en/logstash/current/monitoring-logstash.html).

### Configuration:

```toml
  ## This plugin reads metrics exposed by Logstash Monitoring API.
  # https://www.elastic.co/guide/en/logstash/current/monitoring.html
  #
  # url = "http://localhost:9600"
  url = "http://localhost:9600"
```

### Measurements & Fields:

- logstash_jvm
    - threads_peak_count
    - mem_pools_survivor_peak_max_in_bytes
    - mem_pools_survivor_max_in_bytes
    - mem_pools_old_peak_used_in_bytes
    - mem_pools_young_used_in_bytes
    - mem_non_heap_committed_in_bytes
    - threads_count
    - mem_pools_old_committed_in_bytes
    - mem_pools_young_peak_max_in_bytes
    - mem_heap_used_percent
    - gc_collectors_young_collection_time_in_millis
    - mem_pools_survivor_peak_used_in_bytes
    - mem_pools_young_committed_in_bytes
    - gc_collectors_old_collection_time_in_millis
    - gc_collectors_old_collection_count
    - mem_pools_survivor_used_in_bytes
    - mem_pools_old_used_in_bytes
    - mem_pools_young_max_in_bytes
    - mem_heap_max_in_bytes
    - mem_non_heap_used_in_bytes
    - mem_pools_survivor_committed_in_bytes
    - mem_pools_old_max_in_bytes
    - mem_heap_committed_in_bytes
    - mem_pools_old_peak_max_in_bytes
    - mem_pools_young_peak_used_in_bytes
    - mem_heap_used_in_bytes
    - gc_collectors_young_collection_count
    - uptime_in_millis

- logstash_process
    - open_file_descriptors
    - cpu_load_average_1m
    - cpu_load_average_5m
    - cpu_load_average_15m
    - cpu_total_in_millis
    - cpu_percent
    - peak_open_file_descriptors
    - max_file_descriptors
    - mem_total_virtual_in_bytes
    - mem_total_virtual_in_bytes

- logstash_events
    - queue_push_duration_in_millis
    - duration_in_millis
    - in
    - filtered
    - out

- logstash_plugins
  There are 3 categories, input, filter, output. For each will be separate measurement consisting:
      - tags
        - id (the id of the plugin configured fia logstash)
        - type (input|filter|output)
        - plugin (name of the plugin, eg:beats, stdout)
      - fields
        - queue_push_duration_in_millis (for input plugins only)
        - duration_in_millis
        - in
        - out

### Example Output:

```
$ ./telegraf -config telegraf.conf -input-filter logstash -test
* Plugin: inputs.logstash, Collection 1
> logstash_jvm,host=gevislogstp1at.eb.lan.at,node_id=3e440eb1-816b-4000-9e13-050660045aad mem_pools_old_committed_in_bytes=7892041728,mem_pools_old_peak_used_in_bytes=5968082504,gc_collectors_young_collection_time_in_millis=385667,uptime_in_millis=63652303,mem_pools_survivor_committed_in_bytes=69730304,mem_pools_old_used_in_bytes=722930960,mem_heap_committed_in_bytes=8520204288,mem_pools_survivor_peak_max_in_bytes=69730304,mem_pools_old_max_in_bytes=7892041728,mem_pools_young_peak_used_in_bytes=558432256,mem_pools_young_peak_max_in_bytes=558432256,gc_collectors_old_collection_time_in_millis=498,mem_heap_max_in_bytes=8520204288,mem_heap_used_in_bytes=799671248,mem_non_heap_used_in_bytes=138960288,mem_pools_survivor_peak_used_in_bytes=69730304,mem_heap_used_percent=9,gc_collectors_old_collection_count=5,threads_peak_count=71,mem_non_heap_committed_in_bytes=150446080,mem_pools_survivor_max_in_bytes=69730304,mem_pools_old_peak_max_in_bytes=7892041728,mem_pools_survivor_used_in_bytes=16014640,mem_pools_young_committed_in_bytes=558432256,mem_pools_young_used_in_bytes=60725648,mem_pools_young_max_in_bytes=558432256,gc_collectors_young_collection_count=26102,threads_count=69 1516779590000000000
> logstash_process,node_id=3e440eb1-816b-4000-9e13-050660045aad,host=gevislogstp1at.eb.lan.at peak_open_file_descriptors=247,mem_total_virtual_in_bytes=15172792320,cpu_load_average_1m=4.69,cpu_total_in_millis=93515080,open_file_descriptors=235,max_file_descriptors=16384,cpu_percent=53,cpu_load_average_15m=3.32,cpu_load_average_5m=3.9 1516779590000000000
> logstash_events,host=gevislogstp1at.eb.lan.at,node_id=3e440eb1-816b-4000-9e13-050660045aad filtered=49036967,out=49036592,queue_push_duration_in_millis=11373589,duration_in_millis=181516470,in=49037595 1516779590000000000
> logstash_plugins,host=gevislogstp1at.eb.lan.at,type=input,id=100_001_1,plugin=beats queue_push_duration_in_millis=11373589,duration_in_millis=0,in=0,out=49037595 1516779590000000000
> logstash_plugins,type=filter,id=200_006_angpA,plugin=useragent,host=gevislogstp1at.eb.lan.at duration_in_millis=0,in=0,out=0 1516779590000000000
> logstash_plugins,plugin=useragent,host=gevislogstp1at.eb.lan.at,type=filter,id=202_007_prestoWeb duration_in_millis=610365,in=23279887,out=23279887 1516779590000000000
> logstash_plugins,type=filter,id=202_010_prestoGrp,plugin=mutate,host=gevislogstp1at.eb.lan.at duration_in_millis=2428,in=76790,out=76790 1516779590000000000
> logstash_plugins,id=201_009_stsWeb,plugin=geoip,host=gevislogstp1at.eb.lan.at,type=filter duration_in_millis=211976,in=10906494,out=10906494 1516779590000000000
> logstash_plugins,host=gevislogstp1at.eb.lan.at,type=filter,id=201_010_stsWeb,plugin=kv duration_in_millis=496762,in=10906494,out=10906494 1516779590000000000
> logstash_plugins,host=gevislogstp1at.eb.lan.at,type=filter,id=201_007_stsWeb,plugin=mutate out=10906494,duration_in_millis=301181,in=10906494 1516779590000000000
> logstash_plugins,type=filter,id=202_001_prestoWeb,plugin=grok,host=gevislogstp1at.eb.lan.at duration_in_millis=11267769,in=23279890,out=23279887 1516779590000000000
> logstash_plugins,type=filter,id=202_006_prestoWeb,plugin=kv,host=gevislogstp1at.eb.lan.at out=23279887,duration_in_millis=1064133,in=23279887 1516779590000000000
> logstash_plugins,id=202_008_prestoGrp,plugin=grok,host=gevislogstp1at.eb.lan.at,type=filter duration_in_millis=19283,in=76790,out=76790 151677959000000000```

On every node. Download binary and extract 
modify config file

#!/bin/bash
/var/tmp/promtail-linux -client.max-retries=20 --config.file=/var/tmp/promtail-config.yml > /var/tmp/promtail.log 2>&1 &



#Promtail-config.yml, label can be added as "K/V" pair

server:
  http_listen_port: 9080
  grpc_listen_port: 0
positions:
  filename: /var/tmp/positions.yaml
clients:
  - url: http://hostname:3100/loki/api/v1/push
scrape_configs:
- job_name: aerospike
  static_configs:
  - targets:
      - hostname
    labels:
      job: aslogs
      host: hostname
      __path__: /data/aerospike/log/*log




On Loki node, Download binary and extract, Config file change as per requirement

#!/bin/bash
/var/tmp/loki-linux --config.file=/var/tmp/loki-config.yml > /var/tmp/loki.log 2>&1 &


On grafan web server, add loki as data source


Grafana---> Explore--->
1. log search

{host="hostname", filename="/data/aerospike/log/aerospike.log"} |= "gapitw" |= "client" |= "LDAP authentication failed"

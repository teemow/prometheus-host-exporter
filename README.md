# Host exporter

Export host information to prometheus.

## Dependencies

 * Prometheus (obviously)
 * Node Exporter with textfile collector

## Install

Copy `host_exporter` to `/usr/local/bin`.

Copy the systemd unit to `/etc/systemd/system` and run 

```
systemctl enable prometheus-host-exporter
systemctl start prometheus-host-exporter
```

## Configure your node exporter

Make sure your node exporter uses `textfile` in `--collectors.enabled` and add the following parameter: `--collector.textfile.directory=/var/lib/node_exporter/textfile_collector`

## Group this information in your queries

```
sum by (instance)(irate(node_cpu{job="node",mode=~"^(system|user)$"}[5m])) * on (instance) group_right(host,chassis,machineid,bootid,os,kernel,arch) host
```

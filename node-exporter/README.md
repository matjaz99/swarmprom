# swarmprom/node-exporter by matjaz99

I forked this project because I wanted to change the version of node-exporter.

I also added new directory: `/file-collector` (see Dockerfile). You can save your custom metrics in `.prom` format here.
Why I did that? Because original directory /etc/node-exporter already contained entrypoint.sh and I overwrite it with my directory for metrics, which doesn't have entrypoint.sh and the container would not start.

```yaml
services:
  node-exporter:
    image: matjaz99/node-exporter:v0.18.1
    networks:
      - monitoring_network
    ports:
      - 9100:9100
    environment:
      - NODE_ID={{.Node.ID}}
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
      - /etc/hostname:/etc/nodename
      - ${PWD}/node_exporter_file_collector:/file-collector
    command:
      - '--path.sysfs=/host/sys'
      - '--path.procfs=/host/proc'
      - '--collector.textfile.directory=/file-collector'
      - '--collector.filesystem.ignored-mount-points=^/(sys|proc|dev|host|etc)($$|/)'
      # no collectors are explicitely enabled here, because the defaults are sufficient
      - '--no-collector.ipvs'
    deploy:
      mode: global
```

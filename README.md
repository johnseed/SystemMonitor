# Prometheus

## Prometheus

### Install 

    $ docker run --name prometheus -d -p 127.0.0.1:9090:9090 prom/prometheus

## node_exporter

## Auth

Basic auth:

Install httpd-tools

    $ yum install httpd-tools -y

Generate hash

    $ htpasswd -nBC 12 '' | tr -d ':\n'


### Linux

    $ docker run -d \
        --net="host" \
        --pid="host" \
        -v "/:/host:ro,rslave" \
        -v "/root/monitor/node_exporter/config.yaml:/config.yaml" \
        quay.io/prometheus/node-exporter:latest \
        --path.rootfs=/host \
        --web.config=/config.yaml

### Windows

https://github.com/prometheus-community/windows_exporter

## cAdvisor

https://prometheus.io/docs/guides/cadvisor/

    $ docker run    --volume=/:/rootfs:ro    --volume=/var/run:/var/run:ro    --volume=/sys:/sys:ro    --volume=/var/lib/docker/:/var/lib/docker:ro    --volume=/dev/disk/:/dev/disk:ro    --publish=8080:8080    --detach=true    --name=cadvisor    --privileged    --device=/dev/kmsg    gcr.io/cadvisor/cadvisor


## Grafana

Dashboards :   

* 8919 node_exporter  
* 14282 cAdvisor

Run :  

* docker

    $ docker run -d --name=grafana -p 3000:3000 grafana/grafana

* docker compose
```yml
  grafana:
    image: grafana/grafana
    container_name: grafana
    ports: 
    - 3000:3000
    depends_on: 
    - prometheus
```

## export image

```bash
docker save gcr.io/cadvisor/cadvisor > cadvisor.tar
docker save prom/prometheus > prometheus.tar
docker save prom/node-exporter > node-exporter.tar
docker save grafana/grafana > grafana.tar
```

## load image

```bash
docker load < cadvisor.tar
docker load < prometheus.tar
docker load < node-exporter.tar
docker load < grafana.tar
```

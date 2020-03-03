# Prometheus

This configures Prometheus by mounting into the container at `/etc/prometheus/` and sets up scrapers for the three other services that provide metrics: node-exporter, cadvisor and traefik.

It scrapes every 30 seconds, this is a number I eventually came to after a few months of testing under a medium load with a few services running.

Aside from that, there's nothing too special here. You can see the full documentation for this file [here](https://prometheus.io/docs/prometheus/latest/configuration/configuration/).

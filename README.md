# Pico Stack: Core Services

This repository contains the core services that form the foundation of the Pico Stack. It contains:

- [Traefik][traefik]: as a HTTP gateway and for automatic SSL certificates
- [Portainer][portainer]: to see what's running
- [Watchtower][watchtower]: for keeping container images up to date
- [Grafana][grafana]: for viewing logs, metrics and alerts
- [Prometheus][prometheus]: for metric collection
- [Node Exporter][node_exporter]: for system metrics
- [cAdvisor][cadvisor]: for container metrics
- [Loki][loki]: for log aggregation

This project is designed to be plug-and-play and kept up to date. It's perfect for small to medium deployments in individual servers where Kubernetes is considered overkill.

I ([Southclaws][southclaws]) run this exact stack on my production servers using [Pico][pico] to always use the latest commit. So it should be always up to date and in a working state. If you find a problem with the setup or documentation, please open an issue.

## Deployment

Deployment is quite simple the first time around. Provided your execution environment ([Pico][pico] or a shell) has the necessary environment variables, it's just a `docker-compose up -d` followed by some logging and first-time configuring.

All you need is:

- A server, virtual or physical
- A domain name

### Environment Variables

```sh
HOSTNAME=server001
DATA_DIR=/data/shared
DOMAIN_NAME=myawesomeproject.com
ACME_EMAIL=admin@myawesomeproject.com
CA_SERVER=https://acme-v02.api.letsencrypt.org/directory
CORE_SERVICES_BASIC_AUTH=$2a$11$4/ET4dmxtvpW1mgvc9F8qus.gNb9MMFxOxfRGLDA02wACCIIfCYnC
```

#### `HOSTNAME`

The name of the machine. This is required used for building domains for services.

#### `DATA_DIR`

This is where data will be persisted on the host machine.

- `${DATA_DIR}/traefik/acme`: Traefik ACME storage
- `${DATA_DIR}/portainer_data`: Portainer user data
- `${DATA_DIR}/prometheus`: Prometheus database
- `${DATA_DIR}/grafana`: Grafana user data
- `${DATA_DIR}/loki`: Log storage

`/data/shared` is a commonly used value for this variable.

#### `DOMAIN_NAME`

A domain name you own. See the Domains section below for details.

#### `ACME_EMAIL`

Your email address for registering SSL certificates with LetsEncrypt.

#### `CA_SERVER`

The value of `CA_SERVER` depends if you're deploying for the first time. Because LetsEncrypt has some pretty harsh rate limiting rules and you get banned for a whole week if you hit the limit, you should deploy using the staging server first to make sure your domains are all working correctly. Then, once you're happy with the setup, switch to the live server.

Staging:

```
CA_SERVER=https://acme-staging-v02.api.letsencrypt.org/directory
```

Live:

```
CA_SERVER=https://acme-v02.api.letsencrypt.org/directory
```

#### `BASIC_AUTH`

A HTTP basic auth string to use as a login for all the running services with a web interface.

## Domains

The compose configuration uses the convention `service-hostname.domain`. So, lets assume you own `myawesomeproject.com` and your machine is called `felix`. Your services would run on:

- `traefik-felix.myawesomeproject.com`
- `portainer-felix.myawesomeproject.com`
- `cadvisor-felix.myawesomeproject.com`
- `prometheus-felix.myawesomeproject.com`
- `grafana-felix.myawesomeproject.com`

## Auth

Both Grafana and Portainer have built-in user systems. The rest will use the `BASIC_AUTH` to set a basic HTTP login via Traefik.

When you first open Grafana, log in with `admin`/`admin` then change the password.

When you first open Portainer, create an admin account and connect it to the local Docker socket (the option on the far right).

The rest will just prompt a login dialog on your browser.

[traefik]: https://traefik.io
[portainer]: https://portainer.io
[watchtower]: https://containrrr.github.io/watchtower/
[grafana]: https://grafana.com
[prometheus]: https://prometheus.io
[node_exporter]: https://github.com/prometheus/node_exporter
[cadvisor]: https://github.com/google/cadvisor
[loki]: https://github.com/grafana/loki/
[southclaws]: https://southcla.ws
[pico]: https://github.com/picostack/pico

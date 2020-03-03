# Docker Daemon Configuration

This file lives in `/etc/docker/` and tells Docker to use Loki as its logging destination.

The documentation for installing Loki is [here](https://github.com/grafana/loki/tree/master/docs/clients/docker-driver).

Essentially you just need to install the plugin with the `docker plugin install` command then stop _and remove_ any running containers (keep in mind, a stopped container still _exists_, you must remove them completely in order for them to pick up a new logging driver config). Then you just restart the docker daemon with `sudo systemctl restart docker`.

If anything goes wrong, check the daemon logs by running `sudo journalctl -f -u docker.service` and if you're stuck, open an issue here!

## Brief explanation

What's happening here is, this config is telling Docker to use the `loki` logging driver. It then passes some options to the driver. The important one is `loki-url` which tells the driver where to send the logs.

In our compose config, we have a `loki` container running and it's exposing port `3100` into the host system (not publicly). All that happens then is the Docker daemon sends the logs from every container to the Loki driver, which then sends them to the Loki server which does the rest!

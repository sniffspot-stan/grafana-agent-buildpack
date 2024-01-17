# Grafana Agent Buildpack

This is a buildpack for
[Grafana Agent](https://github.com/grafana/agent) deployments.

The official agent buildpack is over 3 years old. This is to update the buildpack to use the latest version, so it is possible to run the agent in flow mode (by adding config variable AGENT_MODE=flow)

## Usage

### Config
Your config file should be placed into the root as `grafana-agent.cfg`. Note that it is not `.river` or `.yml` as this buildpack should support both flows. If syntax highlighting is needed, make `grafana-agent.cfg` a symblink to the specific file with extension desired.

The buildpack will substitute any environment variables. Example:

```yaml
---
server:
  http_listen_port: $PORT

prometheus:
  wal_directory: "./"
  global:
    scrape_interval: 5s
  configs:
    - name: agent
      host_filter: false
      scrape_configs:
        - job_name: taylor-swift-metrics
          metrics_path: /metrics
          scheme: https
          static_configs:
            - targets: ['target.taysway.xyz']
      remote_write:
        - url: https://prom.taysway.xyz/api/prom/push
          basic_auth:
            username: $USERNAME
            password: $PASSWORD
```


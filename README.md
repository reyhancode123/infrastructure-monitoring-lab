# Infrastructure Monitoring Lab

A lightweight Linux host monitoring stack built with Docker Compose, Prometheus, Node Exporter, and Grafana.

## Overview

This project demonstrates infrastructure monitoring on a Linux host using open-source monitoring tools. It collects system metrics, stores time-series data, and visualizes monitoring information through Grafana dashboards.

## Architecture

See the [Architecture Documentation](docs/architecture.md).

## Technology Stack

| Technology | Purpose |
|---|---|
|Docker Compose| Service orchestration |
|Prometheus| Metrics collection and storage |
|Node Exporter| Linux host metrics exporter |
|Grafana| Metrics visualization and dashboards |

## Monitored Metrics

- CPU utilization
- Memory usage
- Filesystem capacity and usage
- Network receive and transmit traffic
- Prometheus self-monitoring metrics

## Services and Ports

| Service | Address | Access |
|---|---|---|
| Prometheus | http://localhost:9090 | Host only |
| Grafana | http://localhost:3000 | Host only |
| Node Exporter | node-exporter:9100 | Internal Docker network |

## Getting Started

### Prerequisites

- Linux host
- Docker Engine
- Docker Compose plugin

### Start the Monitoring Stack

```bash
docker compose up -d
```

Check service status:

```bash
docker compose ps
```

### Access Grafana

Open http://localhost:3000.

Default login:

- Username: `admin`
- Password: `admin`

Change the default password after the first login. Do not use default credentials in production.

### Prometheus Targets

Open http://localhost:9090/targets to verify that the configured scrape targets are healthy.

The current scrape interval is 15 seconds.

## Configuration

- Prometheus: `prometheus/prometheus.yml`
- Grafana datasource: `grafana/provisioning/datasources/prometheus.yml`
- Docker Compose: `docker-compose.yml`
- Architecture: `docs/architecture.md`

Prometheus data retention is configured for 7 days. Prometheus and Grafana use persistent Docker volumes.

## Resource Limits

|Service | Memory Limit |
|---|---:|
| Prometheus | 256 MiB |
| Grafana | 256 MiB |
|Node Exporter | 64 MiB |
| **Total configured limits*z | 576 MiB |

These are maximum container memory limits, not actual memory usage.

## Network and Security

- Prometheus and Grafana ports are bound to `127.0.0.1@. 
- Node Exporter does not publish its port to the host.
- Services communicate over a dedicated Docker bridge network.
- Prometheus configuration is mounted read-only.
- Persistent volumes retain monitoring data across container recreation.

This setup is intended for local lab use, not direct public exposure.

## Validation
- Docker Compose configuration validation passed.
- Prometheus self-monitoring target reported as UP.
- Node Exporter target reported as UP.
- Grafana health endpoint returned OK.
- CPU, memory, filesystem, and network metrics were collected.
- Grafana dashboard successfully displayed host metrics.

## Current Limitations

- Monitoring focuses on Linux host metrics.
- Docker container-level monitoring is not implemented.
- MySQL-specific monitoring is not implemented.
- Alerting and notification channels are not configured.

## Future Improvements

- Add Docker container metrics.
- Integrate MySQL monitoring.
- Build custom Grafana dashboards.
- Configure Prometheus alerting rules and notifications.

## License

This project is intended for educational and portfolio demonstration purposes.

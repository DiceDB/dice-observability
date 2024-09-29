
# DiceDB Observability Stack

This project sets up a monitoring stack for [DiceDB](https://github.com/DiceDB/dice) using Prometheus and Grafana, with metrics exported by the Redis Exporter.

The setup is designed to run in Docker containers.

## Services

- **DiceDB**: The latest DiceDB image running on port 7379.
- **Dice Exporter**: Exports DiceDB metrics in a format compatible with Prometheus.
- **Prometheus**: Collects and stores metrics.
- **Grafana**: Visualizes metrics from Prometheus with pre-configured dashboards.

## Requirements

- Docker
- Docker Compose

## Setup

1. Clone this repository:
    ```bash
    git clone https://github.com/DiceDB/dice-observability.git
    cd dicedb-observability
    ```

2. The repository will contain the following files:
    - `docker-compose.yml`: Defines the services and how they interact.
    - `prometheus.yml`: Configuration for Prometheus to scrape metrics from the metrics Exporter.
    - `grafana/provisioning/datasources/datasource.yml`: Preconfigures Prometheus as a data source in Grafana.
    - `grafana/provisioning/dashboards/`: Contains Grafana dashboard JSON files.

3. Start the services with Docker Compose:
    ```bash
    docker-compose up -d
    ```

   This will start the following services:
   - DiceDB on `localhost:7379`
   - Redis Exporter on `localhost:9121`
   - Prometheus on `localhost:9090`
   - Grafana on `localhost:3000` (default login: `admin/admin`)

4. Log into Grafana:
    - Open your browser and go to `http://localhost:3000`.
    - Log in with the default credentials: `admin` / `admin`.
    - You'll find the pre-configured Redis Exporter dashboard ready to use.

> [!NOTE]
> Prometheus and Grafana may take up to 30 seconds to start showing metrics. If you don't see any data, wait a few moments and refresh the page.

## Configuration

### Prometheus

Prometheus is configured to scrape metrics from the Redis Exporter every 30 seconds. You can modify the scrape interval by editing the `prometheus.yml` file.

```yaml
global:
  scrape_interval: 30s  # Scrape every 30 seconds

scrape_configs:
  - job_name: 'dice_exporter'
    static_configs:
      - targets: ['dice_exporter:9121']
```

### Grafana Dashboards

Grafana is set up to automatically load pre-configured dashboards from the `grafana/provisioning/dashboards/` directory. You can modify or add new dashboards by placing JSON files in this directory.

## Useful Commands

- Start the stack:
    ```bash
    docker-compose up -d
    ```

- Stop the stack:
    ```bash
    docker-compose down
    ```

- View Raw DiceDB metrics: [http://localhost:9121/metrics](http://localhost:9121/metrics)
- View Prometheus metrics: [http://localhost:9090](http://localhost:9090)
- View Grafana dashboards: [http://localhost:3000](http://localhost:3000)

## Customization

- **Prometheus Configuration**: Update `prometheus.yml` to add more scrape jobs or change the scrape interval.
- **Grafana Dashboards**: Add custom dashboards by placing their JSON definitions in `grafana/provisioning/dashboards/`.

## License

This project is licensed under the MIT License.
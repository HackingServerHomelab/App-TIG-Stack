# App-TIG-Stack

## First Time Prerequisites

1. Run [Traefik](https://github.com/HackingServerHomelab/App-Traefik)

## Running the Containers

1. Update the following volume mounts in [docker-compose.yml](./Docker/docker-compose.yml)
    * `../Data/grafana:/var/lib/grafana`
    * `../Data/influxdb:/var/lib/influxdb`
2. Update the following environment variables in [docker-compose.yml](./Docker/docker-compose.yml)
    * `"FG_SERVER_ROOT_URL=CHANGEME.example.com"`
    * `"INFLUXDB_ADMIN_USER=admin"`
    * `"INFLUXDB_ADMIN_PASSWORD=changeme"`
3. Update the Traefik host label in [docker-compose.yml](./Docker/docker-compose.yml)
    * ``"traefik.http.routers.grafana.rule=Host(`localhost`)"``
4. Modify the InfluxDB user and password in [influxdb-init.iql](./Config/influxdb-scripts/influxdb-init.iql)
5. Run `docker-compose -f ./Docker/docker-compose.yml up -d`

## First Time Setup

1. Visit <http://your-ip:3000>
2. Log in to Grafana with the default credentials: `admin:admin`
3. Update the Grafana password
4. Add InfluxDB as a data source
    1. Go to `Configuration -> Data Sources -> Add data source`
    2. Select InfluxDB and set the following settings:
        * `Name: InfluxDB`
        * `Query Language: InfluxQL`
        * `URL: http://influxdb:8086`
        * `Access: Server (default)`
        * `Whilelisted Cookies: ` (Leave blank)
        * `Auth` (Leave All Off)
        * `Custom HTTP Headers` (Do not Add any)
        * `Database: telegraf`
        * `User: ` (Use the credential created in [influxdb-init.iql](./Config/influxdb-scripts/influxdb-init.iql)
        * `Password: ` (Use the credential created in [influxdb-init.iql](./Config/influxdb-scripts/influxdb-init.iql)
        * Click "Save & Test"
5. Review panels at the [Grafana plugin list](https://grafana.com/grafana/plugins?type=panel)
    * Note the plugin_id (`https://grafana.com/grafana/plugins/grafana-clock-panel -> grafana-clock-panel`)
6. Install any plugins by running `docker exec -it grafana grafana-cli plugins install $plugin_id`
7. Consider using dashboards. Browse them at the [Grafana dashboard list](https://grafana.com/grafana/dashboards)

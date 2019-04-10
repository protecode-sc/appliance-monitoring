# appliance-monitoring
InfluxDB/Grafana docker-compose example for monitoring BDBA appliance. BDBA appliance has telegraf pre-installed since release 2019.01 and can be configured (System settings -> Monitoring configuration) to use InfluxDB backend from this example.

## Introduction

Here are example configuration files how to setup InfluxDB/Grafana stack to
monitor BDBA appliance. The setup enforces password authentication and uses
TLS to secure InfluxDB connections.

For more relaxed setup with less complexity (no passwords, no TLS), please refer to:

https://github.com/nicolargo/docker-influxdb-grafana/

These examples assume that data files are located in `/srv/appliance-monitoring`.

## Files and directories for influxdb

Influxdb is time-series database that can be used for storing monitoring information
from BDBA appliance.

Influxdb requires data directory for persistency. Examples have that configured as
`/srv/appliance-monitoring/influxdb/data`. Create that directory on the host system.

Certificate and key are needed for TLS. Files are:

* `/srv/appliance-monitoring/influxdb/ssl/influxdb.pem`
* `/srv/appliance-monitoring/influxdb/ssl/influxdb.key`

Example certificates and keys are not provided. Please generate your own and
preferably have appliance trust the root certificate for proper TLS verification.

Configuration that enables TLS is present in `influxdb/etc/influxdb.conf`. Copy that
as `/srv/appliance-monitoring/influxdb/etc/influxdb.conf`.

Finally, `env.influxdb` contains environment variables needed for InfluxDB.
Replace passwords with proper ones and copy it to the same location where
docker-compose is run from. Available variables are documented here:
https://hub.docker.com/_/influxdb

## Files and directories for grafana

Grafana is a monitoring and visualization platform that can acquire data
to be rendered from InfluxDB.

Data directory for grafana is located in `/srv/appliance-monitoring/grafana/data/`.
Since grafana runs as user id `472`, permissions need to be set accordingly.
Run `chown -R 472:472  /srv/appliance-monitoring/grafana/data/` after creation of
data directory.

Grafana configuration example is located in `grafana/etc/grafana.ini`. Edit the file
properly and place it in `/srv/appliance-monitoring/grafana/etc/`.

Options for grafana.ini are documented in http://docs.grafana.org/installation/configuration/.

### Provisioning dashboards and datasources

Sample dashboards can be found from `grafana/etc/dashboards/`. 
Those can be imported to Grafana via the web UI or provisioned when spinning up the container.

To provision, review the files under `grafana/etc/provisioning/` and modify e.g. 
datasource credentials.
* Copy the `grafana/etc/provisioning/` directory to 
`/srv/appliance-monitoring/grafana/etc/provisioning`. 
* Copy the dashboards directory `grafana/dashboards` to 
`/srv/appliance-monitoring/grafana/dashboards`. 

To enable provisioning, uncomment the two relevant volumes from `docker-compose.yml` from the 
service grafana.

# Running

After configuration files are in place, you can start the stack by issuing
`docker-compose up` in directory that has `env.influxdb`.

Example systemd unit file is provided as `docker.influxgrafana.service` which
expect the env variable file to be present in `/srv/appliance-monitoring`.

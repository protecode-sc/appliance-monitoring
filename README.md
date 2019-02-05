# appliance-monitoring
InfluxDB/Grafana docker-compose example for monitoring BDBA appliance

## Introduction

Here are example configuration files how to setup InfluxDB/Grafana stack to
monitor BDBA appliance. The setup enforces password authentication and uses
TLS to secure InfluxDB connections.

For more relaxed setup with less complexity (no passwords, no TLS), please refer to:

https://github.com/nicolargo/docker-influxdb-grafana/

These examples assume that data files are located in /srv/appliance-monitoring.

## Files and directories for influxdb

Influxdb requires data directory for persistency. Examples have that configured as
"/srv/appliance-monitoring/influxdb/data". Create that directory on the host system.

Certificate and key are needed for TLS. Files are:

* /srv/appliance-monitoring/influxdb/ssl/influxdb.pem
* /srv/appliance-monitoring/influxdb/ssl/influxdb.key

Example certificates and keys are not provided. Please generate your own and
preferably have appliance trust the root certificate for proper TLS verification.

Configuration that enables TLS is present in influxdb/etc/influxdb.conf. Copy that
as /srv/appliance-monitoring/influxdb/etc/influxdb.conf.

Finally, env.influxdb contains environment variables needed for InfluxDB.
Replace passwords with proper ones and copy it to the same location where
docker-compose is run from. Available variables are documented here:
https://hub.docker.com/_/influxdb





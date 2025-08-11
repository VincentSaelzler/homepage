---
title: 'Use InfluxDB 2 as a Metric Server for Proxmox'
categories:
    - 'Open Source'
    - Tech
---

## Architecture

InfluxDB will run on in a Docker container.

This setup assumes that a Docker host is accessible on the LAN. In this example, the local domain is `vnet` and the host name is `docker4`.

Therefore, the Docker host FQDN is `docker4.vnet`

## InfluxDB Container

Your USERNAME, PASSWORD, and ORG will vary.

I made my org the same as my local networking domain, but that is **not** a requirement.

```
docker run -d \
     -p 8086:8086 \
     --mount type=volume,source=influx-data,target=/var/lib/influxdb2 \
     --mount type=volume,source=influx-conf,target=/etc/influxdb2 \
     --name idb \
     -e DOCKER_INFLUXDB_INIT_MODE=setup \
     -e DOCKER_INFLUXDB_INIT_USERNAME=vince \
     -e DOCKER_INFLUXDB_INIT_PASSWORD=secret-password \
     -e DOCKER_INFLUXDB_INIT_ORG=vnet \
     -e DOCKER_INFLUXDB_INIT_BUCKET=proxmox \
```

## Connect Proxmox to InfluxDB 2

As of Proxmox 6.4, it is possible to use the version 2 API of Influx.

Before that, Proxmox would connect to Influx 1.x, but it was sending graphite line protocol over UDP (yuck). Now it connects via TCP/HTTP to the Influx API.

### Get the Token

Log into the Influx web user interface, and copy the token for the user.

Click the token name to reveal the token.

[http://docker4.vnet:8086/](http://docker4.vnet:8086/)

![](https://www.saelzler.com/wp-content/uploads/2021/05/InfluxToken.png)There is probably a command-line way to do that, but I don't know it.

### Configure Proxmox Metric Server 

The name can be whatever you want.

Paste the token from the last step into the "Token" field in the Proxmox UI.

![](https://www.saelzler.com/wp-content/uploads/2021/05/ProxmoxMetricServer.png)

Navigate back to the InfluxDB web UI, and confirm that data is flowing. [http://docker4.vnet:8086/](http://docker4.vnet:8086/)

If things are working, there will be some measurements that clearly look related to Proxmox resource utilization.

![](https://www.saelzler.com/wp-content/uploads/2021/05/PopulatedProxmoxDB.png)

In the next post, we will connect the InfluxDB bucket to a pre-built dashboard using Grafana.

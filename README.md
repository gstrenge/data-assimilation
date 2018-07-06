# Data Assimilation - Server [Work in Progress]

## Introduction
> Docker-Compose and configuration files for the deployment of the data assimilation platform.

## Dependencies and Setup - Install
### Docker
Install [Docker](https://docs.docker.com/install/) 
### Docker-Compose
Install [Docker-Compose](https://docs.docker.com/compose/install/)
1. Clone the repo.
```git clone https://github.com/encresearch/data-assimlation.git```

2. Build and run containers. Bare in mind we are still using the **dev** yml file. This file includes the creation of persistent [docker volumes](https://docs.docker.com/storage/volumes/) and contains its own database containers.
```docker-compose -f docker-compose.dev.yml up -d```

To stop and remove containers, networks and images created by up. (External volumes won't be removed)
```docker-compose -f docker-compose.dev.yml down```

## Overview and Usage

### InfluxDB
[InfluxDB](https://www.influxdata.com/) will be used to store our incoming sensor data. For data persistency, a docker volume ```influx_data``` is created. To access the **CLI** thorugh a container, enter ```docker exec -it influxdb influx```. For configuration and environment variables, see [here](https://hub.docker.com/_/influxdb/).
### Grafana
[Grafana](https://grafana.com/) will be used as web application for data visualization. It runs on port 3000 on the container, which is then mapped to port 80 on the host. For data persistency, a volume named ```grafana_data``` is created.
**Configuration**
The configuration file can be found in the ```etc/``` directory. As of now, the only used configuration is pointing the root url to the host's IP. For more info, see [grafana configuration docs](http://docs.grafana.org/installation/configuration/).
### MQTT Connector
Python application that receives raw measurements data from an MQTT Broker, calculates actual values and stores them in InfluxDB. You can find the repo [here](https://github.com/encresearch/mqtt-connector).
### Telegraf
Plugin-driven server agent for collecting and reporting metrics about the hosting server. Data is also stored in InfluxDB. Configuration file can also be found in the ```etc/``` directory.
### Data Analysis [Work in Progress]
Flask microservice that makes continuous queries to the InfluxDB and performs analysis and predictions based on the data. Repo can be found [here](https://github.com/encresearch/data-analysis)

## Contributing
Pull requests and stars are always welcome. To contribute, please fetch, [create an issue](https://github.com/encresearch/data-assimilation/issues) explaining the bug or feature request, create a branch off this issue and submit a pull request.

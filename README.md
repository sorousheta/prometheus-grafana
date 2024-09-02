# Grafana-Mikrotik

![logo](https://repository-images.githubusercontent.com/366494855/c62052b8-17c2-47f2-a3ae-0e397a3ef074)

![visitors](https://visitor-badge.laobi.icu/badge?page_id=IgorKha.Grafana-Mikrotik)
![example branch parameter](https://github.com/IgorKha/Grafana-Mikrotik/actions/workflows/action.yml/badge.svg?branch=master)
![mikrotikOS](https://img.shields.io/badge/Mikrotik_ROS-v7.4-blue)
![Grafana](https://img.shields.io/badge/Grafana-v9.0.5-orange?logo=grafana)
![Prometheus](https://img.shields.io/badge/Prometheus-v2.37.0-red?logo=prometheus)
![snmp_exporter](https://img.shields.io/badge/snmp__exporter-v0.20.0-red?logo=prometheus)


---

## 🐳 Deploy with docker-compose

### Deploy with bash script

```console
git clone https://github.com/sorousheta/prometheus-grafana.git
cd ./prometheus-grafana
bash ./run.sh --config
```

```console
  You can also pass some arguments to script to set some these options:

    --config: change the user and password to grafana and specify the mikrotik IP address

    --stop: stop docker containers

    --help
```

For example:

```console
    bash run.sh --config
```


### deploy with docker-compose manual

1.Change targets ip (192.168.88.1) into file prometheus/prometheus.yml

2.Run

```console
docker-compose up -d
```

3.Open [localhost:3000](http://localhost:3000)

* Grafana login: `admin`

* Password: `mikrotik`

If you want to change the credentials, then edit the ".env" file

---

## Manual deploy

1.add into prometheus.yml

```yml
  - job_name: Mikrotik
    static_configs:
      - targets:
        - 192.168.88.1  # SNMP device IP.
    metrics_path: /snmp
    params:
      module: [mikrotik]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9116  # The SNMP exporter's real hostname:port.
```

2.Configure Prometheus and run /snmp/snmp_exporter

3.Add dashboard <https://grafana.com/grafana/dashboards/14420>

---

### Docker snmp_exporter (deprecated)

[![Docker Pulls](https://img.shields.io/docker/pulls/mashinkopochinko/snmp_exporter_mikrotik?logo=docker)](https://hub.docker.com/repository/docker/mashinkopochinko/snmp_exporter_mikrotik)

> amd64-linux container

```console
sudo docker run -d -p 9116:9116 mashinkopochinko/snmp_exporter_mikrotik:latest
```

---
![img1](/readme/screen.png)

# promtehus installation (9090)

using below  path we can get the link of promteous installation

```
https://prometheus.io/download/
```

for example we are downloading the linux version of promtheus. that link is pasted below

```
https://github.com/prometheus/prometheus/releases/download/v3.7.1/prometheus-3.7.1.linux-amd64.tar.gz
```

* we can install promteus in /opt folder

```
cd /opt
```
```
wget https://github.com/prometheus/prometheus/releases/download/v3.7.1/prometheus-3.7.1.linux-amd64.tar.gz
```
* we can extract the promtheus file

```
tar -xf prometheus-3.7.1.linux-amd64.tar.gz
```
* here in tar -xf ( x is used for extraction, f is used for file name )

* now we want to rename the file 
```
mv prometheus-3.7.1.linux-amd64.tar.gz prometheus
```

* Now we will go in promtheus directory we can see these below files.

* 1. LICENSE 2. NOTICE 3. prometheus 4.prometheus.yaml
5. promtool


* prometheus.yaml is a configuration file
* prometheus is a script

* now we want to kept prometheus service file in server. To continous running purpose.

* Now we want to create prometheus user

```
useradd prometheus
```
* now we want to create prometeus service file to keep continous running purpose in below path 

```
vim /etc/systemd/system/prometheus.service
```

* Now add the below content in prometheus.service file

```
[Unit]
Description=Prometheus Monitoring System
Wants=network-online.target
After=network-online.target

[Service]
#User=prometheus
# if we are using prometheus user. That prometheus user wants some addtional directories required. Those directories we will not creating in prometheus for practising purpose so we will get the error.
# To escape those directory error that purpose we are using root user only for prometheus
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
```
```
systemctl enable prometheus
```
```
systemctl start prometheus
```
```
systemctl status prometheus
```

# NODE EXPORTER INSTALLATION. (AGENT SERVER) port no :- 9100

* we want to install the node exporter agent server in requiredfor remaining nodes

* using below path we can get the link of node exporter installation
```
https://prometheus.io/download/
```

* for example we are downloading the linux version of agent node exporter. that link is pasted below

```
https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
```

* we can install agent node exporter in another server in /opt folder

```
cd /opt
```
```
https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
```

* we can extract the node exporter file in server.

```
tar -xf node_exporter-1.9.1.linux-amd64.tar.gz
```
* rename the file name

```
mv node_exporter-1.9.1.linux-amd64 node_exporter
```

* now we want to create a service file for node exporter

```
vim /etc/systemd/system/node_exporter.service
```

* Now cpoy and paste the node exporter service file

```
[Unit]
Description=Node Exporter Agent
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/opt/node_exporter/node_exporter
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
```
```
systemctl start node_exporter
```
```
systemctl status node_exporter
```
* we will change the prometheus.yaml file according to our requirement like below configuration file.


# If we want the node server metrics we want to change the prometheus.yaml configuration file according to our requiremnt like below file.

```
# my global config
global:
  scrape_interval: 15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          - "localhost:9093"

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  - "alert-rules/*.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["localhost:9090"]
       # The label name is added as a label `label_name=<label_value>` to any timeseries scraped from this config.
        labels:
          app: "prometheus"
          name: "prometheus"
  
  #- job_name: 'ec2_instances'
    # ec2_sd_configs:
    #  - region: 'us-east-1' # Replace with your AWS region
    #   filters:
    #     - name: tag:Monitoring
    #        values: 
    #       - true
    #    port: 9100 # Default Prometheus exporter port
    #relabel_configs:
    #  - source_labels: [__meta_ec2_instance_id]
    #   target_label: instance_id # For easy filtering in alerts
    #  
    #  - source_labels: [__meta_ec2_tag_Name]
    #    target_label: name
    #
    #  - source_labels: [__meta_ec2_private_ip]
    #    target_label: private_ip

   - job_name: "NODE-1"
     static_configs:
       - targets: ["172.31.39.231:9100"]
         labels:
           app: "NODE-1"
           name: "NODE-1"

```

* INSTALLING GRAFANA port no (3000)
#  Here we will install grafana through repo

```
wget -q -O gpg.key https://rpm.grafana.com/gpg.key
```
# sometimes wget cmdwill fail in that case we will use curl cmd

```
curl -O gpg.key https://rpm.grafana.com/gpg.key

```

# create a repo file for grafana in below path

```
vim /etc/yum.repos.d/grafana.repo
```

# now paste the below conten in this path /etc/yum.repos.d/grafana.repo

```
[grafana]
name=grafana
baseurl=https://rpm.grafana.com
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://rpm.grafana.com/gpg.key
sslverify=1
sslcacert=/etc/pki/tls/certs/ca-bundle.crt
```

# To start grafanna

```
systemctl daemon-reload
```
```
systemctl start grafana-server
```
```
systemctl enable grafana-server
```

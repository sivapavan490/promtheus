# promtehus installation

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
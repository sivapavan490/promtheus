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

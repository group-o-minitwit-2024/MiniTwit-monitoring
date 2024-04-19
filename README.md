# MiniTwit-monitoring

Monitoring with Prometheus and Grafana for our [MiniTwit application](https://github.com/group-o-minitwit-2024/MiniTwit).

## Prometheus
Prometheus tracks application metrics. It currently tracks number of requests, response times and CPU load. It runs on `PORT=9000` and gives information to Grafana for visualizing. 

## Grafana
Grafana is used to visualize metrics tracked by Prometheus. It runs on `PORT=3000`, binds volumes from [/grafana/provisioning/](/grafana/provisioning/) and gets environment variabels from [grafana.env](/grafana/grafana.env). The default username and password is `admin`, but the password is overwritten by [grafana.env](/grafana/grafana.env). 
# MiniTwit-monitoring

Monitoring our [MiniTwit application](https://github.com/group-o-minitwit-2024/MiniTwit) with Prometheus and Grafana. This application is currently running on ip 178.62.193.231. Check it out [here](http://178.62.193.231:3000).

## How to run
This service runs with docker compose. To run the monitoring, run
```
docker compose up
```
This will monitor the production MiniTwit application running on `178.62.218.96`. This ip is configured in [prometheus.yml](/prometheus/prometheus.yml). To track a local MiniTwit, run 
```
docker compose -f compose.dev.yml up
```
This will attach prometheus to an existing `prom_net` and monitor the local app. 

Once running, Prometheus can be found on the `localhost:9090` and Grafana can be found on `localhost:3000`.

## Monitoring description
### Prometheus
Prometheus tracks application metrics. It currently tracks number of requests, response times and CPU load. It runs on `PORT=9000` and gives information to Grafana for visualizing. 

### Grafana
Grafana is used to visualize metrics tracked by Prometheus. It runs on `PORT=3000`, binds volumes from [/grafana/provisioning/](/grafana/provisioning/) and gets environment variabels from [grafana.env](/grafana/grafana.env). The default username and password is `admin`, but the password is overwritten by [grafana.env](/grafana/grafana.env). 

## Deployment
To deploy the monitoring stack, we use _Digital Ocean_. The node should use image `docker-20-04`. This can be done with CLI, `doctl`, as such
```
doctl compute droplet create --region ams --image docker-20-04 --size s-1vcpu-1gb --ssh-keys YOUR_FINGERPRINT  minitwit-monitoring
```

From within the droplet: 
* allow traffic on necessary ports
    ```
    ufw allow 2377/tcp
    ufw allow 7946
    ufw allow 4789/udp

    ufw allow 3000

    ufw allow 22
    ```

* clone this repo and start the compose file.
    ```
    git clone https://github.com/group-o-minitwit-2024/MiniTwit-monitoring
    cd MiniTwit-monitoring
    ```

* Run the monitoring stack with 
    ```
    docker compose up -d
    ```
    or do it remotely with 
    ```
    ssh -i PATH_TO_SSH_KEY root@DROPLET_IP -t "cd /root/MiniTwit-monitoring && docker compose up -d"
    ```
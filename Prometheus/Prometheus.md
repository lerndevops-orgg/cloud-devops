#Installation Steps for Prometheus
- Launch an EC2 instance with Ubuntu and run the below commands

```
sudo su -
git clone https://github.com/SimplyLearner/cloud-devops
cd cloud-devops/Prometheus/scripts
./1-install.sh
ps aux | grep prometheus
sudo service prometheus start
sudo service prometheus status


./3-install-grafana.sh


sudo service grafana-server status


ps uax | grep prometheus

cat /etc/prometheus/prometheus.yml

```
# Install Node Exporter
- Run the script
```
sudo apt-get install vim
sudo update-alternatives --config vi

vim /etc/prometheus/prometheus.yml
  - job_name: 'node_exporter'
    scrape_interval: 5s
    static_configs:
      - targets: ['localhost:9100']
kill -HUP reload prometheus
```


```
localhost:9090 --> Prometheus
localhost:3000 --> grafana
curl localhost:9100 --> node exporter
curl localhost:9100/metrics
```


- setup Alert manager
# Create  a new file
vim /etc/alertmanager/alertmanager.yml

global:
  smtp_smarthost: 'localhost:25'
  smtp_from: 'alertmanager@prometheus.com'
  smtp_auth_username: ''
  smtp_auth_password: ''
  smtp_require_tls: false

templates:
- '/etc/alertmanager/template/*.tmpl'

route:
  repeat_interval: 1h
  receiver: operations-team

receivers:
- name: 'operations-team'
  email_configs:
  - to: 'operations-team+alerts@example.org'
  slack_configs:
  - api_url: https://hooks.slack.com/services/XXXXXX/XXXXXX/XXXXXX
    channel: '#prometheus-course'
    send_resolved: true


# Do Necessary Edits to Prometheus config file



vim /etc/prometheus/prometheus.yml
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      - localhost:9093

service prometheus restart
service prometheus status


```
scripts/install-docker.sh
docker-compose up -d


localhost:5000 --> Flask server
localhost:5000/query --> query
localhost:8000 --> monitoring

#Run the script
add-flask-app.sh

```

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(<kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client --output=yaml
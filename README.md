# Grafana-loki-k8s

Install using APT or RPM package manager

# Welcome to Grafana Labs's package repository
```
mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor > /etc/apt/keyrings/grafana.gpg
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | tee /etc/apt/sources.list.d/grafana.list
```

# Install Loki and Promtail
Using apt-getCopy

```
       apt-get update
       apt-get install loki promtail
```

# Install Grafana

# Complete the following steps to install Grafana from the APT repository:

1. Install the prerequisite packages:
```
    sudo apt-get install -y apt-transport-https software-properties-common wget
```
2. Import the GPG key:
```
    sudo mkdir -p /etc/apt/keyrings/
    wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
```
3. To add a repository for stable releases, run the following command:    
```
   echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
4. To add a repository for beta releases, run the following command:
```       
   echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com beta main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```
5. Run the following command to update the list of available packages:
       # Updates the list of available packages
```
       sudo apt-get update
```
 6. To install Grafana OSS, run the following command:
 Installs the latest OSS release:
```
       sudo apt-get install grafana
```

# Start the Grafana server with systemd

Complete the following steps to start the Grafana server using systemd and verify that it is running.
1. To start the service, run the following commands:
```
    sudo systemctl daemon-reload
    sudo systemctl start grafana-server
    sudo systemctl status grafana-server
```
2. To verify that the service is running, run the following command:
```
       sudo systemctl status grafana-server
```
Configure the Grafana server to start at boot using systemd
To configure the Grafana server to start at boot, run the following command:
```
sudo systemctl enable grafana-server.service
```
# Edit the config.yml of Promtail
```
cd /etc/promtail/
nano config.yml 
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /tmp/positions.yaml

clients:
- url: http://localhost:3100/loki/api/v1/push

scrape_configs:
- job_name: system
  static_configs:
  - targets:
      - localhost
    labels:
      job: varlogs
      #NOTE: Need to be modified to scrape any additional logs of the system.
      __path__: /var/log/*/*/*/*log <path-logs>

# Edit the promtail.service file for root access
```
cd /etc/systemd/system
nano promtail.service 
systemctl daemon-reload
systemctl restart promtail.service
systemctl status promtail.service
```

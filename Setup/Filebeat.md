# Filebeat Setup
This document defines the standardized installation and configuration of **Filebeat** across all Linux-based VMs in the SOC infrastructure.
| VM   | Description                      | Log Source(s)                                |
|------|----------------------------------|----------------------------------------------|
| VM1  | ELK + MISP                       | Internal system logs            |
| VM2  | TheHive + Cortex + Web Server    | Apache2 logs and system logs                 |
| VM3  | Privoxy + DFIR      | Privoxy access logs and system logs 
## 1. Installation (Linux VMs)

Add Elastic APT Repository:

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt install apt-transport-https

echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
```
Install Filebeat:
```bash
sudo apt install filebeat -y
```
## 2. Configuration
edit this file: /etc/filebeat/filebeat.yml
Send logs to Elasticsearch on VM1
```bash
output.elasticsearch:
  hosts: ["http://<ELK_VM1_IP>:9200"]
  username: "elastic"
  password: "<your_password>"
```
## 3.Define logs to collect per VM
### For vm1
```bash
filebeat . inputs:
- type: log
id: my-filestream-id
enabled: true
paths:
- /var/log/ *. log
```
### For vm2
```bash
filebeat . inputs:
- type: log
id: my-filestream-id
enabled: true
paths:
- /var/log/ *. log
- /var/log/apache2/ *. log
```
### For vm3
```bash
filebeat . inputs:
- type: log
id: my-filestream-id
enabled: true
paths:
- /var/log/ *. log
- /var/log/privoxy/log*
```

## 4. Enable Filebeat modules and set up Kibana dashboards
```bash
sudo filebeat modules enable system
sudo filebeat setup -e
sudo filebeat setup --dashboards
sudo filebeat setup --template
sudo filebeat setup --index-management
```
## 5. Start Filebeat
```bash
sudo systemctl enable filebeat
sudo systemctl start filebeat
```

## 6. Verify in Kibana
Go to Kibana > Discover

Set your time range (top right)

Look for logs from the filebeat-* index






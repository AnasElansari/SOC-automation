## ELK Stack Setup
Install Elasticsearch
```bash
sudo apt update
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```
Then update package lists:
```bash
sudo apt update
```
Install ELK Stack (Elasticsearch + Kibana)
```bash
sudo apt install -y elasticsearch kibana
```
Then edit /etc/elasticsearch/elasticsearch.yml:
```bash
network.host: 0.0.0.0
http.port: 9200
discovery.type: single-node
xpack.security.enabled: true
xpack.security.authc.api_key.enabled: true
```
Then enable and start the service:
```bash
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

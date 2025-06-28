# ELK Stack Setup
### 1. Install and Configure Elasticsearch
Update apt & Add Elasticsearch GPG Key and Repo
```bash
sudo apt update
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```
Install Elasticsearch
```bash
sudo apt install elasticsearch -y
```
Then update package lists:
```bash
sudo apt update
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
Verify it's working by browsing in the localhost on your browser or but runing this command:
```bash
curl -X GET http://localhost:9200
```
Set Up Passwords for ELK Stack Users
there are 2 ways to do this either auto or manually: </br>
- The auto-way is: 
```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-setup-passwords auto
```
- The manual way:
```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
```
and then this will prompt you to choose your own password for each account: kibana,elastic,kibana_system, etc... .
### 2. Install and Configure Kibana
Install kibana
```bash
sudo apt install kibana -y
```
Add or edit these lines in /etc/kibana/kibana.yml:
```bash
server.host: "0.0.0.0"
elasticsearch.hosts: ["https://localhost:9200"]
elasticsearch.username: "kibana_system"
elasticsearch.password: "<The-password-from-the-step-before>"
server.ssl.enabled: false
```
Start and enable Kibana:
```bash
sudo systemctl enable kibana
sudo systemctl start kibana
```
Access Kibana via this link in your browser: </br>
http://localhost:5601 : instead of localhost you can put the vm IP
</br> 
#### NB. in the links should replace "localhost" with the actual ip address of the elk-misp machine not the host


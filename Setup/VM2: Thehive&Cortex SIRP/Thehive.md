# TheHive installation and configuration:
## 1.Install java (required for Cassandra)
```bash
sudo apt update
sudo apt install openjdk-8-jre-headless -y
echo 'JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"' | sudo tee -a /etc/environment
source /etc/environment
```
## 2. Install Apache Cassandra
### Add the repo and install Cassandra:
```bash
curl -fsSL https://www.apache.org/dist/cassandra/KEYS | sudo apt-key add -
echo "deb http://www.apache.org/dist/cassandra/debian 311x main" | sudo tee -a /etc/apt/sources.list.d/cassandra.sources.list
sudo apt update
sudo apt install cassandra -y
```
### Start Cassandra and verify:
```bash
sudo systemctl enable cassandra
sudo systemctl start cassandra
nodetool status
```
### change cluster name (this is just optional):
```bash
cqlsh
UPDATE system.local SET cluster_name = 'thehive-cluster' WHERE key='local';
```
## 3. Install TheHive 4
### Add TheHive Project repository:
```bash
echo 'deb https://deb.thehive-project.org release main' | sudo tee /etc/apt/sources.list.d/thehive-project.list
wget -qO - https://raw.githubusercontent.com/TheHive-Project/TheHive/master/PGP-PUBLIC-KEY | sudo apt-key add -
sudo apt update
```
### Install TheHive4:
```bash
sudo apt install thehive4 -y
```
### 4.Start and Enable TheHive4
```bash
sudo systemctl enable thehive
sudo systemctl start --now thehive
```
### Now access TheHive Web UI
•	Open browser and go to: http://<your_VM2_IP>:9000
•	First-time setup: you'll create an admin user, organization, and other roles.
### Now add the API KEYS of CORTEX AND MISP after installing them and configuring them
### Setup Case Templates
•	Go to Settings → Case Templates → Add Template <br>
•	Define steps for specific incident types (e.g. Ransomware, Phishing, etc.)

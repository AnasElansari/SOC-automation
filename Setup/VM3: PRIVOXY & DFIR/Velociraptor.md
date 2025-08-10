# Velociraptor SOC – Server & Client Setup
## 1) Install velociraptor
```bash
# 1) Download the Linux binary
cd /tmp
curl -LO https://github.com/Velocidex/velociraptor/releases/download/v0.6.4-2/velociraptor-v0.6.4-2-linux-amd64
chmod +x velociraptor-v0.6.4-2-linux-amd64

# 2) Generate server + client configs (interactive)
sudo ./velociraptor-v0.6.4-2-linux-amd64 config generate -i
```
During the interactive wizard set (key choices): <br>
•	Use self signed SSL: true <br>
•	Frontend host/IP: <VM3_ip> <br>
•	Frontend bind port: 8000 (client comms) <br>
•	GUI bind address: 0.0.0.0 <br>
•	GUI port: 8889 (web console) <br>
•	Create an admin user + password when prompted. <br>
This produces: server.config.yaml and client.config.yaml.

```bash
# 3) Build a .deb for the server and install as a service
sudo ./velociraptor-v0.6.4-2-linux-amd64 --config server.config.yaml debian server --binary ./velociraptor-v0.6.4-2-linux-amd64
sudo dpkg -i velociraptor_*_server.deb

# 4) Start & enable
sudo systemctl enable velociraptor_server
sudo systemctl start velociraptor_server
```
you need to edit later: /etc/velociraptor/server.config.yaml

```bash
GUI:
  bind_address: 0.0.0.0
  bind_port: 8889
Frontend:
  hostname: <VM3_ip>
  bind_port: 8000
API:
  bind_address: 127.0.0.1
  bind_port: 8001
```

Log in to the web UI: https://<VM3_ip>:8889 <br>
(accept the self‑signed cert) and sign in with the admin user.<br>
2) Add a Linux Client 
On the server, create a Linux client package:
```bash
sudo ./velociraptor-v0.6.4-2-linux-amd64 --config server.config.yaml debian client --binary ./velociraptor-v0.6.4-2-linux-amd64
# copy the resulting .deb to the target Linux host
sudo dpkg -i velociraptor_*_client.deb
sudo systemctl enable velociraptor_client
sudo systemctl start velociraptor_client

```

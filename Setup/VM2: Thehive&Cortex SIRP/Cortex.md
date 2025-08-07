# Installing and configuring Cortex
## 1. Add Cortex Repository & Install
Add repo and GPG key:
```bash

echo 'deb https://deb.thehive-project.org stable main' | sudo tee /etc/apt/sources.list.d/thehive-project.list
wget -qO - https://raw.githubusercontent.com/TheHive-Project/TheHive/master/PGP-PUBLIC-KEY | sudo apt-key add -
sudo apt update
```
Install Cortex:
```bash
sudo apt install cortex -y
```
## 2. Configure Cortex
Edit the config file:
```bash
sudo nano /etc/cortex/application.conf
```
Minimum required config (adjust as needed):
```bash
http.port = 9001
play.http.secret.key = "choose_a_SecretKey"
```
## 3. Start Cortex
```bash
sudo systemctl enable --now cortex
```
Web interface visit:
http://<your_VM2_IP>:9001
## 4. Install Analyzers & Responders
Cortex uses analyzers (to enrich observables) and responders (to take action).
Clone the Analyzer pack:
```bash
cd /opt
sudo git clone https://github.com/TheHive-Project/Cortex-Analyzers.git
sudo chown -R cortex:cortex Cortex-Analyzers
```
Install required dependencies:
```bash
cd Cortex-Analyzers
for i in $(find . -name "requirements.txt"); do sudo -H pip3 install -r $i || true; done
```
## 5. Set Analyzer Paths
Edit the config again:
```bash
sudo nano /etc/cortex/application.conf
```
Add this paths:
```bash
analyzer {
  path = ["/opt/Cortex-Analyzers/analyzers"]
}
responder {
  path = ["/opt/Cortex-Analyzers/responders"]
}
```
## 6. Enable and Configure Analyzers (e.g. VirusTotal, Shodan)
In Cortex Web UI:
1.	Go to "Analyzers" tab
2.	Click Edit → enter API keys (For me i used VirusTotal, AbuseIPDB, and shoddan, you can choose anything you want and add the details required to add them.)
3.	Save and test
## 7. Integrate with TheHive
From Cortex Web UI:
1.	Create a Cortex user and an API key
2.	In TheHive, edit /etc/thehive/application.conf:
```bash
cortex {
  servers = [
    {
      name = "cortex"
      url = "http://<cortex_IP>:9001"
      key = "<your_Cortex_API_key>"
      maxConcurrentJobs = 5
      jobTimeout = 10 minutes
    }
  ]
}
```
3.	Restart TheHive:
```bash
sudo systemctl restart thehive
```

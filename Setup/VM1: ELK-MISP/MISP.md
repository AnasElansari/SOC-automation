# Installing and configuring MISP
## 1. Download and Run the Official MISP Install Script
usin the official MISP installer:
```bash
wget --no-cache -O /tmp/INSTALL.sh https://raw.githubusercontent.com/MISP/MISP/2.4/INSTALL/INSTALL.sh

bash /tmp/INSTALL.sh -A
```
Along the way you are going to be asked to allow creation of a misp user and you will be prompted to give a password for it 
## 2. Set Up Web Access
Once the script finishes:

MISP is accessible from:
http://localhost/
The default login cridentials are: admin@admin.test with password admin </br>
Once logged in you will be asked to change password 
## 3.Integrate MISP with TheHive
This allows automatic alert ingestion into TheHive from MISP.
1.	In MISP, go to:
Administration → Automation → Add new API key
2.	Copy the generated API key.
3.	In TheHive, edit this file /etc/thehive/application.conf:
```bash
sudo nano /etc/thehive/application.conf
```
Add this:
```bash
misp {
  servers = [
    {
      name = "misp"
      url = "https://localhost"
      key = "<YOUR_MISP_API_KEY>"
      certPath = ""
      tag = "misp"
      enabled = true
    }
  ]
}
```
4.	Restart TheHive:
```bash
sudo systemctl restart thehive
```
5. Verify MISP Alerts in TheHive<br>
•	Go to TheHive → Alerts<br>
•	You should see automatic case suggestions from MISP data (from synced feeds or manual input)

#### NB. in the link should replace "localhost" with the actual ip address of the elk-misp machine not the host 

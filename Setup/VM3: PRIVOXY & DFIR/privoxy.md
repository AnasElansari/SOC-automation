# Install and Configure Privoxy (Web Proxy)
### Install Privoxy
```bash
sudo apt update
sudo apt install privoxy -y
```
### Configure Privoxy
```bash
sudo nano /etc/privoxy/config
```
uncomment the next lines and make sure they are there if you can't find them:
```bash
listen-address  0.0.0.0:8118
logfile /var/log/privoxy/logfile
```

### Enable detailed logging:
```bash
debug   1 # Log the destination for each request Privoxy let through. See also debug 1024.
debug  64 # debug regular expression filters
debug 128 # debug redirects
debug 1024 # Log the destination for requests Privoxy didn't let through, and the reason why.
debug 4096 # Startup banner and warnings.  
```
### Route Browser Traffic Through Proxy
I chosed firefox browser, and choose manual proxy and fill it with the next parameters:
```bash
•	Proxy: http://<VM3_IP>:8118
•	Port: 8118
•	Protocol: HTTP

```
and create a file i named it whitelis.action which you will customize for the SME's actual needs:

<img width="1190" height="451" alt="image" src="https://github.com/user-attachments/assets/39df5afc-e853-4cf2-bf28-9a22b9fcb5ee" />
and add it inside the config file that s located /etc/privoxy/config  <br> <br> 
<img width="1432" height="229" alt="image" src="https://github.com/user-attachments/assets/8ea040b6-e0bb-472e-9d6c-c5c84723b6ec" />

### Enable and restart service:
```bash
sudo systemctl enable privoxy
sudo systemctl restart privoxy
```

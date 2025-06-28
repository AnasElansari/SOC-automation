### 1. Download and Run the Official MISP Install Script
usin the official MISP installer:
```bash
wget --no-cache -O /tmp/INSTALL.sh https://raw.githubusercontent.com/MISP/MISP/2.4/INSTALL/INSTALL.sh

bash /tmp/INSTALL.sh -A
```
Along the way you are going to be asked to allow creation of a misp user and you will be prompted to give a password for it 
### 2. Set Up Web Access
Once the script finishes:

MISP is accessible from:
http://localhost/
The default login cridentials are: admin@admin.test with password admin </br>
Once logged in you will be asked to change password 
</br>
#### NB. in the link should replace "localhost" with the actual ip address of the elk-misp machine not the host 

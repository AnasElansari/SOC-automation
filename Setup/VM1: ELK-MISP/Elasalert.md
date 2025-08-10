# Installing and configurin ELastalert
### Prerequisites:
### Ensure Python 3 and pip are installed:
```bash
sudo apt update
sudo apt install python3 python3-pip -y
```
## 1. Install ElastAlert2:
```bash
pip install elastalert2
```
## Create index in Elasticsearch for ElastAlert:
```bash
elastalert-create-index
```
## 2. Configure "config.yaml"
inside the /opt/elastalert/ directory 
```bash
nano config.yaml
```
this is a sample of the config.yaml and should be customized depending on your machines specs:
```bash
es_host: localhost
es_port: 9200
use_ssl: false
verify_certs: false
writeback_index: elastalert_status

run_every:
  minutes: 1

buffer_time:
  minutes: 15

alert_time_limit:
  days: 2
```
## 3.create and Alert rule "rule.yaml"
This will define what alerrt you want and how they will work. <br>
This is just my example you can customize it or modify it to your own cridentials and api key.
You can add as many rules as you want.
```bash
name: Active SIEM signal -> TheHive
index: ".siem-signals-default*,.alerts-security.alerts-default*"
type: any
realert:
  minutes: 0

filter: []      # debug: match everything in those indices

alert: hivealerter
hive_connection:
  hive_host: http://<your own address>
  hive_port: 9000
  hive_apikey: <your own api key>

hive_alert_config:
  title: "ElastAlert: {0}"
  title_args:
    - signal.rule.name
  description: "{0}"
  description_args:
    - signal.reason
  type: "external"
  source: "elastalert"
  severity: 2
#  severity_args:
#    - signal.rule.severity
  tlp: 3
  status: "Neu"
  follow: True

```
The next command is how to run elastalert to receive the alerts.
```bash
python3 -m elastalert.elastalert --config path_to_config.yaml --rule Path_to_rule.yaml --verbose
```

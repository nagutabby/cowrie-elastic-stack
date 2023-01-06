# cowrie-elastic-stack
Build honeypot with Cowrie, and Analyze the logs with Elastic Stack

## Configuration
* Docker Compose
* Cowrie SSH/Telnet honeypot
* Elastic Stack
  * Elasticsearch
  * Kibana
  * Filebeat
  * Logstash

## System requirements
* 2GB RAM
* 20GB free Space

## How to build the environment
1. Update the package to the latest version

```bash
sudo apt update && sudo apt upgrade -y
```

2. Change the port number when logging in via SSH

```bash
sudo vim /etc/ssh/sshd_config
```

Find the line where `#Port 22` is written, and rewrite it as `Port 22222`

Reload ssh.service to reflect the change

```bash
sudo systemctl reload ssh
```

3. Install Git

```bash
sudo apt install -y git
```

4. Install Docker Compose

Please see here: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

5. Clone this repository, and change to the directory

```bash
git clone https://github.com/nagutabby/cowrie-elastic-stack.git
cd cowrie-elastic-stack
```

6. Create a directory and files

You will need to create the following directory and files:

* directory
  * cowrie/log/
* files
  * cowrie/log/cowrie.json
  * cowrie/config/cowrie.cfg
  * cowrie/config/userdb.txt


```bash
mkdir cowrie/log/
touch cowrie/log/cowrie.json
sudo chmod o+w cowrie/log/cowrie.json
touch cowrie/config/cowrie.cfg
touch cowrie/config/userdb.txt
```

Here is an example of the contents of the file:

* cowrie.cfg

```config
[telnet]
enabled = True
# listen 23/tcp and 2323/tcp
listen_endpoints = tcp:23:interface=0.0.0.0 tcp:2323:interface=0.0.0.0

[ssh]
# listen 22/tcp and 2222/tcp
listen_endpoints = tcp:22:interface=0.0.0.0 tcp:2222:interface=0.0.0.0

[output_virustotal]
enabled = True
api_key = [Your API key]
upload = True
scan_file = True
scan_url = True
```

* userdb.txt

```txt
# Example userdb.txt 
# This file may be copied to etc/userdb.txt.
# If etc/userdb.txt is not present, built-in defaults will be used.
#
# ':' separated fields, file is processed line for line
# processing will stop on first match
# 
# Field #1 contains the username
# Field #2 is currently unused
# Field #3 contains the password 
# '*' for any username or password
# '!' at the start of a password will not grant this password access
# '/' can be used to write a regular expression 
#
root:x:*
ubuntu:x:*
pi:x:*
admin:x:*
www:x:*
```

7. Build and run containers

```bash
docker compose up
```

You can run the containers as a daemon with the following command:

```bash
docker compose up -d
```

8. Access Kibana in your browser

You can access Kibana by typing `http://[IP address]:5601` in your browser.

## Tip

You can check whether Elasticsearch works properly with the following command:

```bash
curl http://[IP address]:9200
```

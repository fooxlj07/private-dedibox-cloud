Set /etc/hosts local
<ip_server01> server01
<ip_server02> server02
<ip_server03> server03

# Set up env
pipenv install
pipenv shell

`ansible-galaxy collection install -r requirements.yml`

# Install proxmox from debian
```
ansible-playbook -l 'server02' --tags install proxmox.yml
```

Or can be seperate
```
ansible-playbook --tags pre_config proxmox.yml

ansible-playbook --tags apt_install proxmox.yml

ansible-playbook --tags post_config proxmox.yml
```

## Check if it is proper installed 
ssh in the server check if proxmox is properly installed, and to check the poxmox.
```
nft add rule inet filter input tcp dport 8006 accept
```
Then check the web https://<ip_of_server>:8006

# Setup firewall
It is essetial to keep your servers safe

It will only allow your local host machine and servers among each other to be able to connect, the other request will be rejected.

```
ansible-playbook -vvv -i inventory.ini --ask-become-pass playbooks/firewall.yml
```
It will show, please input your password(same pwd when you do sudo su) 
```
BECOME PASS:
```

# Setup private network

Create tinc mesh vpn
```
ansible-playbook playbooks/network.yml --tags tinc

```
## Check if connections among servers 
i.e 
From server01 
```
ping server02
ping server03
```

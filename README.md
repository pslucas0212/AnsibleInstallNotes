# Ansible Installation Notes
- Updated 2021-04-21

## Pre-reqs
- Virtual Machine Setup - 2 vCPUs, 8GB RAM, 180GB Disk
- Network configuration (static ip, hostname, DNS, gateway) and DNS update
- RHEL 8.3
- yum -y update 

## Installation
- Download Internet connected installation package - [https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz](https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz)

      # tar xvzf ansible-tower-setup-latest.tar.gz
      # cd ansible-tower-setup-3.8.2-1
      
      
- Update inventory file according to your installation preference. **Note:** This current installation doesn't include Automation Hub
- I used the single node inventory
- 
## References
[Ansible Automation Platform Quick Installation Guide v3.8.2](https://docs.ansible.com/ansible-tower/latest/html/quickinstall/index.html)



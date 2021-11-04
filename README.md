# Ansible Installation Notes
- Updated 2021-11-04. 

My notes for setting up Ansible in a lab environment.  My lab enviroment consists of three vSphere x86 8 core, 64GB RAM hosts.

## Pre-reqs
- Virtual Machine Setup - 2 vCPUs, 8GB RAM, 200GB Disk
- Network configuration (static ip, hostname, DNS, gateway) and update DNS records
- RHEL 8.4 with simple content access enabled
  - Register your RHEL server, enable repos, and update all packages
 ```
 # subscription-manager register --org=<your org id> --activationkey=<you activation key>
 # subscription-manager status
 # insights-client --enable
 # insights-client --register
 # subscription-manager repos --list | grep ansible
 # subscription-manager repos --enable ansible-2.9-for-rhel-8-x86_64-rpms
 # subscription-manager repos --enable rhel-8-for-x86_64-baseos-rpms
 # subscription-manager repos --enable rhel-8-for-x86_64-appstream-rpms
 # subscription-manager repos --list-enabled
 # yum -y update 
```

## Installation
If you are installing the latest release of Ansible Automation Platform (AAP), you no longer need to install Ansible first.  The AAP bundle will install Ansible for you. 

Download the latest AAP Internet connected installation package - [https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz](https://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-latest.tar.gz)

```
# tar xvzf ansible-tower-setup-latest.tar.gz
# cd ansible-tower-setup-3.8.4-1
```
Update inventory file according to your installation preference. **Note:** This current installation doesn't include Automation Hub
I created a single node installation with tower and the database installed on the same node
Run the setup installation script setup.sh from the unzipped directory above

```
# ./setup.sh
```
      
- You are now able access your Tower installation at http://localhost/.  You will receive a redirect to port 443.  
- If this is the first time you log into Ansible Tower, you will need to provide a subscription manifest

## Assigning Subscription
- You have a couple of options for adding endpoint subscriptions to your Ansible Tower instance.
- I chose to generate a manifest file and import that into my Ansible Tower instance.  You would create a Satellite manifest.  For this installation, I chose Satellite 6.9 and added the appropriate subscription and the number of endpoints I wanted to manage.

## References
[Ansible Automation Platform Quick Installation Guide - Latest Version](https://docs.ansible.com/ansible-tower/latest/html/quickinstall/index.html)

 

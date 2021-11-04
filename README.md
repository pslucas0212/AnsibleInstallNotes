# Ansible Installation Notes
- Updated 2021-11-04. 

My notes for setting up Ansible in a lab environment.  My lab enviroment consists of three vSphere x86 8 core, 64GB RAM hosts.

## Pre-reqs
Virtual Machine Setup - 2 vCPUs, 8GB RAM, 200GB Disk
Network configuration (static ip, hostname, DNS, gateway) and update DNS records
RHEL 8.4 with simple content access enabled

Register your RHEL server with subscriptiom manager and Insights.  Enable repos for RHEL 8 and Ansible.  Update all packages.
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
Update inventory file according to your installation preference. See the following inventory file snippet.
```
[tower]
localhost ansible_connection=local

[automationhub]

[database]

[all:vars]
admin_password='<YourPasswordHere>'

pg_host=''
pg_port=''

pg_database='awx'
pg_username='awx'
pg_password='<YourPasswordHere>'
pg_sslmode='prefer'  # set to 'verify-full' for client-side enforced SSL
```
I created a single node installation with tower and the database installed on the same node
Run the setup installation script setup.sh from the unzipped directory above

```
# ./setup.sh
```
Here is an excerpt of the installation output you will seen on your terminal.  Look for the "The setup process completed successfully." message.
```
Updating Subscription Management repositories.
Ansible Tower Dependencies Repository -         1.4 MB/s | 523 kB     00:00    
Last metadata expiration check: 0:00:01 ago on Thu 04 Nov 2021 03:57:43 PM CDT.
Dependencies resolved.
...
PLAY RECAP *********************************************************************
localhost                  : ok=175  changed=86   unreachable=0    failed=0    skipped=86   rescued=0    ignored=2   

The setup process completed successfully.
Setup log saved to /var/log/tower/setup-2021-11-04-15:57:40.log.
```
      
You are now able access your Tower installation at http://localhost/.  You will receive a redirect to port 443.  
If this is the first time you log into Ansible Tower, you will need to provide subscription information.  If you have purchased an Ansible subscription, you can enter the subscription information in one of two ways.
1. You can create a manifest file on your Red Hat Customer portal and import the manifest file into Ansible Tower.
2. You can directly login to the Red Hat Customer portal and directly select your AAP subscription.

You might want to choose to use a manifest file for managing your Ansible subscription because you don't have to create a specific user and user password to enable the subscription (and manage the user).

## Creating a Ansible Manifest File

Go to [http://access.redhat.com](http://access.redhat.com) and clock the Login button.  Filling the login prompt with your Red Hat login or email and click the redt Next button.  Fill in your password and click the red Login button.

![Click Login button](/images/aap01.png)

Now click the Subscriptions tab.

![Click Subscriptions tab](/images/aap02.png)

On the Subscriptions page, click the Subscription Allocations tab.  On the Subscription Allocations tab, click the black New Subscription Allocation button.

![Click Subscription Allocation then New Subscription Allocation button](/images/aap03.png)

On the Create New Subscription Allocation page, fill in the Name text field, choose Satellite 6.9 from the drop down list and click the blue reate button.  Note: The Type choice for the manifest doesn't matter.  I chose the latest GA version of Satellite for this example.

![Complete News Subscription Allocation](/images/aap04.png)

On the Subcription Allocations >> app_prod_manifest page, click the Subscriptions tab and then click the blue Add Subscriptions button.

![Add Subscriptions](/images/aap05.png)

On the Add Subscriptions to app_prod_manifest page, scroll down to your Red Hat Ansbile Automation Platform subscription and chose the number of nodes you want to mange with this Ansible Tower instance.

![Chose number of Nodes](/images/app06.png)

Scroll down to the bottom of the page and click the black Submit button to create the manifest file.

On the Subcription Allocations >> app_prod_manifest page, click the black Export Manifest button to download the mandifest file to your computer.

![Export Manifest](/images/aap07.png)

## References
[Ansible Automation Platform Quick Installation Guide - Latest Version](https://docs.ansible.com/ansible-tower/latest/html/quickinstall/index.html)

 

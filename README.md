# automation-vlan-setup

## Prerequisites

We need to :
* Have a functional GNS3VM and running on your computer
* Download the image of IOSL2 Cisco Switch (on this URL: https://drive.google.com/file/d/1Vxs8U-MUK7UuC8saGPuS-mgCVe4QBJQ-/view?usp=drive_link) and install the appliance on your GNS3VM
* Make this infrastructure on GNS3

![Alt text](infrastructure_gns3.png?raw=true "Optional Title")

## Setup

### Switches

Add an address IP on your switch following this configuration :
```
conf t
int vlan1
ip add 192.168.122.240 255.255.255.0
no shut
```

Configure the switch for SSH access : 
```
conf t
hostname SW1
ip domain-name <your_domain>
crypto key generate rsa modulus 2048
username <user_remote> privilege 15 secret <your_password>
line vty 0 4
 transport input ssh
 login local
 exit
```

### Ansible configuration
Replace the `<user_remote>` on the "ansible.cfg" file and add your switch SSH user :

```
[defaults]
inventory = inventory
remote_user = <user_remote>
```

> [!IMPORTANT]  
> Check the setup of switch interfaces on the playbook because this may change depending on the switch model

## Launch of the playbook
Once everything is set up properly, you can launch the playbook with the following command :

```
ansible-playbook playbook.yml -k
```

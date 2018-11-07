# Ansible Basics with Junos

[![Build Status](https://travis-ci.org/tplisson/ansible-basics.svg?branch=master)](https://travis-ci.org/tplisson/ansible-basics)

## Documentation Structure

[**1. Quick Ansible Demo**](README.md#1.-Quick-Ansible-Demo)

[**2. More Advanced Stuff**](README.md#2.-More-Advanced-Stuff)


# 1. Quick Ansible Demo
This is a short demo on the Ansible Galaxy module for Juniper Junos

More on the Ansible Galaxy module for Juniper Junos here:
 - https://junos-pyez.readthedocs.io/en/stable/
 - https://www.juniper.net/documentation/en_US/junos-pyez/information-products/pathway-pages/junos-pyez-developer-guide.html


---
## 1.1 Start a Docker container for the Ansible environment
Using a Docker container greatly simplifies the environment setup for Python, PyEz and Ansible... It also keeps things clean and contained

```
docker run -it --rm -v $(pwd):/project juniper/pyez-ansible ash
```

Check the Ansible version
```
ansible-version
```
Check the version of the "Juniper.junos" Ansible Galaxy module
```
more /etc/ansible/roles/Juniper.junos/version.py
```


See Docker Hub for more info:
- https://hub.docker.com/r/juniper/pyez-ansible/

If you need access to Junos devices to test, you can run vsrx or vqfx vagrant images

- vSRX image on Vagrant Cloud:
    - https://app.vagrantup.com/juniper/boxes/ffp-12.1X47-D15.4-packetmode
- vQFX image on Vagrant Cloud:
    - https://app.vagrantup.com/juniper/boxes/vqfx10k-re
- Examples of multi VMs topologies:
    - https://github.com/Juniper/vqfx10k-vagrant



## 1.2 Setup the Environment

Inventory "hosts" file located in my project directory
```ini
[mx]
mx1
mx2
```

Credentials for the default Junos devices username and password can be stored under:
- group_vars/all.yml
```ini
---
# Default usernae and password
device_user: lab
device_pass: lab123
```

## 1.3 Writing an Ansible Playbook

Ansible Playbook file called pb-facts.yml
```yaml
---
  - name: Gather Facts
    hosts: mx
    roles:
     - Juniper.junos
    connection: local
    gather_facts: no

    tasks:
    - name: Checking NETCONF connectivity
      wait_for: host={{ inventory_hostname }} port=830 timeout=5

    - name: Retrieve information from devices running Junos OS
      juniper_junos_facts:
        host: "{{ inventory_hostname }}"
        #user: "lab"
        #passwd: "lab123"
        user: "{{ device_user }}"
        passwd: "{{ device_pass }}"
      register: junos
    
    - name: Print Junos Software version
      debug: 
        msg="{{ junos.version }}"
```
Expected Output
```
docker run -it --rm -v $(pwd):/project juniper/pyez-ansible ash

/project # ansible-playbook -i hosts pb-facts.yml

PLAY [Gather Facts] *************************************************************************************************************************************************

TASK [Checking NETCONF connectivity] ********************************************************************************************************************************
ok: [mx2]
ok: [mx1]

TASK [Retrieve information from devices running Junos OS] ***********************************************************************************************************
ok: [mx1]
ok: [mx2]

TASK [Print Junos Software version] *********************************************************************************************************************************
ok: [mx1] => {
    "msg": "17.3R1.10"
}
ok: [mx2] => {
    "msg": "17.3R1.10"
}

PLAY RECAP **********************************************************************************************************************************************************
mx1                        : ok=3    changed=0    unreachable=0    failed=0
mx2                        : ok=3    changed=0    unreachable=0    failed=0

/project # 
```


# 2. More Advanced Stuff
text

```
cli
```
## 2.1 subtitle
text

```
cli
```
## 2.2 subtitle
text

```
cli
```
## 2.3 subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```
## 2. subtitle
text

```
cli
```

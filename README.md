# Ansible Basics with Junos

## Documentation Structure

[**1. Quick Ansible Demo**](README.md#-1.-Quick-Ansible-Demo)

[**2. More Advanced Stuff**](README.md#-2.-More-Advanced-Stuff)


# 1. Quick Ansible Demo
This is a short demo on the Ansible Galaxy module for Juniper Junos

More on the Ansible Galaxy module for Juniper Junos here:
 - https://junos-ansible-modules.readthedocs.io
 - https://www.juniper.net/documentation/en_US/junos-ansible/information-products/pathway-pages/junos-ansible.html


---
## 1.1 Start a Docker container for the Ansible environment
Using a Docker container greatly simplifies the environment setup for Python, PyEz and Ansible... It also keeps things clean and contained

```
docker pull juniper/pyez-ansible

docker run -it --rm -v $(pwd):/playbooks juniper/pyez-ansible
```

Check the Ansible version
```
ansible --version
```
Check the version of the "Juniper.junos" Ansible Galaxy module
```
ansible-galaxy --version
ansible-galaxy list
cat /root/.ansible/roles/Juniper.junos/version.py
```


See Docker Hub for more info:
- https://hub.docker.com/r/juniper/pyez-ansible/

If you need access to some Junos devices for lab / testing purposes, you can run vSRX or vQFX vagrant images:

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
```yaml
---
# Default username and password
device_user: lab
device_pass: lab123
```

## 1.3 Ansible Playbook for "juniper_junos_facts"

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
docker run -it --rm -v $(pwd):/playbooks juniper/pyez-ansible ash

/playbooks # ansible-playbook -i hosts pb-facts.yml

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

/playbooks # 
```



## 1.4 Ansible Playbook for "juniper_junos_command"

Ansible Playbook file called pb-cli.yml
```yaml
---
  - name: CLI Commands
    hosts: mx
    roles:
     - Juniper.junos
    connection: local
    gather_facts: no

    tasks:

    - name: Pass CLI command "show chassis hardware"
      juniper_junos_command:
        host: "{{ inventory_hostname }}"
        user: "{{ device_user }}"
        passwd: "{{ device_pass }}"
        commands:
          - show chassis hardware
      register: cli_output
    
    - name: Print CLI output
      debug:
        msg: "{{ cli_output }}"

    - name: Pass CLI command "show route summary"
      juniper_junos_command:
        host: "{{ inventory_hostname }}"
        user: "{{ device_user }}"
        passwd: "{{ device_pass }}"
        commands:
          - show route summary
      register: cli_output
    
    - name: Print CLI output
      debug:
        msg: "{{ cli_output }}"
```

## 1.5 Ansible Playbook for "juniper_junos_ping

Ansible Playbook file called pb-ping.yml
```yaml
---
  - name: Checking internet connectivity
    hosts: mx
    roles:
     - Juniper.junos
    connection: local
    gather_facts: no

    tasks:

    - name: Ping Google at 8.8.8.8
      juniper_junos_ping:
        host: "{{ inventory_hostname }}"
        user: "{{ device_user }}"
        passwd: "{{ device_pass }}"
        dest: "8.8.8.8"
      register: response
    
    - name: Print all keys in the response.
      debug:
        var: response
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

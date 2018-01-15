# Keepalived Ansible Role
Ansible playbooks to setup the Keepalived cluster via APT or YUM.

## Quick Start
To create inventory file, and add the follow content:
```sh
[keepalived-masters]
172.16.35.10 ansible_user=vagrant ansible_password=vagrant

[keepalived-nodes]
172.16.35.[11:12] ansible_user=vagrant ansible_password=vagrant

[keepalived-cluster:children]
keepalived-masters
keepalived-nodes
```

Modify `group_vars/all.yml` to set for your need:
```sh
bind_vip_address: "172.16.35.9"
bind_interface: "eth1" # {{ ansible_default_ipv4.interface }}

# keepalived options
keepalived_check_ip: any
keepalived_check_port: 22
keepalived_check_vid: 53
keepalived_check_vmask: 24
```

To setup keepalived cluster:
```sh
$ ansible-playbook site.yml
```

# Ansible Role for Nebula

Quickly and easily deploy the [Nebula Overlay VPN](https://github.com/slackhq/nebula) software onto all of your hosts.

# What Is Nebula

> Nebula is a scalable overlay networking tool with a focus on performance, simplicity and security. It lets you seamlessly connect computers anywhere in the world.

You can read more about Nebula [on the official repo](https://github.com/slackhq/nebula)

# Example Playbook
```
---
- name: Deploy Nebula
  hosts: all
  gather_facts: yes
  user: ansible
  become: yes
  vars:
    nebula_version: 1.8.0
    nebula_network_name: "Company Nebula Mgmt Net"
    nebula_network_cidr: 16
    nebula_lighthouses:
    - inventory: 'lh1'
      hostname: 'lh1'
      overlay_addr: 10.43.0.1
      public_addr_or_name: lighthouse.company.com
      public_port: 4242
      is_relay: false
      extra_config: {}

    nebula_firewall_drop_action: reject

    nebula_inbound_rules:
      - { port: "any", proto: "icmp", host: "any" }
      - { port: 22, proto: "tcp", host: "any" }
    nebula_outbound_rules:
      - { port: "any", proto: "any", host: "any" }

  roles:
    - role: nebula
```

# Example Inventory
```
[lighthouses]
lighthouse01.company.com

[servers]
web01.company.com nebula_internal_ip_addr=10.43.0.2
docker01.company.com nebula_internal_ip_addr=10.43.0.3
zabbix01.company.com nebula_internal_ip_addr=10.43.0.4
backup01.company.com nebula_internal_ip_addr=10.43.0.5
pbx01.company.com nebula_internal_ip_addr=10.43.0.6
```

**Note:** More variables can be found in the [role defaults.](defaults/main.yml)

# Running the Playbook
```
ansible-playbook -i inventory nebula.yml
```

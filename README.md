# Amtega network_manager role

This is an [Ansible](http://www.ansible.com) role configure ipv4 network interfaces
throught NetworkManager service.

## Role Variables

A list of all the default variables for this role is available in `defaults/main.yml`.

The role setups the following facts:

- network_manager_ip_mac_map: dictionary mapping ip addresses to mac addresses

## Example Playbook

This is an example playbook:

``` yaml
---
- name: Role network_manager sample
  hosts: localhost
  roles:
    - amtega.network_manager
  vars:
    network_manager_hostname: "{{ inventory_hostname }}"
    network_manager_gateway: 192.168.5.1
    network_manager_ipv6: no
    network_manager_dns_domain: acme.com
    network_manager_dns_search:
      - acme.com
      - acme.org
    network_manager_dns_nameserver:
      - 192.168.5.200
      - 192.168.5.201
    network_manager_dns_options:
      - timeout:1
      - rotate

    network_manager_interfaces:
      - logicalname: management-01
        macaddress: 08:00:27:06:c1:f8
        ipv4:
          - address: 192.168.5.15
            cidr: 24
        mtu: 1500
        route:
          - net: 192.168.6.0/24
            gateway: 192.168.5.34
        route_multicast: no
        vlanid: 1024
        bond: no
```

## Testing

Tests are based on [molecule with vagrant virtual machines](https://molecule.readthedocs.io/en/latest/installation.html).

```shell
cd amtega.network_manager

molecule lint
molecule converge
```
Idempotence fails when the interface embeds routes or mtu in its properties.
This is due to a bug in the community.general.nmcli module.
Tested with community.general collection version 5.8.0.

## License

Copyright (C) 2022 AMTEGA - Xunta de Galicia

This role is free software: you can redistribute it and/or modify it under the terms of:

GNU General Public License version 3, or (at your option) any later version; or the European Union Public License, either Version 1.2 or – as soon they will be approved by the European Commission ­subsequent versions of the EUPL.

This role is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more details or European Union Public License for more details.

## Author Information

- José Enrique Mourón Regueira
- Juan Antonio Valiño García.

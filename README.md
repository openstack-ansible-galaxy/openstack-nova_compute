Nova compute
=========

OpenStack Nova compute service installation

_Tested on Ubuntu Precise (12.04) and Trusty (14.04)_

Requirements
------------

A MySQL server. See below.

A RabbitMQ server. See below.

Role Variables
--------------
### Nova compute (set by this role)

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `my_ip` | `{{ ansible_eth0.ipv4.address }}` | Management IP for nova-compute |
| `vncserver_proxyclient_address` | `{{ my_ip }}` | The address to use to connect to the vnc proxy ||
| `vncserver_proxy_address` | `{{ my_ip }}` | The address to which proxy clients should connect ||
| `novncproxy_base_url` | `"http://{{ vncserver_proxy_address }}:6080/vnc_auto.html"` | Desired novncproxy base_url ||

### RabbitMQ (must exist)

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `rabbit_hostname` | `localhost` | Hostname/IP address where the RabbitMQ service runs ||
| `rabbit_username` | `rabbit_username_default` | RabbitMQ username for nova ||
| `rabbit_pass` | `rabbit_pass_default` | RabbitMQ password for nova ||

### Neutron (must exist)

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `neutron_hostname` | `localhost` | Hostname/IP address where the neutron server runs ||
| `neutron_pass` | `neutron_pass_default` | Neutron admin password ||
| `neutron_port` | `9696` | Neutron port ||
| `neutron_protocol` | `http` | Neutron protocol (http/https) ||

### Keystone (must exist)

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `keystone_admin_port` | `35357` | Keystone admin port ||
| `keystone_hostname` | `localhost` | Hostname/IP address where keystone runs ||
| `keystone_port` | `5000` | Keystone port ||
| `keystone_protocol` | `http` | Keystone protocol (http/https) ||

### Glance (must exist)

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `glance_hostname` | `localhost` | Hostname/IP address where glance runs ||
| `glance_port` | `9292` | Glance port ||
| `glance_protocol` | `http` | Glance protocol (http/https) ||


Dependencies
------------

None.

Example Playbook
----------------

    - hosts: compute001
      roles:
        - role: openstack-nova_compute
          rabbit_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          rabbit_username: "openstack-neutron"
          rabbit_pass: "{{ RABBIT_NEUTRON_PASS }}"
          keystone_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          neutron_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          glance_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          vncserver_proxy_address: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          neutron_pass: "{{ NEUTRON_PASS }}"
          nova_pass: "{{ NOVA_PASS }}"

---

A complete Ansible playbook demo, which uses this role, is available on Github (dguerri/vagrant-ansible-openstack) <https://github.com/dguerri/vagrant-ansible-openstack>

---


License
-------

Apache

Author Information
------------------

Davide Guerri - davide.guerri@gmail.com

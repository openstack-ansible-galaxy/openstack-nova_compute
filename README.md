Nova Compute OpenStack Ansible Role
=========

**Status**
* [![Build Status](https://travis-ci.org/openstack-ansible-galaxy/openstack-nova_compute.svg?branch=master)](https://travis-ci.org/openstack-ansible-galaxy/openstack-nova_compute) on master branch
* [![Build Status](https://travis-ci.org/openstack-ansible-galaxy/openstack-nova_compute.svg?branch=development)](https://travis-ci.org/openstack-ansible-galaxy/openstack-nova_compute) on development branch
* [![Ansible Galaxy](http://img.shields.io/badge/dguerri-openstack--nova_compute-blue.svg)](https://galaxy.ansible.com/list#/roles/1911) on Ansible Galaxy

OpenStack Nova Compute service installation

_Tested on Ubuntu Precise (12.04) and Trusty (14.04)_

Requirements
------------

A RabbitMQ server. See below.

For RHEL/CentOS, RHOSP or RDO repositories are needed.

Role Variables
--------------

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `nova_compute_database_url` | `sqlite:////var/lib/nova/nova.sqlite` | Database URI ||
| `nova_compute_my_ip` | `{{ ansible_eth0.ipv4.address }}` | Management IP for nova-compute |
| `nova_compute_vncserver_proxyclient_address` | `{{ my_ip }}` | The address to use to connect to the vnc proxy ||
| `nova_compute_vncserver_proxy_address` | `{{ my_ip }}` | The address to which proxy clients should connect ||
| `nova_compute_novncproxy_base_url` | `"http://{{ vncserver_proxy_address }}:6080/vnc_auto.html"` | Desired novncproxy base_url ||
| `nova_compute_virt_type` | `kvm` | Desired virtualization type ||
| `neutron_hostname` | `localhost` | Hostname/IP address where the neutron server runs ||
| `neutron_pass` | `neutron_pass_default` | Neutron admin password ||
| `neutron_port` | `9696` | Neutron port ||
| `neutron_protocol` | `http` | Neutron protocol (http/https) ||
| `keystone_admin_port` | `35357` | Keystone admin port ||
| `keystone_hostname` | `localhost` | Hostname/IP address where keystone runs ||
| `keystone_port` | `5000` | Keystone port ||
| `keystone_protocol` | `http` | Keystone protocol (http/https) ||
| `glance_hostname` | `localhost` | Hostname/IP address where glance runs ||
| `glance_port` | `9292` | Glance port ||
| `glance_protocol` | `http` | Glance protocol (http/https) ||

### RabbitMQ (must exist)

| Name | Default value | Description | Note |
|---  |---  |---  |--- |
| `nova_compute_rabbit_userid` | `rabbit_username_default` | RabbitMQ username for console auth ||
| `nova_compute_rabbit_password` | `rabbit_pass_default` | RabbitMQ password for console auth ||
| `nova_compute_rabbit_virtual_host`| `/` | RabbitMQ virtual host for console auth ||
| `nova_compute_rabbit_retry_interval` | `1` | Frequency to retry connecting to RabbitMQ ||
| `nova_compute_rabbit_host` | `localhost` | The RabbitMQ broker address where a single node is used ||
| `nova_compute_rabbit_port` | `5672` | The RabbitMQ broker port where a single node is used ||
| `nova_compute_rabbit_hosts` | `$rabbit_host:$rabbit_port` | RabbitMQ HA cluster host:port pairs ||
| `nova_compute_rabbit_ha_queues` | `False` | Use HA queues in RabbitMQ (x-ha-policy: all) ||

Dependencies
------------

None.

Example Playbook
----------------

    - hosts: compute001
      roles:
        - role: openstack-nova_compute
          nova_compute_rabbit_host: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          nova_compute_rabbit_userid: "openstack-neutron"
          nova_compute_rabbit_password: "{{ RABBIT_NEUTRON_PASS }}"
          keystone_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          neutron_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          glance_hostname: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          nova_compute_vncserver_proxy_address: "{{ hostvars['controller'].ansible_all_ipv4_addresses[0] }}"
          neutron_pass: "{{ NEUTRON_PASS }}"
          nova_pass: "{{ NOVA_PASS }}"

---

A complete Ansible playbook demo, which uses this role, is available on Github (openstack-ansible-galaxy/vagrant-ansible-openstack) <https://github.com/openstack-ansible-galaxy/vagrant-ansible-openstack>

---

Credits
-------
RedHat support implemented by Abel Boldú <abel.boldu@gmx.com>


License
-------

Apache

Author Information
------------------

Davide Guerri - davide.guerri@gmail.com

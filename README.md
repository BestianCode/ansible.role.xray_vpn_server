# Role Description

This Ansible role automates the setup of a Xray VPN Server on a remote host. The role installs the Xray VPN Server, configures the server, and sets up the VPN users.

## VPN Clients

* Clients tested by me or my friends:
  - iPhone:
    - Fair - Works fine, IPv6 supported
    - FoXray - Works fine, IPv6 supported
    - v2RayTun - Works fine, IPv6 supported
    - V2Box - Works fine, IPv6 supported, but annoing ads
    - AmneziaVPN - Works unstable, IPv4 only, no IPv6, sometimes no traffic - Not recommended.
  - Android:
    - v2rayNG - Works fine, IPv6 supported
    - v2RayTun - Works fine, IPv6 supported
    - Hiddify - Works fine, IPv6 supported
    - HiddifyNG - Works with problems, for me IPv4 was not working... - Not recommended.
    - AmneziaVPN - Works unstable, IPv4 only, no IPv6, sometimes no traffic - Not recommended.
    - V2Box - Unusable shit on Android due to ads - Not recommended.

## Default Variables

For more information on default variables, please refer to the `defaults/main.yml` file.

## Installation

* __This role should be used together with ansible-galaxy collection bestiancode.nginx_ssl to manage SSL certificates__
* Galaxy: https://galaxy.ansible.com/ui/repo/published/bestiancode/nginx_ssl/
* GitHub: https://github.com/BestianCode/ansible.collection.nginx_ssl

You can install this collection and role using the `ansible-galaxy` command:

```bash
ansible-galaxy collection install bestiancode.nginx_ssl
ansible-galaxy role install bestiancode.xray_vpn_server
```

Alternatively, you can create a `requirements.yml` file and use it to install all collections and roles at once:

```bash
ansible-galaxy collection install -r requirements.yml --force
ansible-galaxy role install -r requirements.yml --force
```

Here's a sample `requirements.yml` file:

```yaml
collections:
  - name: bestiancode.sysadmin
  - name: bestiancode.nginx_ssl

roles:
  - name: bestiancode.xray_vpn_server
```

## Usage

Here's a sample `playbook`:

```yaml
- hosts:
    - all
  become: true
  tags:
    - basic
    - init
  roles:
    - bestiancode.sysadmin.initial_setup

- hosts:
    - web
  become: true
  tags:
    - nginx
  roles:
    # If collections are not defined here, it is mandatory to specify prefix bestiancode.nginx_ssl!
    - bestiancode.nginx_ssl.install
    - bestiancode.nginx_ssl.letsencrypt
    - bestiancode.nginx_ssl.reload
    # Add other roles here

- hosts:
    - web
  become: true
  tags:
    - nginx
  collections:
    # If collections are defined here...
    - bestiancode.nginx_ssl
  roles:
    # If you defined collections, prefix bestiancode.nginx_ssl is not needed.
    - install
    - letsencrypt
    - reload

- hosts:
    - signal
  become: true
  tags:
    - signal
  roles:
    - bestiancode.xray_vpn_server

#...
# - other constructions can be here
#...
```

## Parameters

```yaml
#
# Xray VPN Server connection strings will be provided as the latest step of the ansible role
#
# If you use port 443, you have to disable catching of default port 443 by Nginx
# catch_default_443: false
# if not 443, you can use default catching
# catch_default_443: true
#
xray_dns_name: "{{ ansible_host }}"
xray_ssl_name: "{{ xray_dns_name }}"

xray_port: 8443
xray_path: /maps

# Don't use default UIDs, for a security reasons! Generate your own!
#
# Use the following command to generate a new UUID:
#
# uuidgen
#
xray_users:
  - { uid: "E6AA699D-8642-4767-8C98-DB93F958A48B", name: "John" }
  - { uid: "39ADD62B-B607-4C56-866F-364F4056EF83", name: "Vika" }
  - { uid: "A1B2C3D4-E5F6-7890-1234-56789ABCDEF0", name: "Michael" }
  - { uid: "B2C3D4E5-F678-9012-3456-789ABCDE1234", name: "Sophia" }
  - { uid: "C3D4E5F6-7890-1234-5678-9ABCDEF12345", name: "David" }
  - { uid: "D4E5F678-9012-3456-789A-BCDEF1234567", name: "Emma" }
  - { uid: "E5F67890-1234-5678-9ABC-DEF123456789", name: "James" }
  - { uid: "F6789012-3456-789A-BCDE-F123456789AB", name: "Olivia" }
  - { uid: "67890123-4567-89AB-CDEF-123456789ABC", name: "Daniel" }
  - { uid: "78901234-5678-9ABC-DEF1-23456789ABCD", name: "Ava" }
```

## URLs

- **GitHub**: https://github.com/BestianCode/ansible.role.xray_vpn_server
- **Ansible Galaxy**: https://galaxy.ansible.com/bestiancode/xray_vpn_server

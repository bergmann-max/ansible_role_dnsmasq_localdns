# Ansible Role: dnsmasq_localdns

![Ansible](https://img.shields.io/badge/ansible-ready-blue.svg)
![Platform](https://img.shields.io/badge/platform-Debian%2FUbuntu-lightgrey)
![License](https://img.shields.io/badge/license-MIT-green)

## Description

This Ansible role installs and configures **dnsmasq** as a **local DNS resolver** and **DNS forwarder**.  
It sets up **custom DNS records**, defines **local SRV services**, and integrates with **systemd-resolved** for smooth local name resolution.

### Features:
- Supports **Debian-family systems** (Debian, Ubuntu)
- Configures **dnsmasq** to listen on specific interfaces and IP addresses
- Provides **custom DNS records** (hosts file)
- Defines **local domain** and SRV service records
- Integrates cleanly with **systemd-resolved**
- Includes **DNS caching** for performance
- **Sanity Checks** and proper handlers for smooth deployment
- Secure and minimal — strictly focused on **local DNS**

---

## Supported Platforms

Any APT-based Debian-family system (Ubuntu, Debian).

---

## Role Variables

| Variable                                   | Default Value                      | Description                                                               |
|--------------------------------------------|------------------------------------|---------------------------------------------------------------------------|
| `dnsmasq_hosts_file`                       | `/etc/hosts.dnsmasq`               | Path to the generated dnsmasq hosts file                                  |
| `dnsmasq_interface`                        | `eth0`                             | Network interface where dnsmasq listens                                   |
| `dnsmasq_dns_server`                       | `10.0.0.1`                         | Upstream DNS server used for forwarding DNS queries                       |
| `dnsmasq_listen_addresses`                 | `[127.0.0.1, 10.0.0.100]`          | IP addresses where dnsmasq will bind and listen                           |
| `dnsmasq_cache_size`                       | `1000`                             | Size of the DNS cache (number of cached entries)                          |
| `dnsmasq_local_domain`                     | `myhome.local`                     | Local domain used for name resolution                                     |
| `dnsmasq_srv_records`                      | See defaults/main.yml              | List of SRV records (service discovery for local services)                |
| `dnsmasq_localdns_hosts`                   | See defaults/dnsmasq_hosts.yml     | List of static host entries (IP + hostname(s)) for local resolution       |

> **Note:**  
> Variables are kept role-specific and can be customized as needed. Sensitive data is **not stored**; role is strictly local.

---

## Usage Example

```yaml
- name: Configure local DNS resolver via dnsmasq
  hosts: all
  become: true
  roles:
    - role: ansible_role_dnsmasq_localdns
      vars:
        dnsmasq_interface: "ens3"
        dnsmasq_dns_server: "1.1.1.1"
        dnsmasq_local_domain: "home.lan"
        dnsmasq_listen_addresses:
          - "127.0.0.1"
          - "192.168.1.1"
        dnsmasq_localdns_hosts:
          - "192.168.1.10 nas.home.lan"
          - "192.168.1.20 media.home.lan"
```

---

## Tags

| Tag         | Purpose                                  |
|------------|------------------------------------------|
| dnsmasq     | Install and configure dnsmasq            |
| localdns    | Manage local DNS entries                 |
| resolver    | Configure system resolver integration    |
| caching     | Enable DNS caching for faster lookups    |

---

## Sanity Checks

The role ensures:

- `dnsmasq` is installed and running
- Configured to listen only on intended interfaces/IPs
- `/etc/dnsmasq.d/dnsmasq.conf` and `/etc/hosts.dnsmasq` files are correctly templated
- `systemd-resolved` is adjusted (`DNSStubListener=no`)
- SRV records and hosts entries are properly applied
- DNS caching is enabled as expected

---

## License

MIT

---

## Author Information

Role maintained by **Max Bergmann**.  
Feel free to contribute, suggest improvements, or raise issues!

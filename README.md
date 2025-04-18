# Ansible Role: ansible_role_dnsmasq_localdns

![Ansible](https://img.shields.io/badge/ansible-ready-blue.svg)
![Platform](https://img.shields.io/badge/platform-Ubuntu-lightgrey)
![License](https://img.shields.io/badge/license-Unlicense-green)

## Description

This Ansible role installs and configures **dnsmasq** as a local DNS resolver on Ubuntu systems.  
It disables `systemd-resolved`, configures `resolv.conf` for local resolution, and sets up static host entries and SRV records to enable robust name resolution in local environments or homelabs.

### Features:
- Installs and configures `dnsmasq` with a custom template
- Disables `systemd-resolved` and manages `/etc/resolv.conf`
- Binds to specified interfaces and IP addresses
- Adds local DNS entries and SRV records
- Enables DNS caching with adjustable cache size
- Delivers systemd override for full dnsmasq control

## Role Variables

This role provides the following configurable variables (see `defaults/main.yml`):

| Variable | Default | Description |
|----------|---------|-------------|
| `dnsmasq_hosts_file` | `/etc/hosts.dnsmasq` | Path to the additional hosts file used by dnsmasq |
| `dnsmasq_interface` | `eth0` | Network interface dnsmasq listens on |
| `dnsmasq_dns_server` | `10.0.0.1` | Upstream DNS server for forwarding queries |
| `dnsmasq_listen_addresses` | `["127.0.0.1", "10.0.0.100"]` | List of IPs dnsmasq binds to |
| `dnsmasq_cache_size` | `1000` | Size of the DNS cache |
| `dnsmasq_local_domain` | `example.com` | Local DNS domain |
| `dnsmasq_srv_records` | see `defaults/main.yml` | List of SRV records to configure |
| `dnsmasq_localdns_hosts` | see `defaults/main.yml` | Local IP-to-hostname mappings |

## Usage Example

```yaml
- name: Configure local DNS with dnsmasq
  hosts: all
  become: true
  roles:
    - role: ansible_role_dnsmasq_localdns
```

## Sanity Checks

This role ensures:
- `systemd-resolved` is disabled and does not interfere with dnsmasq
- A static `/etc/resolv.conf` points to `127.0.0.1` and a fallback DNS
- `dnsmasq` is installed and configured with a systemd override
- Static DNS and SRV records are deployed
- The dnsmasq service is enabled and started reliably

## Directory Structure

Follows standard Ansible role layout:

```
ansible_role_dnsmasq_localdns/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── meta/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   ├── dnsmasq.conf.j2
│   └── hosts.dnsmasq.j2
└── README.md
```

✔️ Designed for reproducibility and robust DNS behavior in local and home lab setups.

## License

Unlicense

## Author Information

Role maintained by Max Bergmann.  
Feedback, suggestions, and contributions are always welcome! 🚀

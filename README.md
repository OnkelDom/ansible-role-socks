# Ansible Role: Sockd

[![ubuntu-18](https://img.shields.io/badge/ubuntu-18.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![ubuntu-20](https://img.shields.io/badge/ubuntu-20.x-orange?style=flat&logo=ubuntu)](https://ubuntu.com/)
[![debian-9](https://img.shields.io/badge/debian-9.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![debian-10](https://img.shields.io/badge/debian-10.x-orange?style=flat&logo=debian)](https://www.debian.org/)
[![centos-7](https://img.shields.io/badge/centos-7.x-orange?style=flat&logo=centos)](https://www.centos.org/)
[![centos-8](https://img.shields.io/badge/centos-8.x-orange?style=flat&logo=centos)](https://www.centos.org/)

[![License](https://img.shields.io/badge/license-MIT%20License-brightgreen.svg?style=flat)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/OnkelDom/ansible-role-sockd?style=flat)](https://github.com/OnkelDom/ansible-role-sockd/issues)
[![GitHub tag](https://img.shields.io/github/tag/OnkelDom/ansible-role-sockd.svg?style=flat)](https://github.com/OnkelDom/ansible-role-sockd/tags)
[![GitHub action](https://github.com/OnkelDom/ansible-role-sockd/workflows/ansible-lint/badge.svg)](https://github.com/OnkelDom/ansible-role-sockd)

## Description

Install and configure an Dante Socks Proxy on CentOS/RHEL or Debian systems using ansible.

Only in destination ip is the subnetmask required. Not in source.

## Requirements

- Ansible >= 3
- Community Packages
- `ansible-galaxy collection install community.general`
- `ansible-galaxy collection install ansible.posix`
- `ansible-galaxy collection install onkeldom.caddyserver`

After you have installed dante socks, you van use to following tag to only change the configuration and reload the service
```
ansible-playbook <playbook>.yml --tags sockd_acls
```

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `proxy_env` | {} | Set proxy environment variables | 
| `sockd_user` | sockd | Run user |
| `sockd_group` | sockd | Run group |
| `sockd_bind_ip` | "0.0.0.0" | Default bind IP |
| `sockd_bind_port` | 1080 | Default bind Port |
| `sockd_bind_external_interface` | "{{ ansible_default_ipv4.interface }}" | Default bind Interface |
| `sockd_bind_ipv6_enabled` | true | Enable/Disable ipv6 |
| `sockd_bind_ipv6` | "::" | Default ipv6 bind IP |
| `sokcd_log_folder` | "/var/log/sockd"` | Default log dir |
| `sokcd_pid_folder` | "/var/run/sockd"` | Default pid dir |
| `sockd_logrotate_days` | 28 | Default logrotate days |
| `sockd_workers` | 8 | Default num workers |
| `sockd_debug` | "" | Debug mode |
| `sockd_html_vip` | "{{ ansible_hostname }}.{{ ansible_domain }}" | Default virtual (ip) service name for html config |
| `socks_html_mail` | "my@mail.com" | Default email-address for html config |
| `sockd_html_output` | false | Generate HTML Output [ansible-role-caddyserver](https://github.com/OnkelDom/ansible-role-caddyserver) required |
| `sockd_html_path` | /var/www/ | Default Caddy webserver path |
| `sockd_access_rules` | {} | Default access rules (see below) |

## Example

```yaml
---
- hosts: all
  roles:
  - onkeldom.sockd
  vars:
    sockd_access_rules:
      - name: "Allow any dst"
        src:
          - 10.100.0.2 # boss01.loca.lan
      - name: "Empty Logging for this"
        log: "" # with this, logging is disabled for this hosts
        src:
          - 10.100.0.101 # test01.loca.lan
          - 10.100.0.102 # test02.loca.lan
        dst:
          - 173.249.21.102/32
      - name: "Other with multiple destinations"
        src:
          - 10.100.1.212 # test04.loca.lan
          - 10.100.1.30  # test06.loca.lane
          - 10.100.1.16  # test08.loca.lan
        dst:
          - 173.249.21.102/32
          - .github.com
          - .githubusercontent.com
          - .stackoverflow.com
```

## Contributing

See [contributor guideline](CONTRIBUTING.md).

## License

This project is licensed under MIT License. See [LICENSE](/LICENSE) for more details.

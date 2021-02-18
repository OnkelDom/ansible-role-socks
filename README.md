# Ansible Role: sockd (centos/rhel) only

## Description

Install and configure an Dante Socks Proxy on CentOS/RHEL systems using ansible.

* The binary file is from [https://github.com/koss822/misc/tree/master/Linux/Projects/dante_socks](Koss822)
* Inspired by [https://www.enigma14.eu/martin/blog/2018/02/01/dante-socks-proxy-how-to-install-and-manage-on-rhel-or-other-distros/](Martin's Blog)

## Requirements

- Ansible >= 2.5 (It might work on previous versions, but we cannot guarantee it)

## Role Variables

All variables which can be overridden are stored in [defaults/main.yml](defaults/main.yml) file as well as in table below.

| Name           | Default Value | Description                        |
| -------------- | ------------- | -----------------------------------|
| `sockd_user` | sockd | Run user |
| `sockd_group` | sockd | Run group |
| `sockd_bind_ip` | "{{ ansible_default_ipv4.address }}" | Default bind IP |
| `sockd_bind_port` | 1080 | Default bind Port |
| `sockd_bind_interface` | "{{ ansible_default_ipv4 }}" | Default bind Interface |
| `sockd_debug` | "" | Debug mode |
| `sockd_bind_ipv6_enabled` | false | Enable/Disable ipv6 |
| `sockd_bind_ipv6` | "{{ ansible_default_ipv6.address | default('') }}" | Default ipv6 bind IP |
| `sockd_bind_port_ipv6` | 1080 | Default ipv6 bind Port |
| `sockd_binary_install_dir` | "/usr/local/bin" | Default binary installation dir |
| `sockd_config` | "/etc/sockd.conf" | Default config pat |
| `sockd_vip` | "{{ ansible_hostname }}.{{ ansible_domain }}" | Default virtual (ip) service name for html config |
| `socks_contact_mail` | "my@mail.com" | Default email-address for html config |
| `sockd_access_rules` | {} | Default access rules (see below) |
| `sockd_caddy_generate_html_output` | false | Generate HTML Output [https://github.com/OnkelDom/ansible-role-caddyserver](ansible-role-caddyserver) required |
| `sockd_caddy_webserver_path` | /var/www/ | Default Caddy webserver path |
| `sockd_caddy_user` | caddy | Default Caddy user |
| `sockd_caddy_group` | caddy | Default Caddy group |
| `sockd_consul_service_register` | false | Generate Consul service snipped |
| `consul_service_config` | [defaults/main.yml](defaults/main.yml) | Generate Consul service [https://github.com/OnkelDom/ansible-role-consul](ansible-role-consul) required  |


## Example

```yaml
---
- hosts: all
  roles:
  - ansible-role-socks
  vars:
    sockd_access_rules:
      - name: "Test1"
        log: "" # with this, logging is disabled
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
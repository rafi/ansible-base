### Ansible base system roles

Ansible roles for a common sensible base configuration for new machines. Should support Debian/Ubuntu and Archlinux.

Configures sshd, iptables, logrotate, /etc/skel and users.

### Install
Add this repo as a submodule in your `roles/` folder and include it in
a playbook of yours.
Make sure you have a folder named `public_keys` along-side your playbooks
in which you can place public keys for users to authorize, and any
generated ssh key will be fetched under a folder named as the host.

### Config example
The machine's `hostname` will be set as your playbook's host name.

```yaml
---
ansible_python_interpreter: /usr/bin/python2

# Base role
timezone: UTC
ntp_enable_daemon: true
locales:
 - name: en_US.UTF-8
   charset: UTF-8
   default: true

# Archlinux only
systemd_journal_max_size: 100M
systemd_vconsole_keymap: us
systemd_vconsole_font: Lat2-Terminus16
systemd_logind_ignore_lid_switch: false

# Common role
ssh_port: 22
ssh_allow_users:
  - john
  - bob
  - dylan

iptables_accept:
  - port: 80
    proto: tcp
    source: null
  - port: 22
    proto: tcp
    source: limit

user_groups:
  - name: owners
    sudo_nopasswd: yes
  - name: developers

users:
  - name: bob
    groups:
      - owners
      - developers
    authorized_key: public_keys/bob
  - name: deploy
    generate_ssh_key: yes
    groups:
      - developers
  - name: ronen
    groups:
      - developers
    authorized_key: public_keys/dylan
```

### Credits
Big thanks to [uggedal's playbooks]!-- this repository uses some of his
base roles and iptables template.

### License
MIT

[uggedal's playbooks]: https://github.com/uggedal/playbooks

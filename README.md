# Ansible Role: BootstrapLinux

This Ansible role allow users to customize a linux server.

## 1. Requirements

There is no requirements as this role will install and configure everything. You only need an ssh root access from your Ansible server to your linux server.

## 2. Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml` if exist).

You **can** configure the following services:
- snmp
- ntp
- syslog
- smtp
- qemu agent (for kvm)
- bash
- users
- ssh

```yml
# SNMP configuration
snmp_config:
  configure: yes # Can be yes or no (bolean)
  rocommunity: public
  syscontact: sebastien@dewarlez.fr
  syslocation: home
  conf_location: /etc/snmp/snmpd.conf
  service_name: snmpd
  backup_config: yes # Can be yes or no (bolean)

# NTP client configuration
ntp_config:
  configure: yes # Can be yes or no (bolean)
  server: 192.168.2.134
  timezone: Europe/Paris
  conf_location:
    debian: /etc/ntpsec/ntp.conf
    ubuntu: /etc/ntpsec/ntp.conf
    rocky: /etc/ntp.conf
  service_name:
    debian: ntpd
    ubuntu: ntpd
    rocky: ntpd
  backup_config: yes # Can be yes or no (bolean)

# Syslog configuration
syslog_config:
  configure: yes # Can be yes or no (bolean)
  server: syslog.dewarlez.fr
  conf_location: /etc/rsyslog.d/custom.conf
  service_name: rsyslog
  backup_config: yes # Can be yes or no (bolean)

# SMTP configuration to allow server sending email
smtp_config:
  configure: yes # Can be yes or no (bolean)
  host: smtp.dewarlez.fr
  port: 25
  test: yes # Can be yes or no (bolean)
  test_from: noreply@dewarlez.fr
  test_to: sebastien@dewarlez.fr
  conf_location: /etc/mail.rc
  backup_config: yes # Can be yes or no (bolean)

# QEMU Agent configuration
qemu_agent_config:
  configure: yes # Can be yes or no (bolean)
  service_name: qemu-guest-agent
  backup_config: yes # Can be yes or no (bolean)

# Bashrc customisation
bash_config:
  configure: yes # Can be yes or no (bolean)
  default_bashrc: /etc/profile.d/default_bashrc.sh
  root_bashrc: /root/.bashrc
  skeleton_bashrc: /etc/skel/.bashrc
  backup_config: yes # Can be yes or no (bolean)
  alert_login:
    enable: no # Can be yes or no (bolean)
    send_to: sebastien@dewarlez.fr
    send_from: noreply@dewarlez.fr

# Create a series of default users
users_config:
  configure: yes # Can be yes or no (bolean)
  users:
    - name: root
      password: "$6$cDP4/kfEjABOFzju$absFqDNB2KVrcxGQTRZkBbFuK5CAIMXUUPqu2/HSkYRSfeUZwyN5XjmUxApYuYCiHHDnjhvOBVzDjmrRXrsk6." # poiuyt123$
      state: present # Can be present or absent
      createhome: no # Can be yes or no (bolean)
      sudo: yes # Can be yes or no (bolean)
    - name: sdewarle
      password: "$6$cDP4/kfEjABOFzju$absFqDNB2KVrcxGQTRZkBbFuK5CAIMXUUPqu2/HSkYRSfeUZwyN5XjmUxApYuYCiHHDnjhvOBVzDjmrRXrsk6." # poiuyt123$
      state: present # Can be present or absent
      createhome: yes # Can be yes or no (bolean)
      sudo: yes # Can be yes or no (bolean)

# Parameters to reconfigure SSH service
security_config:
  configure: yes # Can be yes or no (bolean)
  conf_location: /etc/ssh/sshd_config
  service_name: sshd
  backup_config: yes # Can be yes or no (bolean)
```

Also, you **can** change the default locations if needed as below.

```yml
# File and folder default location
locations:
  working_folder: "/tmp"
  ansible_public_key: "~/.ssh/id_rsa.pub"
```

Or python location.
```yml
# Python is needed for ansible to work correctly, please provide package name and path
python:
  pkg_name: python3
  path: /usr/bin/python3
```

## 3. Results

You will have a customized server.

## 4. Dependencies

This role have no dependencies with other roles.

## 5. How to test and examples

Go to your ansible server, download this role and then browse into `roles/tests` in order to execute the following command:

```
ansible-playbook test.yml -i inventory.yml -u ansible
```

Update the inventory to match your servers (file called `inventory.yml` in tests folder) and then replace `ansible` by a user that have an access to your machine.

## 6. Todo

Nothing right now.

## 7. Author Information

This role has been writed by [Sébastien DEWARLEZ](mailto:sebastien@dewarlez.fr) for [Personal Purpose](https://blog.dewarlez.fr).
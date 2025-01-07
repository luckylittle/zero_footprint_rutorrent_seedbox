ansible-role-zero-footprint-ruT-seedbox
=========

Configures vanilla RHEL system to be lightweight and bulletproof seedbox running rTorrent and ruTorrent. It aims to be secure (SELinux, SSL, Fail2Ban enabled) and creates very few logs (zero footprint).

Requirements
------------

* It is expected, that you have a brand new RHEL system and have Ansible access sorted out - including working `sudo`. You can use my role [luckylittle/ansible-role-create-user](https://github.com/luckylittle/ansible-role-create-user) for passwordless SSH access and sudo.

Role Variables
--------------

`defaults/main.yml`:

* `set_timezone` - change the time zone of the server
* `set_google_dns` - if `true`, it will add Google DNS to the primary interface
* `epel_dl` - URL of the EPEL RPM
* `libtorrent_dl` - URL of the libtorrent
* `rtorrent_dl` - URL of the rtorrent
* `rtorrent_port` - what port should rtorrent listen on
* `rt_memory_max_set` - how much memory should rTorrent use by default. It is using 75% of your total memory by default.
* `ftp_port` - what port should vsftpd listen on
* `pasv_port_range` - what port range should be used for FTP PASV
* `single_user` - when `true` only one FTP user will be used and it is the same username who runs this playbook. When `false`, [this](files/vsftpd/users.txt) file is used
* `rutorrent_dl` - URL of the ruTorrent
* `https_port` - what port should rutorrent listen on
* `htpasswd` - HTTP basic password to log in to ruTorrent interface
* `fail2ban_ignore_ipv4` - what IPv4 address should be excluded from being banned by Fail2Ban
* `require_reboot` - does the machine require reboot after the playbook is finished

_Note:_ Lot of the tasks rely on `remote_user` / `ansible_user` variable (user who logs in to the remote machine via Ansible). For example, it creates directory structure under that user.

Dependencies
------------

None

Example Playbook
----------------

`echo 'password1' > password`

`ansible-playbook -i inventory --vault-password-file=password site.yml`

```ini
[seedbox]
123.124.125.126
```

```yaml
---
- hosts: seedbox
  remote_user: redhat
  roles:
    - ansible-role-zero-footprint-ruT-seedbox
```

Testing
-------

On a brand new RHEL8.6, 1x vCPU, 4GB RAM playbook took 18m 32s to finish.

License
-------

MIT

Author Information
------------------

Lucian Maly <<lmaly@redhat.com>>

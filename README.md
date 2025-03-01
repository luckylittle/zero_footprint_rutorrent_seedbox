ansible-role-zero-footprint-ruT-seedbox
=======================================

Configures vanilla RHEL system to be lightweight and bulletproof seedbox running rTorrent and ruTorrent. It aims to be secure (SELinux, SSL, Fail2Ban enabled) and creates very few logs (zero footprint).

Requirements
------------

* It is expected, that you have a brand new RHEL system and have Ansible access sorted out - including working `sudo`. You can use my other role [luckylittle/ansible-role-create-user](https://github.com/luckylittle/ansible-role-create-user) for passwordless SSH access and sudo.

Role Variables
--------------

`defaults/main.yml`:

* `set_timezone` - change the time zone of the server, defaults to Europe/Prague.
* `set_google_dns` - if `true`, it will add Google DNS servers to the primary interface. Defaults to false.
* `create_new_user` - whether you want to also create another user. Defaults to false.
* `autobrr_version` and `autobrr_port` - contains the latest [autobrr](https://github.com/autobrr/autobrr) version and a standard port 7474.
* `epel_dl` - URL of the [EPEL](https://docs.fedoraproject.org/en-US/epel/) RPM. Default to the RHEL9 EPEL.
* `libtorrent_dl` - URL of the [libtorrent](https://github.com/rakshasa/rtorrent/releases) sources.
* `rtorrent_dl` - URL of the [rtorrent](https://github.com/rakshasa/rtorrent/releases) sources.
* `rtorrent_port` - what port should rtorrent listen on. Default is 55442.
* `rt_memory_max_set` - how much memory should rTorrent use by default. It is using 75% of your total memory by default.
* `ftp_port` - what port should vsftpd listen on. Default is 55443.
* `pasv_port_range` - what port range should be used for FTP PASV, by default this is 64000-64321.
* `single_user` - when `true` only one FTP user will be used and it is the same username who runs this playbook. When `false`, [this](files/vsftpd/users.txt) file is used.
* `rutorrent_dl` - URL of the [ruTorrent](https://github.com/Novik/ruTorrent) sources.
* `https_port` - what port should rutorrent listen on, by default 443.
* `htpasswd` - HTTP basic password to log in to ruTorrent interface. Default is r3dh4t.
* `fail2ban_ignore_ipv4` - what IPv4 address should be excluded from being banned by Fail2Ban. Whitelisted is arbitrary address 123.124.125.126. You need to change it to your own!
* `require_reboot` - does the machine require reboot after the playbook is finished. It is recommended & default to be true.

_Note:_ Lot of the tasks rely on `remote_user` / `ansible_user` variable (user who logs in to the remote machine via Ansible). For example, it creates directory structure under that user.
the ratio defaults should be sufficient (between 400%-500%).

Dependencies
------------

- Ansible core v2.14.17
- community.general:10.4.0 (`ansible-galaxy collection install community.general`)
- community.crypto:2.25.0 (`ansible-galaxy collection install community.crypto`)
- ansible.posix:2.0.0 (`ansible-galaxy collection install ansible.posix`)

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

The following Terraform can be used to create necessary infrastructure (based on RHEL9 on AWS):

`terraform apply -var=key_name=<NAME_OF_THE_EXISTING_KEY_PAIR_IN_AWS>`

```hcl

```

Then you can just run this role against the EC2 machine like: `time ansible-playbook -i inventory -u ec2-user test.yml --ask-vault-pass`

License
-------

MIT

Author Information
------------------

Lucian Maly <<lmaly@redhat.com>>

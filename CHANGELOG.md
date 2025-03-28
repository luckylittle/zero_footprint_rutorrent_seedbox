# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.2.0] - 2025-03-27

### Added

- UseDNS no to sshd
- AB current_version injection into a backup in the upgrade script
- `maximum_number_of_open_file_descriptors` in common tasks
- Discard unused blocks once a week
- Display service_facts in smoke tests
- new default variable
- New limited firewalld zone for SSHD access

### Changed

- systemd definition of rTorrent service inside tmux
- Standardized ruT config definitions to maintain a consistent...
- Bumped ab & sc versions, added maximum_number_of_open_file_d...
- Updated rtorrent.rc with maximum_number_of_open_file_descrip...
- Minor improvement with ruT plugins
- journald rotation improvement
- `fail2ban_ignore_ipv4` to include private ranges

### Removed

- logrotate completely

### Deprecated

- N/A

### Fixed

- Incorrect role name in a few places
- yamllint & ansible-lint passing

### Security

- `fail2ban_ignore_ipv4` IS NOW ALSO USED BY FIREWALLD!!!!!!!!!!


## [2.1.0] - 2025-03-05

### Added

- This [CHANGELOG.md](CHANGELOG.md) :-)
- Autobrr to use reverse proxy lighttpd
- Sizechecker (https://github.com/s0up4200/sizechecker)
- Simple smoke tests
- panic.sh script
- Missing log file /var/log/rhsm/rhsmcertd.log to 06-cleanup.yml

### Changed

- Merged files/fail2ban/lighttpd-auth.conf from `fail2ban:master` (https://github.com/fail2ban/fail2ban/pull/3955)
- Merged files/fail2ban/vsftpd.conf from `fail2ban:master` (https://github.com/fail2ban/fail2ban/pull/3954)

### Removed

- N/A

### Deprecated

- N/A

### Fixed

- N/A

### Security

- N/A

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [2.3.1] - 2025-05-22

### Added

- @luckylittle Added: Preflight tasks
- @luckylittle Added: Preflight to main tasks
- @luckylittle Added: Missing handlers

### Changed

- @luckylittle Changed: Moved most static variables to role vars
- @luckylittle Changed: ratio limits are tunable
- @luckylittle Changed: Moved some defaults to role vars
- @luckylittle Changed: 01-common variables to role vars
- @luckylittle Changed: 02-rtorrent variables to role vars
- @luckylittle Changed: 03-vsftpd variables to role vars
- @luckylittle Changed: 04-rutorrent variables to role vars
- @luckylittle Changed: 05-security variables to role vars
- @luckylittle Changed: 06-cleanup variables to role vars
- @luckylittle Changed: 07-reboot variables to role vars
- @luckylittle Changed: 08-smoke_tests variables to role vars
- @luckylittle Changed: README.md

## [2.3.0] - 2025-05-19

### Added

- @luckylittle Added: new sysctl_tunables, updated some defaults
- @luckylittle Added: sysctl_tunables task, updated nofile to all users
- @luckylittle Added: autobrr healthz endpoint check
- @luckylittle Added: Massive list of sysctl tunables for a high-performance file servers
- @luckylittle Added: Support for CentOS Stream release 9

### Changed

- @luckylittle Update: README, added image

### Fixed

- @luckylittle Bugfix: Missing stream-response-body
- @luckylittle Bugfix: Tar is not always present
- @luckylittle Bugfix: policycoreutils-python-utils is not everywhere
- @luckylittle Bugfix: yamllint to PASS, CentOS is the same as EL
- @luckylittle Bugfix: Accidentally removed comment
- @luckylittle Bugfix: Changed the order of sysctl thresh keys
- @luckylittle Bugfix: Ansible facts not found
- @luckylittle Bugfix: Applying sysctl

## [2.2.1] - 2025-04-16

### Changed

- Renamed variables to `_ver`
- Bumped versions
- Updated new `_ver` names in README

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

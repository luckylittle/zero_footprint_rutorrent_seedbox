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

- community.general:10.4.0
- community.crypto:2.25.0
- ansible.posix:2.0.0

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

Ansible core v2.14.17 on a brand new RHEL8.6, 1x vCPU, 4GB RAM playbook took 18m 32s to finish.


The following Terraform can be used to test it on RHEL9 on AWS:

```hcl
# Configure the AWS Provider
provider "aws" {
  region = "ap-southeast-2"
}

# Variable
variable "key_name" {
  type        = string
  default     = "ec2-pair"
  description = "AWS Key-pair"
}

# Find latest RHEL 9 AMI
data "aws_ami" "rhel9" {
  most_recent = true
  owners      = ["309956199498"] # Red Hat's AWS account ID

  filter {
    name   = "name"
    values = ["RHEL-9*"]
  }

  filter {
    name   = "architecture"
    values = ["x86_64"]
  }

  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }

  filter {
    name   = "root-device-type"
    values = ["ebs"]
  }
}

# Create a security group
resource "aws_security_group" "rhel9_sg" {
  name        = "rhel9_sg"
  description = "Security group for RHEL 9 EC2 seedbox instance"

  tags = {
    Name = "RHEL9-SecurityGroup"
  }
}

resource "aws_vpc_security_group_ingress_rule" "allow_rtorrent_port_tcp" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 55442
  ip_protocol       = "tcp"
  to_port           = 55442
  description       = "Default rtorrent_port (TCP)"
}

resource "aws_vpc_security_group_ingress_rule" "allow_rtorrent_port_udp" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 55442
  ip_protocol       = "udp"
  to_port           = 55442
  description       = "Default rtorrent_port (UDP)"
}

resource "aws_vpc_security_group_ingress_rule" "allow_pasv_port_range_tcp" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 64000
  ip_protocol       = "tcp"
  to_port           = 64321
  description       = "Default pasv_port_range (TCP)"
}

resource "aws_vpc_security_group_ingress_rule" "allow_pasv_port_range_udp" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 64000
  ip_protocol       = "udp"
  to_port           = 64321
  description       = "Default pasv_port_range (UDP)"
}

resource "aws_vpc_security_group_ingress_rule" "allow_ftp_port" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 55443
  ip_protocol       = "tcp"
  to_port           = 55443
  description       = "Default ftp_port"
}

resource "aws_vpc_security_group_ingress_rule" "allow_tls_ipv4" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 443
  ip_protocol       = "tcp"
  to_port           = 443
  description       = "Default ruTorrent port (IPv4)"
}

resource "aws_vpc_security_group_ingress_rule" "allow_autobrr_port" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 7474
  ip_protocol       = "tcp"
  to_port           = 7474
  description       = "Default Autobrr port"
}

resource "aws_vpc_security_group_ingress_rule" "allow_ssh_port" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  from_port         = 22
  ip_protocol       = "tcp"
  to_port           = 22
  description       = "Default SSH port"
}

resource "aws_vpc_security_group_ingress_rule" "allow_tls_ipv6" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv6         = "::/0"
  from_port         = 443
  ip_protocol       = "tcp"
  to_port           = 443
  description       = "Default ruTorrent port (IPv6)"
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv4" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv4         = "0.0.0.0/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}

resource "aws_vpc_security_group_egress_rule" "allow_all_traffic_ipv6" {
  security_group_id = aws_security_group.rhel9_sg.id
  cidr_ipv6         = "::/0"
  ip_protocol       = "-1" # semantically equivalent to all ports
}

# Create an EC2 instance
resource "aws_instance" "rhel_instance" {
  ami                    = data.aws_ami.rhel9.id
  instance_type          = "t3.medium"
  vpc_security_group_ids = [aws_security_group.rhel9_sg.id]
  key_name               = var.key_name # Replace with your key pair name

  root_block_device {
    volume_size = 20
    volume_type = "gp3"
    encrypted   = true
  }

  tags = {
    Name        = "RHEL-9-Seedbox"
    Environment = "Dev"
  }
}

# Output the instance details
output "instance_id" {
  value = aws_instance.rhel_instance.id
}

output "instance_public_ip" {
  value = aws_instance.rhel_instance.public_ip
}

output "instance_dns" {
  value = aws_instance.rhel_instance.public_dns
}
```

`time ansible-playbook -i inventory -u ec2-user test.yml --ask-vault-pass`

License
-------

MIT

Author Information
------------------

Lucian Maly <<lmaly@redhat.com>>

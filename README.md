# Ansible role for Wireguard
## Description
Ansible role to install and setup a [Wireguard](https://www.wireguard.com) VPN server.

## Prerequisites
* An EC2 instance (t3-micro recommended) with public Elastic IP address
* Ubuntu 18.04 Minimal
* S3 bucket with credentials for backup & centralize user config key

## Required variables

The *dictionary* list of users to add to the VPN, with username, unique private(local to the VPN node itself) IP address and settings:

```yaml
s3_bucket     : "nfq-infrastructure-terraform"
s3_key_prefix : "credentials/wireguard" # s3://<bucket-name>/credentials/wireguard
aws_access_key: "{{ lookup('env','AWS_ACCESS_KEY_ID') }}"
aws_secret_key: "{{ lookup('env','AWS_SECRET_ACCESS_KEY') }}"
region        : "ap-southeast-1"
wg_user_list:
  devops:
    username: "devops"
    private_ip: "10.99.0.11"
    default_route: true
    wg_dns_enabled: true
    remove: false
  nam.nguyen:
    username: "nam.nguyen"
    private_ip: "10.99.0.11"
    default_route: true
    wg_dns_enabled: true
    remove: false
  nghia.pham:
    username: "nghia.pham"
    private_ip: "10.99.0.12"
    default_route: true
    wg_dns_enabled: true
    remove: false
```

Decsription of the required parameters:
* `username`: username
* `private_ip`: private IP address to be assigned on Wireguard tunnel
* `default_route`: yes if user is allowed to use VPN as default route
* `wg_dns_enabled`: yes if user is allowed to use server DNS resolver
* `remove`: no (yes if user is marked to be deleted)

## Defaults
* Listen port: UDP/51820 (Set with `wg_listen_port`)
* Tunnel network: 10.99.0.0/24 (Set with `wg_private_ip` and change `private_ip` of each client)
* Download path for profile is default to `~/Downloads`, set in `wg_download_path`

## Optional variables
n/a

## Tags
* `sysctl` - Add IPv4 forwarding to Sysctl
* `install` - Install Wireguard
* `configure` - Configure Wireguard
* `users` - Add/remove users
* `upload` - Push client key into S3 bucket

## Usage
To use, either use [wg-quick](https://git.zx2c4.com/WireGuard/about/src/tools/man/wg-quick.8):

```bash
brew install wireguard-tools
wg-quick up ~/Downloads/path-to/wg0.conf
# Shutdown:
wg-quick down ~/Downloads/path-to/wg0.conf
```

Or use the official clients [here](https://www.wireguard.com/install/)

## Notes
* DNS push not working automatically on MacOS

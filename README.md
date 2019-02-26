## Ansible Wireguard VPN
---

#### Purpose
* Ansible role to install and manage a [Wireguard VPN server](www.wireguard.com)

#### Prerequisites
* An EC2 instance with public IP address (prefer Elastic IP), with
Ubuntu 18.04 installed
* dnsmasq (or the like) installed for DNS forwarding
* add `net.ipv4.ip_forward = 1` into `/etc/sysctl.conf` file

#### Defaults

* Listen host: Public IP address
* Listen port: UDP/51820 - configure with `wg_listen_port`
* Tunnel network: 10.99.0.0/24 - configure with `wg_private_ip`
* Routed traffic: 10.0.0.0/8, 172.31.0.0/16 - configure with `wg_routed_traffic`

#### Required variables

* `wg_user_list` with the following fields
  *  username
  *  private_ip: private IP address to be assigned on Wireguard tunnel
  *  default_route:
    - true if user will use VPN as default route (for all traffic)
    - false will use only preconfigured route `wg_routed_traffic`
  *  wg_dns_enabled: true if user will use server DNS resolver (will be able to query AWS internal DNS records)
  *  remove: true/false to mark user to be deleted

#### Tags

* `wg-install` - install service
* `wg-configure` - initial config
* `wg-users` - manage users

#### Notes

* Check ~/Download/<server_public_ip>/etc/wireguard/ directory for users
configs

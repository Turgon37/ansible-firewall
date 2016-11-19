Ansible Role Firewall
========

[![Build Status](https://travis-ci.org/Turgon37/ansible-firewall.svg?branch=master)](https://travis-ci.org/Turgon37/ansible-firewall)

This roles allow configuration of firewall rules

## OS Family

This role is available for Debian and CentOS

## Features

At this day the role can be used to configure :

  * IPv4 firewall rules

The IPv6 feature exist but has not been tested yet, so I discourage you to use it before I've performed some tests
The firewalld is not implemented because I consider this tools as to high-level to manage iptables rules. So it's that why I've decide (for now) to not use it.

## Configuration

  * The role accept this list of parameters

| Name                            | Description                                                                |
| ------------------------------- | -------------------------------------------------------------------------- |
| firewall__engine                | The name of the engine to use to setup the firewall. Choice in ['iptables']|
| iptables__service_name          | The name of the iptables service                                           |
|                                 |                                                                            |
| iptables__filter_input_policy   | The default policy for INPUT chain in FILTER table                         |
| iptables__filter_forward_policy | The default policy for FORWARD chain in FILTER table                       |
| iptables__filter_output_policy  | The default policy for OUTPUT chain in FILTER table                         |
| iptables__filter_rules          | The list of rules for FILTER table                                         |  
| iptables__nat_prerouting_policy | The default policy for PREROUTING chain in NAT table                            |
| iptables__nat_input_policy      | The default policy for INPUT chain in NAT table                            |
| iptables__nat_output_policy     | The default policy for OUTPUT chain in NAT table                            |
| iptables__nat_postrouting_policy| The default policy for POSTROUTING chain in NAT table                            |
| iptables__nat_rules             | The list of rules for NAT table                                            |

  * In each list of rules, each rules can take the following key-value (according to the condition at the right)

| Key                   | Format                                     | Description                                  | Available                       |
| ----------------------| -------------------------------------------| ---------------------------------------------|---------------------------------|
|chain                  | string                                     | the name of the chain to append the rule     |                                 |
|protocol               | tcp/udp/icmp/esp...                        | the protocol on which the rule will match    |                                 |
|not_protocol           | tcp/udp/icmp/esp...                        | the protocol on which the rule will NOT match|                                 |
|in_interface           | iface[+]                                   | the input interface match                    | only in input/forwarding chains |
|not_in_interface       | iface[+]                                   | the input interface does not match           | only in input/forwarding chains |
|out_interface          | iface[+]                                   | the output interface match                   | only in output/forwarding chains|
|not_out_interface      | iface[+]                                   | the output interface does not match          | only in output/forwarding chains|
|source_address         | X.X.X.X[/XX] or array [X.X.X.X[/XX], ...]  | the source address match                     |                                 |
|not_source_address     | X.X.X.X[/XX] or array [X.X.X.X[/XX], ...]  | the source address does not match            |                                 |
|destination_address    | X.X.X.X[/XX] or array [X.X.X.X[/XX], ...]  | the destination address match                |                                 |
|not_destination_address| X.X.X.X[/XX] or array [X.X.X.X[/XX], ...]  | the destination address does not match       |                                 |
|source_port            | int/service name (see /etc/services)       | the source port match                        |                                 |
|not_source_port        | int/service name (see /etc/services)       | the source port does not match               |                                 |
|destination_port       | int/service name (see /etc/services)       | the destination port match                   |                                 |
|not_destination_port   | int/service name (see /etc/services)       | the destination port does not match          |                                 |
|comment                | string                                     | a comment to add to the rule                 |                                 |
|target                 | see man 8 iptables-extensions              | the target of the rule                       |                                 |
|match                  | a complex match string (see example below) | the match sequence                           |                                 |

For the syntax of the interface name, please refer to the man 8 iptables

### Example

  * Simple rule to open port 53 from any host

```
iptables__filter_rules:
  - chain: input
    protocol: tcp
    source_address: 0.0.0.0/0
    in_interface: eth0
    destination_address: 172.16.0.0/12
    destination_port: 53
    comment: "Allow DNS queries from anyone"
    target: accept
```

  * Simple rule with automatic interface address lookup from ansible host vars

```
iptables__filter_rules:
  - chain: input
    protocol: tcp
    source_address: 172.16.0.0/12
    in_interface: eth0
    destination_address: "{{ hostvars[inventory_hostname]['ansible_eth0']['ipv4']['address'] }}"
    destination_port: 443
    comment: "Allow HTTPS"
    target: accept
```

  * Complex rule with match sequence

```
iptables__filter_rules:
  - chain: forward
    protocol: tcp
    in_interface: 'eth0'
    destination_address: 172.16.1.1
    out_interface: 'eth1'
    match: -m multiport --dports 25,143,465,587,993
    comment: "Forward all IMAP/SMTP ports"
    target: accept
```

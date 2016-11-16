Ansible Role Firewall
========

This roles allow configuration of firewall rules

## OS Family

This role is available for Debian and CentOS

## Features

At this day the role can be used to configure :

  * IPv4 firewall rules

The IPv6 feature exist but has not been tested yet, so I discourage you to use it before I've performed some tests
The firewalld is not implemented because I consider this tools as to high-level to manage iptables rules. So it's that why I've decide (for now) to not use it.

## Configuration

The variables that can be passed to this role and a brief description about them are as follows:

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

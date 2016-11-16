Ansible Role Firewall
========

This roles allow configuration of firewall rules

## OS Family

This role is available for Debian and CentOS

## Features

At this day the role can be used to configure :

  * IPv4 firewall rules

The IPv6 feature exist but has not been tested yet, so I discourage you to use it before I've performed some tests

## Configuration

The variables that can be passed to this role and a brief description about them are as follows:

| Name                            | Description                                                                |
| ------------------------------- | -------------------------------------------------------------------------- |
| firewall__engine                | The name of the engine to use to setup the firewall. Choice in ['iptables']|
| iptables__service_name          | The name of the iptables service                                           |
|                                 |                                                                            |
| iptables__filter_input_policy   | The default policy for input chain in FILTER table                         |
| iptables__filter_forward_policy | The default policy for forward chain in FILTER table                       |
| iptables__filter_output_policy  | The default policy for ouput chain in FILTER table                         |
| iptables__filter_rules          | The list of rules for FILTER table                                         |  
| iptables__nat_prerouting_policy | The default policy for input chain in NAT table                            |
| iptables__nat_input_policy      | The default policy for input chain in NAT table                            |
| iptables__nat_output_policy     | The default policy for input chain in NAT table                            |
| iptables__nat_postrouting_policy| The default policy for input chain in NAT table                            |
| iptables__nat_rules             | The list of rules for NAT table                                            |

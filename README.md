ryezone_labs.edge_router
=========

This role configures iptables firewall rules for an edge router.

Role Variables
--------------

## Global Role Variables

See [Common Variables](https://github.com/ryezone-labs/documentation/blob/master/common-variables.md)

Common Variables used:

- `ryezone_labs_wanInterface`: eth0
- `ryezone_labs_lanInterface`: eth1
- `ryezone_labs_lanAddress`: 10.100.10.1

## Allowed Incoming TCP Ports

```yaml
edge_firewall_open_incoming_tcp_ports:
  - cidr: 172.16.0.0/12
    port: 22
```

- `cidr` (string) IP Address CIDR block from which to allow connections.
- `port` (string) Port(s) from which to accept connections.

## Allowed ICMP Protocols

```yaml
edge_firewall_allowed_incoming_icmp:
  - cidr: 172.16.0.0/12
    icmp_type: 8
```

- `cidr` (string) IP Address CIDR block from which to allow connections.
- `icmp_type` (integer) ICMP Packets 

## Rejected Incoming UDP Ports

```yaml
edge_firewall_closed_incoming_udp_ports:
  - cidr: 172.16.0.0/12
    port: 33434:33523
```

- `cidr` (string) IP Address CIDR block from which to reject connections.
- `port` (string) Port(s) from which to block connections.

## NAT Rules

```yaml
edge_firewall_nat_rules:
  - protocol: tcp
    wan_interface: "{{ ryezone_labs_wanInterface }}"
    wan_port: 80
    lan_interface: "{{ ryezone_labs_lanInterface }}"
    lan_address: "{{ ryezone_labs_lanAddress }}"
    host_port: 80
    host_address: 10.100.10.11
```

- `protocol` (string) Protocol to map through the firewall.
- `wan_interface` (string) WAN Interface to accept traffic on.
- `wan_port` (string) WAN port to accept traffic from and map through the firewall.
- `lan_interface` (string) LAN Interface to route traffic to.
- `lan_address` (string) LAN Interface IP Address to route traffic to.  Used for packet source address rewrites.
- `host_address` (string) Host IP Address to route traffic to.  Used for packet source address rewrites.
- `host_port` (string) Host Port to route traffic to.

Example Playbook
----------------

```yaml
- hosts: 127.0.0.1
  connection: local
  vars:
    ryezone_labs_wanInterface: eth0
    ryezone_labs_lanInterface: eth1
    ryezone_labs_lanAddress: 10.100.10.1
    edge_firewall_open_incoming_tcp_ports:
      - cidr: 172.16.0.0/12
        port: 22
    edge_firewall_allowed_incoming_icmp:
      - icmp_type: 8
        cidr: 172.16.0.0/12
    edge_firewall_closed_incoming_udp_ports:
      - cidr: 172.16.0.0/12
        port: 33434:33523
    edge_firewall_nat_rules:
      - protocol: tcp
        wan_interface: "{{ ryezone_labs_wanInterface }}"
        wan_port: 80
        lan_interface: "{{ ryezone_labs_lanInterface }}"
        lan_address: "{{ ryezone_labs_lanAddress }}"
        host_port: 80
        host_address: 10.100.10.11
  roles:
    - ryezone_labs.edge_firewall
```

License
-------

BSD

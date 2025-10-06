# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| DC1 | l3leaf | dc1-leaf1 | 192.168.0.13/24 | - | Provisioned | - |
| DC1 | l3leaf | dc1-leaf2 | 192.168.0.14/24 | - | Provisioned | - |
| DC1 | l3leaf | dc1-leaf3 | 192.168.0.15/24 | - | Provisioned | - |
| DC1 | l3leaf | dc1-leaf4 | 192.168.0.16/24 | - | Provisioned | - |
| DC1 | spine | dc1-spine1 | 192.168.0.11/24 | - | Provisioned | - |
| DC1 | spine | dc1-spine2 | 192.168.0.12/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf1 | 192.168.0.23/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf2 | 192.168.0.24/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf3 | 192.168.0.25/24 | - | Provisioned | - |
| DC2 | l3leaf | dc2-leaf4 | 192.168.0.26/24 | - | Provisioned | - |
| DC2 | spine | dc2-spine1 | 192.168.0.21/24 | - | Provisioned | - |
| DC2 | spine | dc2-spine2 | 192.168.0.22/24 | - | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | dc1-leaf1 | Ethernet7 | l3leaf | dc2-leaf1 | Ethernet7 |
| l3leaf | dc1-leaf1 | Ethernet8 | l3leaf | dc2-leaf2 | Ethernet8 |
| l3leaf | dc1-leaf1 | Ethernet9 | spine | dc1-spine1 | Ethernet1 |
| l3leaf | dc1-leaf1 | Ethernet10 | spine | dc1-spine2 | Ethernet1 |
| l3leaf | dc1-leaf1 | Ethernet11 | mlag_peer | dc1-leaf2 | Ethernet11 |
| l3leaf | dc1-leaf1 | Ethernet12 | mlag_peer | dc1-leaf2 | Ethernet12 |
| l3leaf | dc1-leaf2 | Ethernet7 | l3leaf | dc2-leaf2 | Ethernet7 |
| l3leaf | dc1-leaf2 | Ethernet8 | l3leaf | dc2-leaf1 | Ethernet8 |
| l3leaf | dc1-leaf2 | Ethernet9 | spine | dc1-spine1 | Ethernet2 |
| l3leaf | dc1-leaf2 | Ethernet10 | spine | dc1-spine2 | Ethernet2 |
| l3leaf | dc1-leaf3 | Ethernet9 | spine | dc1-spine1 | Ethernet3 |
| l3leaf | dc1-leaf3 | Ethernet10 | spine | dc1-spine2 | Ethernet3 |
| l3leaf | dc1-leaf3 | Ethernet11 | mlag_peer | dc1-leaf4 | Ethernet11 |
| l3leaf | dc1-leaf3 | Ethernet12 | mlag_peer | dc1-leaf4 | Ethernet12 |
| l3leaf | dc1-leaf4 | Ethernet9 | spine | dc1-spine1 | Ethernet4 |
| l3leaf | dc1-leaf4 | Ethernet10 | spine | dc1-spine2 | Ethernet4 |
| l3leaf | dc2-leaf1 | Ethernet9 | spine | dc2-spine1 | Ethernet1 |
| l3leaf | dc2-leaf1 | Ethernet10 | spine | dc2-spine2 | Ethernet1 |
| l3leaf | dc2-leaf1 | Ethernet11 | mlag_peer | dc2-leaf2 | Ethernet11 |
| l3leaf | dc2-leaf1 | Ethernet12 | mlag_peer | dc2-leaf2 | Ethernet12 |
| l3leaf | dc2-leaf2 | Ethernet9 | spine | dc2-spine1 | Ethernet2 |
| l3leaf | dc2-leaf2 | Ethernet10 | spine | dc2-spine2 | Ethernet2 |
| l3leaf | dc2-leaf3 | Ethernet9 | spine | dc2-spine1 | Ethernet3 |
| l3leaf | dc2-leaf3 | Ethernet10 | spine | dc2-spine2 | Ethernet3 |
| l3leaf | dc2-leaf3 | Ethernet11 | mlag_peer | dc2-leaf4 | Ethernet11 |
| l3leaf | dc2-leaf3 | Ethernet12 | mlag_peer | dc2-leaf4 | Ethernet12 |
| l3leaf | dc2-leaf4 | Ethernet9 | spine | dc2-spine1 | Ethernet4 |
| l3leaf | dc2-leaf4 | Ethernet10 | spine | dc2-spine2 | Ethernet4 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.1.2.0/24 | 256 | 16 | 6.25 % |
| 10.2.2.0/24 | 256 | 16 | 6.25 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| dc1-leaf1 | Ethernet7 | 10.0.0.0/31 | dc2-leaf1 | Ethernet7 | 10.0.0.1/31 |
| dc1-leaf1 | Ethernet8 | 10.0.0.2/31 | dc2-leaf2 | Ethernet8 | 10.0.0.3/31 |
| dc1-leaf1 | Ethernet9 | 10.1.2.1/31 | dc1-spine1 | Ethernet1 | 10.1.2.0/31 |
| dc1-leaf1 | Ethernet10 | 10.1.2.3/31 | dc1-spine2 | Ethernet1 | 10.1.2.2/31 |
| dc1-leaf2 | Ethernet7 | 10.0.0.4/31 | dc2-leaf2 | Ethernet7 | 10.0.0.5/31 |
| dc1-leaf2 | Ethernet8 | 10.0.0.6/31 | dc2-leaf1 | Ethernet8 | 10.0.0.7/31 |
| dc1-leaf2 | Ethernet9 | 10.1.2.5/31 | dc1-spine1 | Ethernet2 | 10.1.2.4/31 |
| dc1-leaf2 | Ethernet10 | 10.1.2.7/31 | dc1-spine2 | Ethernet2 | 10.1.2.6/31 |
| dc1-leaf3 | Ethernet9 | 10.1.2.9/31 | dc1-spine1 | Ethernet3 | 10.1.2.8/31 |
| dc1-leaf3 | Ethernet10 | 10.1.2.11/31 | dc1-spine2 | Ethernet3 | 10.1.2.10/31 |
| dc1-leaf4 | Ethernet9 | 10.1.2.13/31 | dc1-spine1 | Ethernet4 | 10.1.2.12/31 |
| dc1-leaf4 | Ethernet10 | 10.1.2.15/31 | dc1-spine2 | Ethernet4 | 10.1.2.14/31 |
| dc2-leaf1 | Ethernet9 | 10.2.2.1/31 | dc2-spine1 | Ethernet1 | 10.2.2.0/31 |
| dc2-leaf1 | Ethernet10 | 10.2.2.3/31 | dc2-spine2 | Ethernet1 | 10.2.2.2/31 |
| dc2-leaf2 | Ethernet9 | 10.2.2.5/31 | dc2-spine1 | Ethernet2 | 10.2.2.4/31 |
| dc2-leaf2 | Ethernet10 | 10.2.2.7/31 | dc2-spine2 | Ethernet2 | 10.2.2.6/31 |
| dc2-leaf3 | Ethernet9 | 10.2.2.9/31 | dc2-spine1 | Ethernet3 | 10.2.2.8/31 |
| dc2-leaf3 | Ethernet10 | 10.2.2.11/31 | dc2-spine2 | Ethernet3 | 10.2.2.10/31 |
| dc2-leaf4 | Ethernet9 | 10.2.2.13/31 | dc2-spine1 | Ethernet4 | 10.2.2.12/31 |
| dc2-leaf4 | Ethernet10 | 10.2.2.15/31 | dc2-spine2 | Ethernet4 | 10.2.2.14/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.1.0.0/24 | 256 | 6 | 2.35 % |
| 10.2.0.0/24 | 256 | 6 | 2.35 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| DC1 | dc1-leaf1 | 10.1.0.3/32 |
| DC1 | dc1-leaf2 | 10.1.0.4/32 |
| DC1 | dc1-leaf3 | 10.1.0.5/32 |
| DC1 | dc1-leaf4 | 10.1.0.6/32 |
| DC1 | dc1-spine1 | 10.1.0.1/32 |
| DC1 | dc1-spine2 | 10.1.0.2/32 |
| DC2 | dc2-leaf1 | 10.2.0.3/32 |
| DC2 | dc2-leaf2 | 10.2.0.4/32 |
| DC2 | dc2-leaf3 | 10.2.0.5/32 |
| DC2 | dc2-leaf4 | 10.2.0.6/32 |
| DC2 | dc2-spine1 | 10.2.0.1/32 |
| DC2 | dc2-spine2 | 10.2.0.2/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------------ | ------------------- | ------------------ | ------------------ |
| 10.1.1.0/24 | 256 | 4 | 1.57 % |
| 10.2.1.0/24 | 256 | 4 | 1.57 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| DC1 | dc1-leaf1 | 10.1.1.3/32 |
| DC1 | dc1-leaf2 | 10.1.1.3/32 |
| DC1 | dc1-leaf3 | 10.1.1.5/32 |
| DC1 | dc1-leaf4 | 10.1.1.5/32 |
| DC2 | dc2-leaf1 | 10.2.1.3/32 |
| DC2 | dc2-leaf2 | 10.2.1.3/32 |
| DC2 | dc2-leaf3 | 10.2.1.5/32 |
| DC2 | dc2-leaf4 | 10.2.1.5/32 |

# dc2-spine1

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SFlow](#sflow)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Switchport Default](#switchport-default)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Router BGP](#router-bgp)
- [Multicast](#multicast)
  - [Router Multicast](#router-multicast)
  - [PIM Sparse Mode](#pim-sparse-mode)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | mgmt | 192.168.0.21/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | mgmt | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf mgmt
   ip address 192.168.0.21/24
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **Europe/Stockholm**.

#### Clock Device Configuration

```eos
!
clock timezone Europe/Stockholm
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| se.pool.ntp.org | mgmt | True | - | - | 4 | - | - | Management1 | - |

#### NTP Device Configuration

```eos
!
ntp server vrf mgmt se.pool.ntp.org prefer version 4 local-interface Management1
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| mgmt | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf mgmt
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| cvpadmin | 15 | network-admin | False | - |
| df | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
username df privilege 15 role network-admin secret sha512 <removed>
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | mgmt | token,/mnt/flash/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/mnt/flash/token -cvvrf=mgmt -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

### SFlow

#### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | Loopback0 | - | - |

sFlow Sample Rate: 7000

sFlow is enabled.

#### SFlow Device Configuration

```eos
!
sflow sample 7000
sflow destination 127.0.0.1
sflow source-interface Loopback0
sflow run
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 3700 | 3900 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 3700 3900
```

## MAC Address Table

### MAC Address Table Summary

- MAC address table entry maximum age: 901 seconds

### MAC Address Table Device Configuration

```eos
!
mac address-table aging-time 901
```

## Interfaces

### Switchport Default

#### Switchport Defaults Summary

- Default Switchport Mode: routed

#### Switchport Default Device Configuration

```eos
!
switchport default mode routed
```

### Interface Defaults

#### Interface Defaults Summary

- Default Ethernet Interface Shutdown: True

#### Interface Defaults Device Configuration

```eos
!
interface defaults
   ethernet
      shutdown
```

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1 | P2P_LINK_TO_DC2-LEAF1_Ethernet9 | routed | - | 10.2.2.0/31 | default | 1500 | False | - | - |
| Ethernet2 | P2P_LINK_TO_DC2-LEAF2_Ethernet9 | routed | - | 10.2.2.4/31 | default | 1500 | False | - | - |
| Ethernet3 | P2P_LINK_TO_DC2-LEAF3_Ethernet9 | routed | - | 10.2.2.8/31 | default | 1500 | False | - | - |
| Ethernet4 | P2P_LINK_TO_DC2-LEAF4_Ethernet9 | routed | - | 10.2.2.12/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description P2P_LINK_TO_DC2-LEAF1_Ethernet9
   no shutdown
   mtu 1500
   no switchport
   ip address 10.2.2.0/31
   pim ipv4 sparse-mode
!
interface Ethernet2
   description P2P_LINK_TO_DC2-LEAF2_Ethernet9
   no shutdown
   mtu 1500
   no switchport
   ip address 10.2.2.4/31
   pim ipv4 sparse-mode
!
interface Ethernet3
   description P2P_LINK_TO_DC2-LEAF3_Ethernet9
   no shutdown
   mtu 1500
   no switchport
   ip address 10.2.2.8/31
   pim ipv4 sparse-mode
!
interface Ethernet4
   description P2P_LINK_TO_DC2-LEAF4_Ethernet9
   no shutdown
   mtu 1500
   no switchport
   ip address 10.2.2.12/31
   pim ipv4 sparse-mode
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.2.0.1/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.2.0.1/32
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| mgmt | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf mgmt
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| mgmt | false |

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65200 | 10.2.0.1 |

| BGP Tuning |
| ---------- |
| distance bgp 150 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| maximum-paths 2 ecmp 2 |

#### Router BGP Peer Groups

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.2.2.1 | 65201 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.2.2.5 | 65201 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.2.2.9 | 65202 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.2.2.13 | 65202 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65200
   router-id 10.2.0.1
   maximum-paths 2 ecmp 2
   no bgp default ipv4-unicast
   distance bgp 150 200 200
   graceful-restart restart-time 300
   graceful-restart
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.2.2.1 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.2.2.1 remote-as 65201
   neighbor 10.2.2.1 description dc2-leaf1_Ethernet9
   neighbor 10.2.2.5 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.2.2.5 remote-as 65201
   neighbor 10.2.2.5 description dc2-leaf2_Ethernet9
   neighbor 10.2.2.9 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.2.2.9 remote-as 65202
   neighbor 10.2.2.9 description dc2-leaf3_Ethernet9
   neighbor 10.2.2.13 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.2.2.13 remote-as 65202
   neighbor 10.2.2.13 description dc2-leaf4_Ethernet9
   redistribute connected
   !
   address-family ipv4
      neighbor IPv4-UNDERLAY-PEERS activate
```

## Multicast

### Router Multicast

#### IP Router Multicast Summary

- Routing for IPv4 multicast is enabled.

#### Router Multicast Device Configuration

```eos
!
router multicast
   ipv4
      routing
```

### PIM Sparse Mode

#### PIM Sparse Mode Enabled Interfaces

| Interface Name | VRF Name | IP Version | Border Router | DR Priority | Local Interface |
| -------------- | -------- | ---------- | ------------- | ----------- | --------------- |
| Ethernet1 | - | IPv4 | - | - | - |
| Ethernet2 | - | IPv4 | - | - | - |
| Ethernet3 | - | IPv4 | - | - | - |
| Ethernet4 | - | IPv4 | - | - | - |

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| mgmt | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance mgmt
```

# dc2-leaf1

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [SFlow](#sflow)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Switchport Default](#switchport-default)
  - [Interface Defaults](#interface-defaults)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | OOB_MANAGEMENT | oob | default | 192.168.0.23/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | OOB_MANAGEMENT | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description OOB_MANAGEMENT
   no shutdown
   ip address 192.168.0.23/24
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
| se.pool.ntp.org | default | True | - | - | 4 | - | - | Management1 | - |

#### NTP Device Configuration

```eos
!
ntp server se.pool.ntp.org prefer version 4 local-interface Management1
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| cvpadmin | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username cvpadmin privilege 15 role network-admin secret sha512 <removed>
```

### Enable Password

Enable password has been disabled

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | default | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=default -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
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

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| dc2-leaf-pair1 | Vlan4094 | 10.2.3.1 | Port-Channel11 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id dc2-leaf-pair1
   local-interface Vlan4094
   peer-address 10.2.3.1
   peer-link Port-Channel11
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
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

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 2 | TEST_VLAN2 | - |
| 3 | TEST_VLAN3 | - |
| 5 | TEST_VLAN5 | - |
| 10 | TEST_VLAN10 | - |
| 100 | TEST_VLAN100 | - |
| 3000 | MLAG_L3_VRF_FW_TEST1 | MLAG |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 2
   name TEST_VLAN2
!
vlan 3
   name TEST_VLAN3
!
vlan 5
   name TEST_VLAN5
!
vlan 10
   name TEST_VLAN10
!
vlan 100
   name TEST_VLAN100
!
vlan 3000
   name MLAG_L3_VRF_FW_TEST1
   trunk group MLAG
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
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
| Ethernet1 | SERVER_SERVER_GW2_Eth0 | *access | *100 | *- | *- | 1 |
| Ethernet2 | SERVER_SERVER2-1_Eth0 | *access | *10 | *- | *- | 2 |
| Ethernet11 | MLAG_dc2-leaf2_Ethernet11 | *trunk | *- | *- | *MLAG | 11 |
| Ethernet12 | MLAG_dc2-leaf2_Ethernet12 | *trunk | *- | *- | *MLAG | 11 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet7 | P2P_dc1-leaf1_Ethernet7 | - | 10.0.0.1/31 | default | 1500 | False | - | - |
| Ethernet8 | P2P_dc1-leaf2_Ethernet8 | - | 10.0.0.7/31 | default | 1500 | False | - | - |
| Ethernet9 | P2P_dc2-spine1_Ethernet1 | - | 10.2.2.1/31 | default | 1500 | False | - | - |
| Ethernet10 | P2P_dc2-spine2_Ethernet1 | - | 10.2.2.3/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description SERVER_SERVER_GW2_Eth0
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description SERVER_SERVER2-1_Eth0
   no shutdown
   channel-group 2 mode active
!
interface Ethernet7
   description P2P_dc1-leaf1_Ethernet7
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.1/31
!
interface Ethernet8
   description P2P_dc1-leaf2_Ethernet8
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.7/31
!
interface Ethernet9
   description P2P_dc2-spine1_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.2.2.1/31
!
interface Ethernet10
   description P2P_dc2-spine2_Ethernet1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.2.2.3/31
!
interface Ethernet11
   description MLAG_dc2-leaf2_Ethernet11
   no shutdown
   channel-group 11 mode active
!
interface Ethernet12
   description MLAG_dc2-leaf2_Ethernet12
   no shutdown
   channel-group 11 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | SERVER_GW2 | access | 100 | - | - | - | - | 1 | - |
| Port-Channel2 | SERVER2-1 | access | 10 | - | - | - | - | 2 | - |
| Port-Channel11 | MLAG_dc2-leaf2_Port-Channel11 | trunk | - | - | MLAG | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description SERVER_GW2
   no shutdown
   switchport access vlan 100
   switchport mode access
   switchport
   mlag 1
   spanning-tree portfast
!
interface Port-Channel2
   description SERVER2-1
   no shutdown
   switchport access vlan 10
   switchport mode access
   switchport
   mlag 2
   spanning-tree portfast
!
interface Port-Channel11
   description MLAG_dc2-leaf2_Port-Channel11
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.2.0.3/32 |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | 10.2.1.3/32 |
| Loopback3 | IGMP snooping querier | FW_TEST1 | 10.255.0.3/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |
| Loopback1 | VXLAN_TUNNEL_SOURCE | default | - |
| Loopback3 | IGMP snooping querier | FW_TEST1 | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.2.0.3/32
!
interface Loopback1
   description VXLAN_TUNNEL_SOURCE
   no shutdown
   ip address 10.2.1.3/32
!
interface Loopback3
   description IGMP snooping querier
   no shutdown
   vrf FW_TEST1
   ip address 10.255.0.3/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan10 | TEST_VLAN10 | FW_TEST1 | 1400 | False |
| Vlan100 | TEST_VLAN100 | FW_TEST1 | 1400 | False |
| Vlan3000 | MLAG_L3_VRF_FW_TEST1 | FW_TEST1 | 1500 | False |
| Vlan4093 | MLAG_L3 | default | 1500 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan10 |  FW_TEST1  |  -  |  10.10.0.1/24  |  -  |  -  |  -  |
| Vlan100 |  FW_TEST1  |  -  |  10.100.0.1/24  |  -  |  -  |  -  |
| Vlan3000 |  FW_TEST1  |  10.2.4.0/31  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.2.4.0/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.2.3.0/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan10
   description TEST_VLAN10
   no shutdown
   mtu 1400
   vrf FW_TEST1
   ip address virtual 10.10.0.1/24
!
interface Vlan100
   description TEST_VLAN100
   no shutdown
   mtu 1400
   vrf FW_TEST1
   ip address virtual 10.100.0.1/24
!
interface Vlan3000
   description MLAG_L3_VRF_FW_TEST1
   no shutdown
   mtu 1500
   vrf FW_TEST1
   ip address 10.2.4.0/31
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 1500
   ip address 10.2.4.0/31
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 10.2.3.0/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 2 | 10002 | - | - |
| 3 | 10003 | - | - |
| 5 | 10005 | - | - |
| 10 | 10010 | - | - |
| 100 | 10100 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Overlay Multicast Group to Encap Mappings |
| --- | --- | ----------------------------------------- |
| FW_TEST1 | 1 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description dc2-leaf1_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 2 vni 10002
   vxlan vlan 3 vni 10003
   vxlan vlan 5 vni 10005
   vxlan vlan 10 vni 10010
   vxlan vlan 100 vni 10100
   vxlan vrf FW_TEST1 vni 1
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:11:22:33:44:55

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:11:22:33:44:55
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| FW_TEST1 | True |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf FW_TEST1
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |
| FW_TEST1 | false |

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65201 | 10.2.0.3 |

| BGP Tuning |
| ---------- |
| distance bgp 150 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| maximum-paths 2 ecmp 2 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Next-hop unchanged | True |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 4 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### LISTENRANGE

| Settings | Value |
| -------- | ----- |
| Remote AS | 65535 |
| Local AS | 65534 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65201 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 10.0.0.0 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | False | - | - | - | - |
| 10.0.0.6 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | False | - | - | - | - |
| 10.1.0.3 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.1.0.4 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.2.0.5 | 65202 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.2.0.6 | 65202 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 10.2.2.0 | 65200 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.2.2.2 | 65200 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 10.2.4.1 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 10.2.4.1 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | FW_TEST1 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Route-map In | Route-map Out | Peer-tag In | Peer-tag Out | Encapsulation | Next-hop-self Source Interface |
| ---------- | -------- | ------------ | ------------- | ----------- | ------------ | ------------- | ------------------------------ |
| EVPN-OVERLAY-PEERS | True | - | - | - | - | default | - |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 2 | 10.2.0.3:10002 | 65000:10002 | - | - | learned |
| 3 | 10.2.0.3:10003 | 65000:10003 | - | - | learned |
| 5 | 10.2.0.3:10005 | 65000:10005 | - | - | learned |
| 10 | 10.2.0.3:10010 | 65000:10010 | - | - | learned |
| 100 | 10.2.0.3:10100 | 65000:10100 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute | Graceful Restart |
| --- | ------------------- | ------------ | ---------------- |
| FW_TEST1 | 10.2.0.3:1 | connected | - |

#### Router BGP Device Configuration

```eos
!
router bgp 65201
   router-id 10.2.0.3
   no bgp default ipv4-unicast
   maximum-paths 2 ecmp 2
   distance bgp 150 200 200
   graceful-restart restart-time 300
   graceful-restart
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 4
   neighbor EVPN-OVERLAY-PEERS password 7 <removed>
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS password 7 <removed>
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor LISTENRANGE peer group
   neighbor LISTENRANGE remote-as 65535
   neighbor LISTENRANGE local-as 65534 no-prepend replace-as
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65201
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description dc2-leaf2
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor MLAG-IPv4-UNDERLAY-PEER password 7 <removed>
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor 10.0.0.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.0 remote-as 65101
   no neighbor 10.0.0.0 bfd
   neighbor 10.0.0.0 description dc1-leaf1
   neighbor 10.0.0.6 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.6 remote-as 65101
   no neighbor 10.0.0.6 bfd
   neighbor 10.0.0.6 description dc1-leaf2
   neighbor 10.1.0.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.1.0.3 remote-as 65101
   neighbor 10.1.0.3 description dc1-leaf1_Loopback0
   neighbor 10.1.0.3 route-map RM-EVPN-FILTER-AS65101 out
   neighbor 10.1.0.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.1.0.4 remote-as 65101
   neighbor 10.1.0.4 description dc1-leaf2_Loopback0
   neighbor 10.1.0.4 route-map RM-EVPN-FILTER-AS65101 out
   neighbor 10.2.0.5 peer group EVPN-OVERLAY-PEERS
   neighbor 10.2.0.5 remote-as 65202
   neighbor 10.2.0.5 description dc2-leaf3_Loopback0
   neighbor 10.2.0.6 peer group EVPN-OVERLAY-PEERS
   neighbor 10.2.0.6 remote-as 65202
   neighbor 10.2.0.6 description dc2-leaf4_Loopback0
   neighbor 10.2.2.0 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.2.2.0 remote-as 65200
   neighbor 10.2.2.0 description dc2-spine1_Ethernet1
   neighbor 10.2.2.2 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.2.2.2 remote-as 65200
   neighbor 10.2.2.2 description dc2-spine2_Ethernet1
   neighbor 10.2.4.1 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.2.4.1 description dc2-leaf2_Vlan4093
   redistribute connected
   !
   vlan 2
      rd 10.2.0.3:10002
      route-target both 65000:10002
      redistribute learned
   !
   vlan 3
      rd 10.2.0.3:10003
      route-target both 65000:10003
      redistribute learned
   !
   vlan 5
      rd 10.2.0.3:10005
      route-target both 65000:10005
      redistribute learned
   !
   vlan 10
      rd 10.2.0.3:10010
      route-target both 65000:10010
      redistribute learned
   !
   vlan 100
      rd 10.2.0.3:10100
      route-target both 65000:10100
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf FW_TEST1
      rd 10.2.0.3:1
      route-target import evpn 65000:1
      route-target export evpn 65000:1
      router-id 10.2.0.3
      neighbor 10.2.4.1 peer group MLAG-IPv4-UNDERLAY-PEER
      neighbor 10.2.4.1 description dc2-leaf2_Vlan3000
      redistribute connected route-map RM-CONN-2-BGP-VRFS
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 5000 | 5000 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 5000 min-rx 5000 multiplier 3
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

##### IP IGMP Snooping Vlan Summary

| Vlan | IGMP Snooping | Fast Leave | Max Groups | Proxy |
| ---- | ------------- | ---------- | ---------- | ----- |
| 10 | - | - | - | - |
| 100 | - | - | - | - |

| Vlan | Querier Enabled | IP Address | Query Interval | Max Response Time | Last Member Query Interval | Last Member Query Count | Startup Query Interval | Startup Query Count | Version |
| ---- | --------------- | ---------- | -------------- | ----------------- | -------------------------- | ----------------------- | ---------------------- | ------------------- | ------- |
| 10 | True | 10.2.0.3 | - | - | - | - | - | - | 3 |
| 100 | True | 10.2.0.3 | - | - | - | - | - | - | 3 |

#### IP IGMP Snooping Device Configuration

```eos
!
ip igmp snooping vlan 10 querier
ip igmp snooping vlan 10 querier address 10.2.0.3
ip igmp snooping vlan 10 querier version 3
ip igmp snooping vlan 100 querier
ip igmp snooping vlan 100 querier address 10.2.0.3
ip igmp snooping vlan 100 querier version 3
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-MLAG-PEER-VRFS

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.2.4.0/31 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-MLAG-PEER-VRFS
   seq 10 permit 10.2.4.0/31
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP-VRFS

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | ip address prefix-list PL-MLAG-PEER-VRFS | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-EVPN-FILTER-AS65101

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | deny | as 65101 | - | - | - |
| 20 | permit | - | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP-VRFS deny 10
   match ip address prefix-list PL-MLAG-PEER-VRFS
!
route-map RM-CONN-2-BGP-VRFS permit 20
!
route-map RM-EVPN-FILTER-AS65101 deny 10
   match as 65101
!
route-map RM-EVPN-FILTER-AS65101 permit 20
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| FW_TEST1 | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance FW_TEST1
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IPv4 Address | Source NAT IPv6 Address |
| -------------- | ----------------------- | ----------------------- |
| FW_TEST1 | 10.255.0.3 | - |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf FW_TEST1 address 10.255.0.3
```

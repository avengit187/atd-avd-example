# s1-leaf4
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [RADIUS Servers](#radius-servers)
  - [IP RADIUS Source Interfaces](#ip-radius-source-interfaces)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
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
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | oob_management | oob | default | 192.168.0.15/24 | 192.168.0.1 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | oob_management | oob | default | -  | - |

### Management Interfaces Device Configuration

```eos
!
interface Management0
   description oob_management
   no shutdown
   ip address 192.168.0.15/24
```

## DNS Domain

### DNS domain: atd.lab

### DNS Domain Device Configuration

```eos
dns domain atd.lab
!
```

## NTP

### NTP Summary

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.0.1 | - | True | - | True | - | - | - | Management0 | - |

### NTP Device Configuration

```eos
!
ntp server 192.168.0.1 prefer iburst local-interface Management0
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role |
| ---- | --------- | ---- |
| arista | 15 | network-admin |

### Local Users Device Configuration

```eos
!
username arista privilege 15 role network-admin secret sha512 $6$4FFVdsOX/1WIH/86$6eYwOGBNE5x4AMFE0CEujICWTJy4kc3jNGGg6GP9PbWcNgGhxHC9rscfjf3d3f797GYpVGaugiUSEmfhVWxls0
username arista ssh-key ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC1yElfRotgcSgF1qY7G3bYOL/M/D3a72Tnrw24FoW58JiPlxCRw+2YofecauC4JimjbBCJobmnnjCBxzwRY64yAXRPd6ChBtqfqXpPbrpnfpcMHIqAyam2FC8t0HgWu5W6iOFvWpjgHKN8UzC3nSyNLMt/x/LYI4uKs1KIn+jIkBumS06VhOtdhUf+zcaj6waTGg1ugr0vWW+a8Reo9bu90ku6dn96/ABgRA+ldA7c1NIeAl6MQUpaWjtsXX/1BDckABJgJf9ZWIRqBaTcJnVjvhmt7T6GO3BKYVX/ykwZu1lQjkqftCY737rIhUAu5fypEegm3GO8tjsRTwuOU3rx arista@bnc-rolex-1-085e5ba3-eos
```

## RADIUS Servers

### RADIUS Servers

| VRF | RADIUS Servers |
| --- | ---------------|
| default | 192.168.0.1 |

### RADIUS Servers Device Configuration

```eos
!
radius-server host 192.168.0.1 key 7 0207165218120E
```

## IP RADIUS Source Interfaces

### IP RADIUS Source Interfaces

| VRF | Source Interface Name |
| --- | --------------- |
| default | Management0 |

### IP SOURCE Source Interfaces Device Configuration

```eos
!
ip radius source-interface Management0
```

## AAA Server Groups

### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| atds | radius | default | 192.168.0.1 |

### AAA Server Groups Device Configuration

```eos
!
aaa group server radius atds
   server 192.168.0.1
```

## AAA Authentication

### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | group atds local |

### AAA Authentication Device Configuration

```eos
aaa authentication login default group atds local
!
```

## AAA Authorization

### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | group atds local |

Authorization for configuration commands is disabled.

### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| all | local |

### AAA Authorization Device Configuration

```eos
aaa authorization exec default group atds local
aaa authorization commands all default local
!
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | default | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=default -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

# MLAG

## MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| SITE1_DC_LEAF2 | Vlan4094 | 10.1.6.4 | Port-Channel1 |

Dual primary detection is disabled.

## MLAG Device Configuration

```eos
!
mlag configuration
   domain-id SITE1_DC_LEAF2
   local-interface Vlan4094
   peer-address 10.1.6.4
   peer-link Port-Channel1
   reload-delay mlag 300
   reload-delay non-mlag 330
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 32768 |

### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 32768
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 101 | BLUE_VLAN_101 | - |
| 202 | GREEN_VLAN_202 | - |
| 303 | RED_VLAN_303 | - |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

## VLANs Device Configuration

```eos
!
vlan 101
   name BLUE_VLAN_101
!
vlan 202
   name GREEN_VLAN_202
!
vlan 303
   name RED_VLAN_303
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | MLAG_PEER_s1-leaf3_Ethernet1 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 1 |
| Ethernet4 | S1-HOST2_Ethernet2 | *trunk | *none | *- | *- | 4 |
| Ethernet6 | MLAG_PEER_s1-leaf3_Ethernet6 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 1 |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet2 | P2P_LINK_TO_S1-SPINE1_Ethernet5 | routed | - | 10.1.2.25/31 | default | 9000 | false | - | - |
| Ethernet3 | P2P_LINK_TO_S1-SPINE2_Ethernet5 | routed | - | 10.1.2.27/31 | default | 9000 | false | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description MLAG_PEER_s1-leaf3_Ethernet1
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description P2P_LINK_TO_S1-SPINE1_Ethernet5
   no shutdown
   mtu 9000
   no switchport
   ip address 10.1.2.25/31
!
interface Ethernet3
   description P2P_LINK_TO_S1-SPINE2_Ethernet5
   no shutdown
   mtu 9000
   no switchport
   ip address 10.1.2.27/31
!
interface Ethernet4
   description S1-HOST2_Ethernet2
   no shutdown
   channel-group 4 mode active
!
interface Ethernet6
   description MLAG_PEER_s1-leaf3_Ethernet6
   no shutdown
   channel-group 1 mode active
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | MLAG_PEER_s1-leaf3_Po1 | switched | trunk | 2-4094 | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |
| Port-Channel4 | S1-HOST2_Po1 | switched | trunk | none | - | - | - | - | 4 | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description MLAG_PEER_s1-leaf3_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 2-4094
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
!
interface Port-Channel4
   description S1-HOST2_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan none
   switchport mode trunk
   mlag 4
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.1.3.4/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 10.1.4.3/32 |
| Loopback101 | BLUE_VTEP_DIAGNOSTICS | BLUE | 10.0.10.68/32 |
| Loopback102 | GREEN_VTEP_DIAGNOSTICS | GREEN | 10.0.10.132/32 |
| Loopback103 | RED_VTEP_DIAGNOSTICS | RED | 10.0.10.4/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |
| Loopback101 | BLUE_VTEP_DIAGNOSTICS | BLUE | - |
| Loopback102 | GREEN_VTEP_DIAGNOSTICS | GREEN | - |
| Loopback103 | RED_VTEP_DIAGNOSTICS | RED | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.1.3.4/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 10.1.4.3/32
!
interface Loopback101
   description BLUE_VTEP_DIAGNOSTICS
   no shutdown
   vrf BLUE
   ip address 10.0.10.68/32
!
interface Loopback102
   description GREEN_VTEP_DIAGNOSTICS
   no shutdown
   vrf GREEN
   ip address 10.0.10.132/32
!
interface Loopback103
   description RED_VTEP_DIAGNOSTICS
   no shutdown
   vrf RED
   ip address 10.0.10.4/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan101 | BLUE_VLAN_101 | BLUE | - | false |
| Vlan202 | GREEN_VLAN_202 | GREEN | - | false |
| Vlan303 | RED_VLAN_303 | RED | - | false |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 9000 | false |
| Vlan4094 | MLAG_PEER | default | 9000 | false |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan101 |  BLUE  |  -  |  10.100.1.1/24  |  -  |  -  |  -  |  -  |
| Vlan202 |  GREEN  |  -  |  10.100.2.1/24  |  -  |  -  |  -  |  -  |
| Vlan303 |  RED  |  -  |  10.100.3.1/24  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.1.5.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.1.6.5/31  |  -  |  -  |  -  |  -  |  -  |

### VLAN Interfaces Device Configuration

```eos
!
interface Vlan101
   description BLUE_VLAN_101
   no shutdown
   vrf BLUE
   ip address virtual 10.100.1.1/24
!
interface Vlan202
   description GREEN_VLAN_202
   no shutdown
   vrf GREEN
   ip address virtual 10.100.2.1/24
!
interface Vlan303
   description RED_VLAN_303
   no shutdown
   vrf RED
   ip address virtual 10.100.3.1/24
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 9000
   ip address 10.1.5.5/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9000
   no autostate
   ip address 10.1.6.5/31
```

## VXLAN Interface

### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

#### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 101 | 10101 | - | - |
| 202 | 10202 | - | - |
| 303 | 10303 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| BLUE | 501 | - |
| GREEN | 502 | - |
| RED | 503 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description s1-leaf4_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 101 vni 10101
   vxlan vlan 202 vni 10202
   vxlan vlan 303 vni 10303
   vxlan vrf BLUE vni 501
   vxlan vrf GREEN vni 502
   vxlan vrf RED vni 503
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:1c:73:00:00:34

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:34
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | true |
| BLUE | true |
| default | false |
| GREEN | true |
| RED | true |

### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf BLUE
ip routing vrf GREEN
ip routing vrf RED
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | false |
| BLUE | false |
| default | false |
| GREEN | false |
| RED | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| default | 0.0.0.0/0 | 192.168.0.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 192.168.0.1
```

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65112|  10.1.3.4 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65112 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- |
| 10.0.0.1 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 10.0.0.2 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - |
| 10.1.2.24 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |
| 10.1.2.26 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - |
| 10.1.5.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

### Router BGP VLAN Aware Bundles

| VLAN Aware Bundle | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute | VLANs |
| ----------------- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ | ----- |
| BLUE | 10.1.3.4:501 | 501:501 | - | - | learned | 101 |
| GREEN | 10.1.3.4:502 | 502:502 | - | - | learned | 202 |
| RED | 10.1.3.4:503 | 503:503 | - | - | learned | 303 |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| BLUE | 10.1.3.4:501 | connected |
| GREEN | 10.1.3.4:502 | connected |
| RED | 10.1.3.4:503 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65112
   router-id 10.1.3.4
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65112
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description s1-leaf3
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.0.0.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.0.1 remote-as 65101
   neighbor 10.0.0.1 description s1-spine1
   neighbor 10.0.0.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.0.0.2 remote-as 65101
   neighbor 10.0.0.2 description s1-spine2
   neighbor 10.1.2.24 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.1.2.24 remote-as 65101
   neighbor 10.1.2.24 description s1-spine1_Ethernet5
   neighbor 10.1.2.26 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.1.2.26 remote-as 65101
   neighbor 10.1.2.26 description s1-spine2_Ethernet5
   neighbor 10.1.5.4 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.1.5.4 description s1-leaf3
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan-aware-bundle BLUE
      rd 10.1.3.4:501
      route-target both 501:501
      redistribute learned
      vlan 101
   !
   vlan-aware-bundle GREEN
      rd 10.1.3.4:502
      route-target both 502:502
      redistribute learned
      vlan 202
   !
   vlan-aware-bundle RED
      rd 10.1.3.4:503
      route-target both 503:503
      redistribute learned
      vlan 303
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf BLUE
      rd 10.1.3.4:501
      route-target import evpn 501:501
      route-target export evpn 501:501
      router-id 10.1.3.4
      redistribute connected
   !
   vrf GREEN
      rd 10.1.3.4:502
      route-target import evpn 502:502
      route-target export evpn 502:502
      router-id 10.1.3.4
      redistribute connected
   !
   vrf RED
      rd 10.1.3.4:503
      route-target import evpn 503:503
      route-target export evpn 503:503
      router-id 10.1.3.4
      redistribute connected
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 300 | 300 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 300 min-rx 300 multiplier 3
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

### IP IGMP Snooping Device Configuration

```eos
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.1.3.0/24 eq 32 |
| 20 | permit 10.1.4.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.1.3.0/24 eq 32
   seq 20 permit 10.1.4.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY |

#### RM-MLAG-PEER-IN

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | set origin incomplete |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| BLUE | enabled |
| default | disabled |
| GREEN | enabled |
| RED | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance BLUE
!
vrf instance GREEN
!
vrf instance RED
```

# Virtual Source NAT

## Virtual Source NAT Summary

| Source NAT VRF | Source NAT IP Address |
| -------------- | --------------------- |
| BLUE | 10.0.10.68 |
| GREEN | 10.0.10.132 |
| RED | 10.0.10.4 |

## Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf BLUE address 10.0.10.68
ip address virtual source-nat vrf GREEN address 10.0.10.132
ip address virtual source-nat vrf RED address 10.0.10.4
```

# Quality Of Service

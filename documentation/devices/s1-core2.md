# s1-core2
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
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router BGP](#router-bgp)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Route-maps](#route-maps)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | oob_management | oob | default | 192.168.0.103/24 | 192.168.0.1 |

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
   ip address 192.168.0.103/24
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
| SITE1_CORE_LEAF1 | Vlan4094 | 10.1.11.0 | Port-Channel1 |

Dual primary detection is disabled.

## MLAG Device Configuration

```eos
!
mlag configuration
   domain-id SITE1_CORE_LEAF1
   local-interface Vlan4094
   peer-address 10.1.11.0
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

- Spanning Tree disabled for VLANs: **4094**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4094
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
| 701 | iBGP_VRF_BLUE | - |
| 702 | iBGP_VRF_GREEN | - |
| 703 | iBGP_VRF_RED | - |
| 801 | BGP_VRF_BLUE_CORE1 | - |
| 802 | BGP_VRF_GREEN_CORE1 | - |
| 803 | BGP_VRF_RED_CORE1 | - |
| 811 | BGP_VRF_BLUE_CORE2 | - |
| 812 | BGP_VRF_GREEN_CORE2 | - |
| 813 | BGP_VRF_RED_CORE2 | - |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

## VLANs Device Configuration

```eos
!
vlan 701
   name iBGP_VRF_BLUE
!
vlan 702
   name iBGP_VRF_GREEN
!
vlan 703
   name iBGP_VRF_RED
!
vlan 801
   name BGP_VRF_BLUE_CORE1
!
vlan 802
   name BGP_VRF_GREEN_CORE1
!
vlan 803
   name BGP_VRF_RED_CORE1
!
vlan 811
   name BGP_VRF_BLUE_CORE2
!
vlan 812
   name BGP_VRF_GREEN_CORE2
!
vlan 813
   name BGP_VRF_RED_CORE2
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
| Ethernet1 | MLAG_PEER_s1-core1_Ethernet1 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 1 |
| Ethernet2 | s1-brdr1_Ethernet5 | *trunk | *none | *- | *- | 2 |
| Ethernet3 | s1-brdr2_Ethernet5 | *trunk | *none | *- | *- | 2 |
| Ethernet4 |  P2P_LINK_TO_s2-core2_Ethernet4 | trunk | - | - | - | - |
| Ethernet6 | MLAG_PEER_s1-core1_Ethernet6 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 1 |

*Inherited from Port-Channel Interface

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description MLAG_PEER_s1-core1_Ethernet1
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description s1-brdr1_Ethernet5
   no shutdown
   channel-group 2 mode active
!
interface Ethernet3
   description s1-brdr2_Ethernet5
   no shutdown
   channel-group 2 mode active
!
interface Ethernet4
   description P2P_LINK_TO_s2-core2_Ethernet4
   no shutdown
   switchport mode trunk
   switchport
!
interface Ethernet6
   description MLAG_PEER_s1-core1_Ethernet6
   no shutdown
   channel-group 1 mode active
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | MLAG_PEER_s1-core1_Po1 | switched | trunk | 2-4094 | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |
| Port-Channel2 | S1-BRDR1-PO4 | switched | trunk | none | - | - | - | - | 2 | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description MLAG_PEER_s1-core1_Po1
   no shutdown
   switchport
   switchport trunk allowed vlan 2-4094
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
!
interface Port-Channel2
   description S1-BRDR1-PO4
   no shutdown
   switchport
   switchport trunk allowed vlan none
   switchport mode trunk
   mlag 2
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan701 | iBGP_FOR_VRF_BLUE | BLUE | - | false |
| Vlan702 | iBGP_FOR_VRF_GREEN | GREEN | - | false |
| Vlan703 | iBGP_FOR_VRF_RED | RED | - | false |
| Vlan811 | BGP_FOR_VRF_BLUE | BLUE | - | false |
| Vlan812 | BGP_FOR_VRF_GREEN | GREEN | - | false |
| Vlan813 | BGP_FOR_VRF_RED | RED | - | false |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 9000 | false |
| Vlan4094 | MLAG_PEER | default | 9000 | false |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan701 |  BLUE  |  10.1.17.1/30  |  -  |  -  |  -  |  -  |  -  |
| Vlan702 |  GREEN  |  10.1.18.1/30  |  -  |  -  |  -  |  -  |  -  |
| Vlan703 |  RED  |  10.1.19.1/30  |  -  |  -  |  -  |  -  |  -  |
| Vlan811 |  BLUE  |  10.4.1.2/29  |  -  |  -  |  -  |  -  |  -  |
| Vlan812 |  GREEN  |  10.4.2.2/29  |  -  |  -  |  -  |  -  |  -  |
| Vlan813 |  RED  |  10.4.3.2/29  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.1.10.1/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.1.11.1/31  |  -  |  -  |  -  |  -  |  -  |

### VLAN Interfaces Device Configuration

```eos
!
interface Vlan701
   description iBGP_FOR_VRF_BLUE
   no shutdown
   vrf BLUE
   ip address 10.1.17.1/30
!
interface Vlan702
   description iBGP_FOR_VRF_GREEN
   no shutdown
   vrf GREEN
   ip address 10.1.18.1/30
!
interface Vlan703
   description iBGP_FOR_VRF_RED
   no shutdown
   vrf RED
   ip address 10.1.19.1/30
!
interface Vlan811
   description BGP_FOR_VRF_BLUE
   no shutdown
   vrf BLUE
   ip address 10.4.1.2/29
!
interface Vlan812
   description BGP_FOR_VRF_GREEN
   no shutdown
   vrf GREEN
   ip address 10.4.2.2/29
!
interface Vlan813
   description BGP_FOR_VRF_RED
   no shutdown
   vrf RED
   ip address 10.4.3.2/29
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 9000
   ip address 10.1.10.1/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 9000
   no autostate
   ip address 10.1.11.1/31
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
| 65121|  10.1.8.2 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |

### Router BGP Peer Groups

#### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65121 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- |
| 10.1.10.0 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - |
| 10.1.17.2 | 65121 | BLUE | - | - | - | - | - | - |
| 10.4.1.1 | 65221 | BLUE | - | - | - | - | - | - |
| 10.4.1.3 | 65113 | BLUE | - | - | - | - | - | - |
| 10.4.1.4 | 65113 | BLUE | - | - | - | - | - | - |
| 10.1.18.2 | 65121 | GREEN | - | - | - | - | - | - |
| 10.4.2.1 | 65221 | GREEN | - | - | - | - | - | - |
| 10.4.2.3 | 65113 | GREEN | - | - | - | - | - | - |
| 10.4.2.4 | 65113 | GREEN | - | - | - | - | - | - |
| 10.1.19.2 | 65121 | RED | - | - | - | - | - | - |
| 10.4.3.1 | 65221 | RED | - | - | - | - | - | - |
| 10.4.3.3 | 65113 | RED | - | - | - | - | - | - |
| 10.4.3.4 | 65113 | RED | - | - | - | - | - | - |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| BLUE | 10.1.8.2:502 | connected<br>static |
| GREEN | 10.1.8.2:503 | connected<br>static |
| RED | 10.1.8.2:501 | connected<br>static |

### Router BGP Device Configuration

```eos
!
router bgp 65121
   router-id 10.1.8.2
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65121
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.1.10.0 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.1.10.0 description s1-core1
   !
   address-family ipv4
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf BLUE
      rd 10.1.8.2:502
      router-id 10.1.8.2
      neighbor 10.1.17.2 remote-as 65121
      neighbor 10.1.17.2 description iBGP_VRF_BLUE_s1-core1
      neighbor 10.4.1.1 remote-as 65221
      neighbor 10.4.1.1 description BGP_VRF_BLUE_s2-core2
      neighbor 10.4.1.3 remote-as 65113
      neighbor 10.4.1.3 description BGP_VRF_BLUE_s1-brdr1
      neighbor 10.4.1.4 remote-as 65113
      neighbor 10.4.1.4 description BGP_VRF_BLUE_s1-brdr2
      redistribute connected
      redistribute static
      !
      address-family ipv4
         neighbor 10.1.17.2 activate
         neighbor 10.4.1.1 activate
         neighbor 10.4.1.3 activate
         neighbor 10.4.1.4 activate
   !
   vrf GREEN
      rd 10.1.8.2:503
      router-id 10.1.8.2
      neighbor 10.1.18.2 remote-as 65121
      neighbor 10.1.18.2 description iBGP_VRF_GREEN_s1-core1
      neighbor 10.4.2.1 remote-as 65221
      neighbor 10.4.2.1 description BGP_VRF_GREEN_s2-core2
      neighbor 10.4.2.3 remote-as 65113
      neighbor 10.4.2.3 description BGP_VRF_GREEN_s1-brdr1
      neighbor 10.4.2.4 remote-as 65113
      neighbor 10.4.2.4 description BGP_VRF_GREEN_s1-brdr2
      redistribute connected
      redistribute static
      !
      address-family ipv4
         neighbor 10.1.18.2 activate
         neighbor 10.4.2.1 activate
         neighbor 10.4.2.3 activate
         neighbor 10.4.2.4 activate
   !
   vrf RED
      rd 10.1.8.2:501
      router-id 10.1.8.2
      neighbor 10.1.19.2 remote-as 65121
      neighbor 10.1.19.2 description iBGP_VRF_RED_s1-core1
      neighbor 10.4.3.1 remote-as 65221
      neighbor 10.4.3.1 description BGP_VRF_RED_s2-core2
      neighbor 10.4.3.3 remote-as 65113
      neighbor 10.4.3.3 description BGP_VRF_RED_s1-brdr1
      neighbor 10.4.3.4 remote-as 65113
      neighbor 10.4.3.4 description BGP_VRF_RED_s1-brdr2
      redistribute connected
      redistribute static
      !
      address-family ipv4
         neighbor 10.1.19.2 activate
         neighbor 10.4.3.1 activate
         neighbor 10.4.3.3 activate
         neighbor 10.4.3.4 activate
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

## Route-maps

### Route-maps Summary

#### RM-MLAG-PEER-IN

| Sequence | Type | Match and/or Set |
| -------- | ---- | ---------------- |
| 10 | permit | set origin incomplete |

### Route-maps Device Configuration

```eos
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

# Quality Of Service

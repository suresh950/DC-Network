# Below is the LAB architecture we are going to configure
![](https://github.com/suresh950/DC-Network/blob/main/VXLAN-Config/VXLAN-Lab-2/Image/2025-10-26_11h00_55.png "LAB")

# Steps
## 1. Step 1. Underlay Configuration

#### Configure the interface for all the switch Spine and Leaf, same config for all
##### Spine-1
```python
int e1/1-4
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.1/32
 no shut
 exit
```
##### Spine-2
```python
int e1/1-4
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.2/32
 no shut
 exit
```
##### Leaf-1
```python
int e1/1-2
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.3/32
 no shut
 exit
```
##### Leaf-2
```python
int e1/1-2
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.4/32
 no shut
 exit
```
##### Leaf-3
```python
int e1/1-2
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.5/32
 no shut
 exit
```
##### Leaf-4
```python
int e1/1-2
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.6/32
 no shut
 exit
```
#### Configure OSPF On all Switch Spine and Leaf

##### Spine-1
```python
feature ospf
router ospf UNDERLAY
 router-id 1.1.1.1
 log-adjacency-changes 
 exit
interface e1/1-4
 medium p2p 
 ip unnumbered loopback 0
 ip router ospf UNDERLAY area 0.0.0.0
 exit
interface loopback0
 ip router ospf UNDERLAY area 0.0.0.0

```
##### Spine-2
```python
feature ospf
router ospf UNDERLAY
 router-id 2.2.2.2
 log-adjacency-changes 
 exit
interface e1/1-4
 medium p2p 
 ip unnumbered loopback 0
 ip router ospf UNDERLAY area 0.0.0.0
 exit
interface loopback0
 ip router ospf UNDERLAY area 0.0.0.0
```
##### Leaf-1
```python
feature ospf
router ospf UNDERLAY
 router-id 3.3.3.3
 log-adjacency-changes 
 exit
interface e1/1-2
 medium p2p 
 ip unnumbered loopback 0
 ip router ospf UNDERLAY area 0.0.0.0
 exit
interface loopback0
 ip router ospf UNDERLAY area 0.0.0.0
```
##### Leaf-2
```python
feature ospf
router ospf UNDERLAY
 router-id 4.4.4.4
 log-adjacency-changes 
 exit
interface e1/1-2
 medium p2p 
 ip unnumbered loopback 0
 ip router ospf UNDERLAY area 0.0.0.0
 exit
interface loopback0
 ip router ospf UNDERLAY area 0.0.0.0
```
##### Leaf-3
```python
feature ospf
router ospf UNDERLAY
 router-id 5.5.5.5
 log-adjacency-changes 
 exit
interface e1/1-2
 medium p2p 
 ip unnumbered loopback 0
 ip router ospf UNDERLAY area 0.0.0.0
 exit
interface loopback0
 ip router ospf UNDERLAY area 0.0.0.0
```
##### Leaf-4
```python
feature ospf
router ospf UNDERLAY
 router-id 6.6.6.6
 log-adjacency-changes 
 exit
interface e1/1-2
 medium p2p 
 ip unnumbered loopback 0
 ip router ospf UNDERLAY area 0.0.0.0
 exit
interface loopback0
 ip router ospf UNDERLAY area 0.0.0.0
```

## 2 Step 2. Underlay OSPF Verification
##### All the switches
```python
SPine-1(config)# show ip ospf neig
 OSPF Process ID UNDERLAY VRF default
 Total number of neighbors: 4
 Neighbor ID     Pri State            Up Time  Address         Interface
 3.3.3.3           1 FULL/ -          00:01:07 192.168.1.3     Eth1/1 
 4.4.4.4           1 FULL/ -          00:00:58 192.168.1.4     Eth1/2 
 5.5.5.5           1 FULL/ -          00:00:50 192.168.1.5     Eth1/3 
 6.6.6.6           1 FULL/ -          00:00:44 192.168.1.6     Eth1/4 
SPine-1(config)#

Spine-2(config)# show ip ospf neig
 OSPF Process ID UNDERLAY VRF default
 Total number of neighbors: 4
 Neighbor ID     Pri State            Up Time  Address         Interface
 4.4.4.4           1 FULL/ -          00:00:57 192.168.1.4     Eth1/1 
 3.3.3.3           1 FULL/ -          00:01:04 192.168.1.3     Eth1/2 
 5.5.5.5           1 FULL/ -          00:00:53 192.168.1.5     Eth1/3 
 6.6.6.6           1 FULL/ -          00:00:42 192.168.1.6     Eth1/4 
Spine-2(config)#

Leaf-1(config)# show ip ospf neig
 OSPF Process ID UNDERLAY VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 1.1.1.1           1 FULL/ -          00:01:07 192.168.1.1     Eth1/1 
 2.2.2.2           1 FULL/ -          00:01:04 192.168.1.2     Eth1/2 
Leaf-1(config)#

Leaf-2(config)# 
Leaf-2(config)# show ip ospf neig
 OSPF Process ID UNDERLAY VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 2.2.2.2           1 FULL/ -          00:00:57 192.168.1.2     Eth1/1 
 1.1.1.1           1 FULL/ -          00:00:58 192.168.1.1     Eth1/2 
Leaf-2(config)#

Leaf-3(config)# show ip ospf neig
 OSPF Process ID UNDERLAY VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 1.1.1.1           1 FULL/ -          00:00:50 192.168.1.1     Eth1/1 
 2.2.2.2           1 FULL/ -          00:00:53 192.168.1.2     Eth1/2 
Leaf-3(config)#

Leaf-4(config)# show ip ospf neig
 OSPF Process ID UNDERLAY VRF default
 Total number of neighbors: 2
 Neighbor ID     Pri State            Up Time  Address         Interface
 2.2.2.2           1 FULL/ -          00:00:42 192.168.1.2     Eth1/1 
 1.1.1.1           1 FULL/ -          00:00:44 192.168.1.1     Eth1/2 
Leaf-4(config)# 
```
## 3. Step 3. Multicast Configuration (PIM)
##### Spine-1
```python
feature pim
int loopback 1
  ! # Same IP you need to configure on both the Spine 
 ip address 1.2.3.4/32
 no shut
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode 
 
int loo0
 ip pim sparse-mode
 exit
int e1/1-4
 ip pim sparse-mode
 exit

ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4 
ip pim ssm range 232.0.0.0/8
 ! anycast RP for redundancy purposes 
ip pim anycast-rp 1.2.3.4 192.168.1.1
ip pim anycast-rp 1.2.3.4 192.168.1.2

```
##### Spine-2
```python
feature pim
int loopback 1
  ! # Same IP you need to configure on both the Spine 
 ip address 1.2.3.4/32
 no shut
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode 
 
int loo0
 ip pim sparse-mode
 exit
int e1/1-4
 ip pim sparse-mode
 exit

ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4 
ip pim ssm range 232.0.0.0/8
 ! anycast RP for redundancy purposes 
ip pim anycast-rp 1.2.3.4 192.168.1.1
ip pim anycast-rp 1.2.3.4 192.168.1.2
```
##### Leafs (Leaf-1, Leaf-2, Leaf-3, Leaf-4)
##### Note: Exact Same Config for all the Leafs
```python

feature pim 
ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
int loopback 0
 ip pim sparse-mode 
int e1/1-2
 ip pim sparse-mode 

```
## (Quick Checks)
```python
SPine-1(config)# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD   
 ECMP Redirect
                                                         Priority Capable State 
    Capable
192.168.1.3     Ethernet1/1          00:04:58  00:01:36  1        yes     n/a   
  no
192.168.1.4     Ethernet1/2          00:01:32  00:01:41  1        yes     n/a   
  no
192.168.1.5     Ethernet1/3          00:01:26  00:01:18  1        yes     n/a   
  no
192.168.1.6     Ethernet1/4          00:01:24  00:01:21  1        yes     n/a   
  no
SPine-1(config)#
SPine-1(config)#     show ip pim rp
PIM RP Status Information for VRF "default"
BSR disabled
Auto-RP disabled
BSR RP Candidate policy: None
BSR RP policy: None
Auto-RP Announce policy: None
Auto-RP Discovery policy: None

Anycast-RP 1.2.3.4 members:
  192.168.1.1*  192.168.1.2  

RP: 1.2.3.4*, (0), 
 uptime: 00:10:37   priority: 255, 
 RP-source: (local),  
 group ranges:
 224.0.0.0/4   
SPine-1(config)# 
SPine-1(config)# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 00:21:24, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


SPine-1(config)# show ip igmp snooping groups
Type: S - Static, D - Dynamic, R - Router port, F - Fabricpath core port

Vlan  Group Address      Ver  Type  Port list
SPine-1(config)# show forwarding distribution multicast
Number of Multicast FIB Processes Active: 1
Slot     FIB State 
   1        ACTIVE 
SPine-1(config)#
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Spine-2(config)# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD   
 ECMP Redirect
                                                         Priority Capable State 
    Capable
192.168.1.4     Ethernet1/1          00:01:32  00:01:41  1        yes     n/a   
  no
192.168.1.3     Ethernet1/2          00:04:58  00:01:36  1        yes     n/a   
  no
192.168.1.5     Ethernet1/3          00:01:26  00:01:20  1        yes     n/a   
  no
192.168.1.6     Ethernet1/4          00:01:24  00:01:20  1        yes     n/a   
  no
Spine-2(config)#

Spine-2(config)# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 00:21:26, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


Spine-2(config)# show ip igmp snooping groups
Type: S - Static, D - Dynamic, R - Router port, F - Fabricpath core port

Vlan  Group Address      Ver  Type  Port list
Spine-2(config)# show forwarding distribution multicast
Number of Multicast FIB Processes Active: 1
Slot     FIB State 
   1        ACTIVE 
Spine-2(config)#
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-1(config)# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD   
 ECMP Redirect
                                                         Priority Capable State 
    Capable
192.168.1.1     Ethernet1/1          00:04:58  00:01:39  1        yes     n/a   
  no
192.168.1.2     Ethernet1/2          00:04:58  00:01:40  1        yes     n/a   
  no
Leaf-1(config)#
Leaf-1(config)# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 00:21:25, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


Leaf-1(config)# show ip igmp snooping groups
Type: S - Static, D - Dynamic, R - Router port, F - Fabricpath core port

Vlan  Group Address      Ver  Type  Port list
Leaf-1(config)# show forwarding distribution multicast
Number of Multicast FIB Processes Active: 1
Slot     FIB State 
   1        ACTIVE 
Leaf-1(config)#
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-2(config-if-range)# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD   
 ECMP Redirect
                                                         Priority Capable State 
    Capable
192.168.1.2     Ethernet1/1          00:01:32  00:01:41  1        yes     n/a   
  no
192.168.1.1     Ethernet1/2          00:01:32  00:01:36  1        yes     n/a   
  no
Leaf-2(config-if-range)#
Leaf-2(config-if-range)# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 00:21:25, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


Leaf-2(config-if-range)# show ip igmp snooping groups
Type: S - Static, D - Dynamic, R - Router port, F - Fabricpath core port

Vlan  Group Address      Ver  Type  Port list
Leaf-2(config-if-range)# show forwarding distribution multicast
Number of Multicast FIB Processes Active: 1
Slot     FIB State 
   1        ACTIVE 
Leaf-2(config-if-range)#
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-3(config-if-range)# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD   
 ECMP Redirect
                                                         Priority Capable State 
    Capable
192.168.1.1     Ethernet1/1          00:01:26  00:01:29  1        yes     n/a   
  no
192.168.1.2     Ethernet1/2          00:01:26  00:01:32  1        yes     n/a   
  no
Leaf-3(config-if-range)#
Leaf-3(config-if-range)# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 00:21:26, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


Leaf-3(config-if-range)# show ip igmp snooping groups
Type: S - Static, D - Dynamic, R - Router port, F - Fabricpath core port

Vlan  Group Address      Ver  Type  Port list
Leaf-3(config-if-range)# show forwarding distribution multicast
Number of Multicast FIB Processes Active: 1
Slot     FIB State 
   1        ACTIVE 
Leaf-3(config-if-range)#
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-4# show ip pim neighbor
PIM Neighbor Status for VRF "default"
Neighbor        Interface            Uptime    Expires   DR       Bidir-  BFD   
 ECMP Redirect
                                                         Priority Capable State 
    Capable
192.168.1.2     Ethernet1/1          00:01:24  00:01:21  1        yes     n/a   
  no
192.168.1.1     Ethernet1/2          00:01:24  00:01:42  1        yes     n/a   
  no
Leaf-4#
Leaf-4# show ip mroute
IP Multicast Routing Table for VRF "default"

(*, 232.0.0.0/8), uptime: 00:21:26, pim ip 
  Incoming interface: Null, RPF nbr: 0.0.0.0
  Outgoing interface list: (count: 0)


Leaf-4# show ip igmp snooping groups
Type: S - Static, D - Dynamic, R - Router port, F - Fabricpath core port

Vlan  Group Address      Ver  Type  Port list
Leaf-4# show forwarding distribution multicast
Number of Multicast FIB Processes Active: 1
Slot     FIB State 
   1        ACTIVE 
Leaf-4#
```
## 4. Step 4. Overlay BGP Configuration
##### Spine-1
```Python
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

router bgp 64520
 log-neighbor-changes 
 router-id 1.1.1.1
 address-family ipv4 unicast 
  exit
 address-family l2vpn evpn 
 retain route-target all 
  exit 
 template peer VXLAN-FEAF
  remote-as 64520
  update-source loopback 0
  address-family ipv4 unicast 
  send-community 
  send-community Extended 
  soft-reconfiguration inbound 
  exit
 address-family l2vpn evpn 
  send-community 
  send-community extended
  route-reflector-client 
   exit
  exit
neighbor 192.168.1.3
 inherit peer VXLAN-FEAF
 exit
neighbor 192.168.1.4
 inherit peer VXLAN-FEAF
 exit
neighbor 192.168.1.5
 inherit peer VXLAN-FEAF
 exit
neighbor 192.168.1.6
 inherit peer VXLAN-FEAF
 exit

```
##### Spine-2
```Python
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

router bgp 64520
 log-neighbor-changes 
 router-id 2.2.2.2
 address-family ipv4 unicast 
  exit
 address-family l2vpn evpn 
 retain route-target all 
  exit 
 template peer VXLAN-FEAF
  remote-as 64520
  update-source loopback 0
  address-family ipv4 unicast 
  send-community 
  send-community Extended 
  soft-reconfiguration inbound 
  exit
 address-family l2vpn evpn 
  send-community 
  send-community extended
  route-reflector-client 
   exit
  exit
neighbor 192.168.1.3
 inherit peer VXLAN-FEAF
 exit
neighbor 192.168.1.4
 inherit peer VXLAN-FEAF
 exit
neighbor 192.168.1.5
 inherit peer VXLAN-FEAF
 exit
neighbor 192.168.1.6
 inherit peer VXLAN-FEAF
 exit
```
##### Leaf-1
```Python
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

router bgp 64520
 log-neighbor-changes 
 router-id 3.3.3.3
 address-family ipv4 unicast 
  exit
 address-family l2vpn evpn 
  exit 
 template peer VXLAN-SPINE
  remote-as 64520
  update-source loopback 0
  address-family ipv4 unicast 
  send-community 
  send-community Extended 
  soft-reconfiguration inbound 
  exit
 address-family l2vpn evpn 
  send-community 
  send-community extended
   exit
  exit
neighbor 192.168.1.1
 inherit peer VXLAN-SPINE
 exit
neighbor 192.168.1.2
 inherit peer VXLAN-SPINE
 exit
```
##### Leaf-2
```Python
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

router bgp 64520
 log-neighbor-changes 
 router-id 4.4.4.4
 address-family ipv4 unicast 
  exit
 address-family l2vpn evpn 
  exit 
 template peer VXLAN-SPINE
  remote-as 64520
  update-source loopback 0
  address-family ipv4 unicast 
  send-community 
  send-community Extended 
  soft-reconfiguration inbound 
  exit
 address-family l2vpn evpn 
  send-community 
  send-community extended
   exit
  exit
neighbor 192.168.1.1
 inherit peer VXLAN-SPINE
 exit
neighbor 192.168.1.2
 inherit peer VXLAN-SPINE
 exit
```
##### Leaf-3
```Python
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

router bgp 64520
 log-neighbor-changes 
 router-id 5.5.5.5
 address-family ipv4 unicast 
  exit
 address-family l2vpn evpn 
  exit 
 template peer VXLAN-SPINE
  remote-as 64520
  update-source loopback 0
  address-family ipv4 unicast 
  send-community 
  send-community Extended 
  soft-reconfiguration inbound 
  exit
 address-family l2vpn evpn 
  send-community 
  send-community extended
   exit
  exit
neighbor 192.168.1.1
 inherit peer VXLAN-SPINE
 exit
neighbor 192.168.1.2
 inherit peer VXLAN-SPINE
 exit
```
##### Leaf-4
```Python
feature bgp
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay
nv overlay evpn

router bgp 64520
 log-neighbor-changes 
 router-id 6.6.6.6
 address-family ipv4 unicast 
  exit
 address-family l2vpn evpn 
  exit 
 template peer VXLAN-SPINE
  remote-as 64520
  update-source loopback 0
  address-family ipv4 unicast 
  send-community 
  send-community Extended 
  soft-reconfiguration inbound 
  exit
 address-family l2vpn evpn 
  send-community 
  send-community extended
   exit
  exit
neighbor 192.168.1.1
 inherit peer VXLAN-SPINE
 exit
neighbor 192.168.1.2
 inherit peer VXLAN-SPINE
 exit
```
## (Quick BGP Checks)
##### For all the Switch (Spine, Leaf)
```python
SPine-1(config-if)# show ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 1.1.1.1, local AS number 64520
BGP table version is 6, IPv4 Unicast config peers 4, capable peers 4
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.3     4 64520      20      20        6    0    0 00:11:20 0         
192.168.1.4     4 64520      20      20        6    0    0 00:11:17 0         
192.168.1.5     4 64520      20      20        6    0    0 00:11:43 0         
192.168.1.6     4 64520      20      20        6    0    0 00:11:32 0         

SPine-1(config-if)# 
SPine-1(config-if)#
SPine-1(config-if)# show bgp l2vpn evpn summ
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 1.1.1.1, local AS number 64520
BGP table version is 6, L2VPN EVPN config peers 4, capable peers 4
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.3     4 64520      26      26        6    0    0 00:17:28 0         
192.168.1.4     4 64520      26      26        6    0    0 00:17:25 0         
192.168.1.5     4 64520      26      26        6    0    0 00:17:51 0         
192.168.1.6     4 64520      26      26        6    0    0 00:17:40 0         
SPine-1(config-if)# 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Spine-2(config-if)# show ip bgp summary 
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 2.2.2.2, local AS number 64520
BGP table version is 6, IPv4 Unicast config peers 4, capable peers 4
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.3     4 64520      23      23        6    0    0 00:14:11 0         
192.168.1.4     4 64520      22      22        6    0    0 00:13:26 0         
192.168.1.5     4 64520      23      23        6    0    0 00:14:13 0         
192.168.1.6     4 64520      23      23        6    0    0 00:14:02 0         
Spine-2(config-if)#
Spine-2(config-if)# show bgp l2vpn evpn summ
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 2.2.2.2, local AS number 64520
BGP table version is 6, L2VPN EVPN config peers 4, capable peers 4
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.3     4 64520      26      26        6    0    0 00:17:32 0         
192.168.1.4     4 64520      25      25        6    0    0 00:16:47 0         
192.168.1.5     4 64520      26      26        6    0    0 00:17:34 0         
192.168.1.6     4 64520      26      26        6    0    0 00:17:23 0         
Spine-2(config-if)# 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-1(config-if)# show ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 3.3.3.3, local AS number 64520
BGP table version is 4, IPv4 Unicast config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      20      20        4    0    0 00:11:20 0         
192.168.1.2     4 64520      20      20        4    0    0 00:11:24 0         
Leaf-1(config-if)# 
Leaf-1(config-if)#
Leaf-1(config-if)# show bgp l2vpn evpn summ
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 3.3.3.3, local AS number 64520
BGP table version is 4, L2VPN EVPN config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      26      26        4    0    0 00:17:28 0         
192.168.1.2     4 64520      26      26        4    0    0 00:17:32 0         
Leaf-1(config-if)# 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-2(config-if)# show ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 4.4.4.4, local AS number 64520
BGP table version is 4, IPv4 Unicast config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      20      20        4    0    0 00:11:17 0         
192.168.1.2     4 64520      19      19        4    0    0 00:10:39 0         
Leaf-2(config-if)# 
Leaf-2(config-if)#
Leaf-1(config-if)# show bgp l2vpn evpn summ
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 3.3.3.3, local AS number 64520
BGP table version is 4, L2VPN EVPN config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      26      26        4    0    0 00:17:28 0         
192.168.1.2     4 64520      26      26        4    0    0 00:17:32 0         
Leaf-1(config-if)# 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-3(config-if)# 
Leaf-3(config-if)# show ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 5.5.5.5, local AS number 64520
BGP table version is 4, IPv4 Unicast config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      20      20        4    0    0 00:11:43 0         
192.168.1.2     4 64520      20      20        4    0    0 00:11:26 0         
Leaf-3(config-if)# 
Leaf-3(config-if)#
Leaf-3(config-if)# show bgp l2vpn evpn summ
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 5.5.5.5, local AS number 64520
BGP table version is 4, L2VPN EVPN config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      26      26        4    0    0 00:17:51 0         
192.168.1.2     4 64520      26      26        4    0    0 00:17:34 0         
Leaf-3(config-if)# 
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
Leaf-4(config-if)# show ip bgp summary
BGP summary information for VRF default, address family IPv4 Unicast
BGP router identifier 6.6.6.6, local AS number 64520
BGP table version is 4, IPv4 Unicast config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      20      20        4    0    0 00:11:32 0         
192.168.1.2     4 64520      20      20        4    0    0 00:11:15 0         
Leaf-4(config-if)# 
Leaf-4(config-if)#
Leaf-4(config-if)# show bgp l2vpn evpn summ
BGP summary information for VRF default, address family L2VPN EVPN
BGP router identifier 6.6.6.6, local AS number 64520
BGP table version is 4, L2VPN EVPN config peers 2, capable peers 2
0 network entries and 0 paths using 0 bytes of memory
BGP attribute entries [0/0], BGP AS path entries [0/0]
BGP community entries [0/0], BGP clusterlist entries [0/0]

Neighbor        V    AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
192.168.1.1     4 64520      26      26        4    0    0 00:17:40 0         
192.168.1.2     4 64520      26      26        4    0    0 00:17:23 0         
Leaf-4(config-if)# 
```
## 5. VLAN/VNI/NVE Configuration on Leafs
##### Same Configuration on all the Leafs (Leaf-1, Leaf2, Leaf3, Leaf4))
```Python
! # Provide some memory on TCAM
hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide
! # Configure MAC to be used by Gateways
fabric forwarding anycast-gateway-mac 0000.0011.1234
vlan 10
 vn-segment 100010
int vlan 10
 ip address 10.10.10.254/24
 no shut
 fabric forwarding mode anycast-gateway
vlan 20
 vn-segment 100020
int vlan 20
 ip address 10.10.20.254/24
 no shut
 fabric forwarding mode anycast-gateway
vlan 30
 vn-segment 100030
int vlan 30
 ip address 10.10.30.254/24
 no shut
 fabric forwarding mode anycast-gateway
```
## 6. Configure the VTEP interface 
##### Same Configuration on all the Leafs (Leaf-1, Leaf2, Leaf3, Leaf4))
```Python

int nve 1
no shutdown
 host-reachability protocol bgp 
 source-interface loopback 0
 member vni 100010
  suppress-arp 
  mcast-group 224.1.1.192
 member vni 100020
  suppress-arp 
  mcast-group 224.1.1.192
 member vni 100030
  suppress-arp 
  mcast-group 224.1.1.192

evpn 
 vni 100010 l2 
  rd auto 
  route-target import auto 
  route-target export auto
 vni 100020 l2 
  rd auto 
  route-target import auto 
  route-target export auto
 vni 100030 l2 
  rd auto 
  route-target import auto 
  route-target export auto 

```
## 7. Configure the interface in vlan 10, vlan 20, and vlan 30 for all the Leafs
```Python
interface e1/5
 switchport
 switchport mode access
 switchport access vlan 10
interface e1/6
 switchport
 switchport mode access
 switchport access vlan 20
interface e1/7
 switchport
 switchport mode access
 switchport access vlan 30
```
## 8. Validation

```python
! Now all the VLANs will be able to communicate with in self vlan across the Leafs
! Till now, we have not established the inter-VLAN communication 
````
## 9. Configuration for inter-VLAN communication 
#### Create L3 VNI
```python
Vlan 999
 vn-segment 100999
 exit
````
#### Create L3 VRF
```python
vrf context T1
 vni 100999
 rd auto 
  address-family ipv4 unicast 
  route-target both auto 
  route-target both auto evpn 
 exit
````
#### Create L3 interface for Vlan 999
```python
interface vlan 999
 vrf member T1
 ip forward
 no shut
````
#### Associate the New vni to nve 1
```python
interface nve 1
   member vni 100999 associate-vrf 
````
#### Configure BGP for VRF T1
```python
router bgp 64520
  vrf T1
  log-neighbor-changes 
  address-family ipv4 unicast 
    network 10.10.10.0/24
    network 10.10.20.0/24
    network 10.10.30.0/24
````
#### Associate all the vlan interfaces to the VRF
```python
int vlan 10
 vrf member T1
 ip address 10.10.10.254/24
 fabric forwarding mode anycast-gateway 
 no shut
int vlan 20
 vrf member T1
 ip address 10.10.20.254/24
 fabric forwarding mode anycast-gateway 
 no shut 
int vlan 30
 vrf member T1
 ip address 10.10.30.254/24
 fabric forwarding mode anycast-gateway 
 no shut
````

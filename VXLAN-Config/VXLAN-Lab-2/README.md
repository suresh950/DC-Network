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
## 4. VLANs & VNIs
## 5. AnyCast G/W
## 6. EVPN
## 7. VxLAN
## 8. Validation

### Spine-1 Configuration NXOS-9.3.3
 
```Python
 
Spine-1# show running-config 

!Command: show running-config
!Running configuration last done at: Fri Oct 24 13:54:06 2025
!Time: Sat Oct 25 01:02:16 2025

version 9.3(3) Bios:version  
hostname Spine-1
 

nv overlay evpn
feature ospf
feature bgp
feature pim
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay



ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
ip pim anycast-rp 1.2.3.4 192.168.1.1
ip pim anycast-rp 1.2.3.4 192.168.1.2


interface Vlan1

interface Ethernet1/1
  no switchport
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf UNDERLAY area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/2
  no switchport
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf UNDERLAY area 0.0.0.0
  ip pim sparse-mode
  no shutdown

interface Ethernet1/3
  no switchport
  mtu 9216
  medium p2p
  ip unnumbered loopback0
  ip router ospf UNDERLAY area 0.0.0.0
  ip pim sparse-mode
  no shutdown



interface loopback0
  ip address 192.168.1.1/32
  ip router ospf UNDERLAY area 0.0.0.0
  ip pim sparse-mode

interface loopback1
  ip address 1.2.3.4/32
  ip router ospf UNDERLAY area 0.0.0.0
  ip pim sparse-mode
line console
line vty
router ospf UNDERLAY
  router-id 1.1.1.1
  log-adjacency-changes
router bgp 64520
  router-id 1.1.1.1
  log-neighbor-changes
  address-family ipv4 unicast
  address-family l2vpn evpn
    retain route-target all
  template peer VXLAN-LEAF
    remote-as 64520
    update-source loopback0
    address-family ipv4 unicast
      send-community
      send-community extended
      soft-reconfiguration inbound
    address-family l2vpn evpn
      send-community
      send-community extended
      route-reflector-client
  neighbor 192.168.1.3
    inherit peer VXLAN-LEAF
  neighbor 192.168.1.4
    inherit peer VXLAN-LEAF
  neighbor 192.168.1.5
    inherit peer VXLAN-LEAF

```

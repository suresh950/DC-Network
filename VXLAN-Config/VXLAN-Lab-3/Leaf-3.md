### Leaf-1 Configuration NXOS-9.3.3

```python
Leaf-3# 
Leaf-3# show running-config 

!Command: show running-config
!Running configuration last done at: Sat Oct 25 00:59:08 2025
!Time: Sat Oct 25 01:13:56 2025

version 9.3(3) Bios:version  
hostname Leaf-3


nv overlay evpn
feature ospf
feature bgp
feature pim
feature fabric forwarding
feature interface-vlan
feature vn-segment-vlan-based
feature nv overlay


advertise evpn multicast


fabric forwarding anycast-gateway-mac 0000.0011.1234
ip pim rp-address 1.2.3.4 group-list 224.0.0.0/4
ip pim ssm range 232.0.0.0/8
vlan 1,10,20,999
vlan 10
  vn-segment 100010
vlan 20
  vn-segment 100020
vlan 999
  vn-segment 100999

vrf context T1
  vni 100999
  rd auto
  address-family ipv4 unicast
    route-target both auto
    route-target both auto evpn
vrf context management
hardware access-list tcam region racl 512
hardware access-list tcam region arp-ether 256 double-wide



interface Vlan10
  no shutdown
  vrf member T1
  ip address 10.10.1.254/24
  fabric forwarding mode anycast-gateway

interface Vlan20
  no shutdown
  vrf member T1
  ip address 10.10.2.254/24
  fabric forwarding mode anycast-gateway

interface Vlan999
  no shutdown
  vrf member T1
  ip forward

interface nve1
  no shutdown
  host-reachability protocol bgp
  source-interface loopback0
  member vni 100010
    suppress-arp
    mcast-group 224.1.1.192
  member vni 100020
    suppress-arp
    mcast-group 224.1.1.192
  member vni 100999 associate-vrf

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



interface Ethernet1/6
  switchport access vlan 10

interface Ethernet1/7
  switchport access vlan 20



interface loopback0
  ip address 192.168.1.5/32
  ip router ospf UNDERLAY area 0.0.0.0
  ip pim sparse-mode
line console
line vty
router ospf UNDERLAY
  router-id 5.5.5.5
  log-adjacency-changes
router bgp 64520
  router-id 5.5.5.5
  log-neighbor-changes
  address-family ipv4 unicast
  address-family l2vpn evpn
  template peer VXLAN-SPINE
    remote-as 64520
    update-source loopback0
    address-family ipv4 unicast
      send-community
      send-community extended
      soft-reconfiguration inbound
    address-family l2vpn evpn
      send-community
      send-community extended
  neighbor 192.168.1.1
    inherit peer VXLAN-SPINE
  neighbor 192.168.1.2
    inherit peer VXLAN-SPINE
  vrf T1
    log-neighbor-changes
    address-family ipv4 unicast
      network 10.10.1.0/24
      network 10.10.2.0/24
evpn
  vni 100010 l2
    rd auto
    route-target import auto
    route-target export auto
  vni 100020 l2
    rd auto
    route-target import auto
    route-target export auto

```

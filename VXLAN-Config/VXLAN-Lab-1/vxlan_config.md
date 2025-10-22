VXLAN Lab-1
 
Overlay Configuration Step 4:BGP EVPN
==========================================

Spine1
======
```Python
conf t
hostname Spine1

interface eth1/1
 no switchport
 ip address 10.1.11.1/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface eth1/2
 no switchport
 ip address 10.1.12.1/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface loopback 0 
 ip address 1.1.1.1/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode
interface loopback 1 
 ip address 100.100.100.100/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 1.1.1.1

feature pim

ip pim rp-address 100.100.100.100
ip pim anycast-rp 100.100.100.100 1.1.1.1
ip pim anycast-rp 100.100.100.100 2.2.2.2

feature bgp

router bgp 65000
 router-id 1.1.1.1
 address-family ipv4 unicast
 address-family l2vpn evpn
  retain route-target all
 template peer LEAF
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
   route-reflector-client
  address-family l2vpn evpn
   send-community
   send-community extended
   route-reflector-client
  neighbor 11.11.11.11
   inherit peer LEAF 
  neighbor 12.12.12.12
   inherit peer LEAF 


feature nv overlay
feature vn-segment-vlan-based
nv overlay evpn

```
Spine2
======
```Python
conf t
hostname Spine2

interface eth1/1
 no switchport
 ip address 10.2.11.2/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface eth1/2
 no switchport
 ip address 10.2.12.2/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface loopback 0 
 ip address 2.2.2.2/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode
interface loopback 1 
 ip address 100.100.100.100/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 2.2.2.2

feature pim 

ip pim rp-address 100.100.100.100
ip pim anycast-rp 100.100.100.100 1.1.1.1
ip pim anycast-rp 100.100.100.100 2.2.2.2

feature bgp

router bgp 65000
 router-id 2.2.2.2
 address-family ipv4 unicast
 address-family l2vpn evpn
  retain route-target all
 template peer LEAF
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
   route-reflector-client
 address-family l2vpn evpn
   send-community
   send-community extended
   route-reflector-client
  neighbor 11.11.11.11
   inherit peer LEAF 
  neighbor 12.12.12.12
   inherit peer LEAF 

feature nv overlay
feature vn-segment-vlan-based
nv overlay evpn

```
Leaf1
======
```Python
conf t
hostname Leaf1

interface eth1/1
 no switchport
 ip address 10.1.11.11/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface eth1/2
 no switchport
 ip address 10.2.11.11/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface eth1/3
 switchport
 switchport mode access
 switchport access vlan 2
 no shutdown
interface eth1/4
 switchport
 switchport mode access
 switchport access vlan 3
 no shutdown
interface loopback 0 
 ip address 11.11.11.11/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode
interface loopback 1 
 ip address 172.16.0.11/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 11.11.11.11

feature pim 

ip pim rp-address 100.100.100.100

feature bgp

router bgp 65000
 router-id 11.11.11.11
 address-family ipv4 unicast
 address-family l2vpn evpn
 neighbor 1.1.1.1 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
 address-family l2vpn evpn
   send-community
   send-community extended
 neighbor 2.2.2.2 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
 address-family l2vpn evpn
   send-community
   send-community extended

feature nv overlay
feature vn-segment-vlan-based
nv overlay evpn

Vlan 2
 name T1-V2
 vn-segment 1002

Vlan 3
 name T1-V3
 vn-segment 1003

Vlan 1000
 name T1-L3VNI
 vn-segment 1000

vrf context T1
 vni 1000
 rd auto
 address-family ipv4 unicast
  route-target both auto
  route-target both auto evpn

feature interface-vlan 

interface vlan 2
 vrf member T1
 ip address 192.168.2.1/24
 no shutdown
 fabric forwarding mode anycast-gateway

interface vlan 3
 vrf member T1
 ip address 192.168.3.1/24
 no shutdown
 fabric forwarding mode anycast-gateway

interface vlan 1000
 no shutdown
 vrf member T1
 ip forward

fabric forwarding anycast-gateway-mac 0000.1111.2222

interface nve1
 source-interface loopback1
 host-reachability protocol bgp
 no shutdown
 member vni 1002
  mcast-group 239.0.0.2
 member vni 1003
  mcast-group 239.0.0.3
 member vni 1000 associate-vrf
```
Leaf2
======
```Python
conf t
hostname Leaf2

interface eth1/1
 no switchport
 ip address 10.1.12.12/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface eth1/2
 no switchport
 ip address 10.2.12.12/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
 ip pim sparse-mode
interface eth1/3
 switchport
 switchport mode access
 switchport access vlan 2
 no shutdown
interface eth1/4
 switchport
 switchport mode access
 switchport access vlan 3
 no shutdown
interface loopback 0 
 ip address 12.12.12.12/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode
interface loopback 1 
 ip address 172.16.0.12/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 12.12.12.12

feature pim

ip pim rp-address 100.100.100.100

feature bgp 

router bgp 65000
 router-id 12.12.12.12
 address-family ipv4 unicast
 address-family l2vpn evpn
 neighbor 1.1.1.1 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
 address-family l2vpn evpn
   send-community
   send-community extended
 neighbor 2.2.2.2 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
 address-family l2vpn evpn
   send-community
   send-community extended

feature nv overlay
feature vn-segment-vlan-based
nv overlay evpn

Vlan 2
 name T1-V2
 vn-segment 1002

Vlan 3
 name T1-V3
 vn-segment 1003

Vlan 1000
 name T1-L3VNI
 vn-segment 1000

vrf context T1
 vni 1000
 rd auto
 address-family ipv4 unicast
  route-target both auto
  route-target both auto evpn

feature interface-vlan 

interface vlan 2
 vrf member T1
 ip address 192.168.2.1/24
 no shutdown
 fabric forwarding mode anycast-gateway

interface vlan 3
 vrf member T1
 ip address 192.168.3.1/24
 no shutdown
 fabric forwarding mode anycast-gateway

interface vlan 1000
 no shutdown
 vrf member T1
 ip forward

fabric forwarding anycast-gateway-mac 0000.1111.2222

interface nve1
 source-interface loopback1
 host-reachability protocol bgp
 no shutdown
 member vni 1002
  mcast-group 239.0.0.2
 member vni 1003
  mcast-group 239.0.0.3
 member vni 1000 associate-vrf

evpn
 vni 1002 l2
  rd auto
  route-target import auto
  route-target export auto
 vni 1003 l2
  rd auto
  route-target import auto
  route-target export auto
```


VXLAN Lab-1
============================================ 
Underlay Configuration Step 1:IP Addressing
============================================
Spine1
======
```python
conf t
hostname Spine1

interface eth1/1
 no switchport
 ip address 10.1.11.1/24
 no shutdown
interface eth1/2
 no switchport
 ip address 10.1.12.1/24
 no shutdown
interface loopback 0 
 ip address 1.1.1.1/32
```

Spine2
======
```python
conf t
hostname Spine2

interface eth1/1
 no switchport
 ip address 10.2.11.2/24
 no shutdown
interface eth1/2
 no switchport
 ip address 10.2.12.2/24
 no shutdown
interface loopback 0 
 ip address 2.2.2.2/32
```

Leaf1
======
```python
conf t
hostname Leaf1

interface eth1/1
 no switchport
 ip address 10.1.11.11/24
 no shutdown
interface eth1/2
 no switchport
 ip address 10.2.12.11/24
 no shutdown
interface loopback 0 
 ip address 11.11.11.11/32
```

Leaf2
======
```python
conf t
hostname Leaf2

interface eth1/1
 no switchport
 ip address 10.2.11.12/24
 no shutdown
interface eth1/2
 no switchport
 ip address 10.1.12.12/24
 no shutdown
interface loopback 0 
 ip address 12.12.12.12/32
```


Underlay Configuration Step 2:MTU
=================================
Spine1
======
```python
conf t
hostname Spine1

interface eth1/1
 no switchport
 ip address 10.1.11.1/24
 no shutdown
 mtu 9216
interface eth1/2
 no switchport
 ip address 10.1.12.1/24
 no shutdown
 mtu 9216
interface loopback 0 
 ip address 1.1.1.1/32
```

Spine2
======
```python
conf t
hostname Spine2

interface eth1/1
 no switchport
 ip address 10.2.11.2/24
 no shutdown
 mtu 9216
interface eth1/2
 no switchport
 ip address 10.2.12.2/24
 no shutdown
 mtu 9216
interface loopback 0 
 ip address 2.2.2.2/32
```

Leaf1
======
```python
conf t
hostname Leaf1

interface eth1/1
 no switchport
 ip address 10.1.11.11/24
 no shutdown
 mtu 9216
interface eth1/2
 no switchport
 ip address 10.2.11.11/24
 no shutdown
 mtu 9216
interface loopback 0 
 ip address 11.11.11.11/32
```

Leaf2
======
```python
conf t
hostname Leaf2

interface eth1/1
 no switchport
 ip address 10.1.12.12/24
 no shutdown
 mtu 9216
interface eth1/2
 no switchport
 ip address 10.2.12.12/24
 no shutdown
 mtu 9216
interface loopback 0 
 ip address 12.12.12.12/32
```

Underlay Configuration Step 3:Routing Protocol
==============================================
Spine1
======
```python
conf t
hostname Spine1

interface eth1/1
 no switchport
 ip address 10.1.11.1/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface eth1/2
 no switchport
 ip address 10.1.12.1/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface loopback 0 
 ip address 1.1.1.1/32
 ip router ospf UNDERLAY area 0.0.0.0

feature ospf 

router ospf UNDERLAY
 router-id 1.1.1.1
  
```
Spine2
======
```python
conf t
hostname Spine2

interface eth1/1
 no switchport
 ip address 10.2.11.2/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface eth1/2
 no switchport
 ip address 10.2.12.2/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface loopback 0 
 ip address 2.2.2.2/32
 ip router ospf UNDERLAY area 0.0.0.0

feature ospf 

router ospf UNDERLAY
 router-id 2.2.2.2

```
Leaf1
======
```python
conf t
hostname Leaf1

interface eth1/1
 no switchport
 ip address 10.1.11.11/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface eth1/2
 no switchport
 ip address 10.2.11.11/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface loopback 0 
 ip address 11.11.11.11/32
 ip router ospf UNDERLAY area 0.0.0.0

feature ospf 

router ospf UNDERLAY
 router-id 11.11.11.11

```
Leaf2
======
```python
conf t
hostname Leaf2

interface eth1/1
 no switchport
 ip address 10.1.12.12/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface eth1/2
 no switchport
 ip address 10.2.12.12/24
 no shutdown
 mtu 9216
 ip router ospf UNDERLAY area 0.0.0.0
 ip ospf network point-to-point
interface loopback 0 
 ip address 12.12.12.12/32
 ip router ospf UNDERLAY area 0.0.0.0

feature ospf 

router ospf UNDERLAY
 router-id 12.12.12.12
```

Underlay Configuration Step 4:Multi Destination Traffic
=======================================================
Spine1
======
```python
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
```

Spine2
======
```python
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
```
Leaf1
======
```python
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
interface loopback 0 
 ip address 11.11.11.11/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode
interface loopback 1 
 ip address 100.100.100.100/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 11.11.11.11

feature pim 

ip pim rp-address 100.100.100.100
```
Leaf2
======
```python
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
interface loopback 0 
 ip address 12.12.12.12/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 12.12.12.12

feature pim 

ip pim rp-address 100.100.100.100
```

Underlay Configuration Step 5:BGP IPv4 Address-family
======================================================
Spine1
======
```python
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
 template peer LEAF
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
   route-reflector-client
  neighbor 11.11.11.11
   inherit peer LEAF 
  neighbor 12.12.12.12
   inherit peer LEAF 

```
Spine2
======
```python
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
 template peer LEAF
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
   route-reflector-client
  neighbor 11.11.11.11
   inherit peer LEAF 
  neighbor 12.12.12.12
   inherit peer LEAF 
```
Leaf1
======
```python
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
interface loopback 0 
 ip address 11.11.11.11/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode
interface loopback 1 
 ip address 100.100.100.100/32
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
 neighbor 1.1.1.1 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
 neighbor 2.2.2.2 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
```
Leaf2
======
```python
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
interface loopback 0 
 ip address 12.12.12.12/32
 ip router ospf UNDERLAY area 0.0.0.0
 ip pim sparse-mode

feature ospf 

router ospf UNDERLAY
 router-id 12.12.12.12

feature pim 

ip pim rp-address 100.100.100.100

router bgp 65000
 router-id 12.12.12.12
 address-family ipv4 unicast
 neighbor 1.1.1.1 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
 neighbor 2.2.2.2 
  remote-as 65000
  update-source loopback 0 
  address-family ipv4 unicast
   send-community
   send-community extended
```

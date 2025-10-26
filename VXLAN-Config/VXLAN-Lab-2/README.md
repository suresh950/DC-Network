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


## 3. Overlay
## 4. VLANs & VNIs
## 5. AnyCast G/W
## 6. EVPN
## 7. VxLAN
## 8. Validation

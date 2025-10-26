# Below is the LAB architecture we are going to configure
![](https://github.com/suresh950/DC-Network/blob/main/VXLAN-Config/VXLAN-Lab-2/Image/2025-10-26_11h00_55.png "LAB")

# Steps
## 1. Step 1. Overlay Configuration

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
int e1/1-4
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
int e1/1-4
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
int e1/1-4
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
int e1/1-4
 no switchport 
 no shut 
 mtu 9216
 exit
int loopback 0
 ip address 192.168.1.6/32
 no shut
 exit
```
## 2. Underlay
## 3. Overlay
## 4. VLANs & VNIs
## 5. AnyCast G/W
## 6. EVPN
## 7. VxLAN
## 8. Validation

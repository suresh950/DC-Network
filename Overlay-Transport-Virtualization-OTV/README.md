### Overlay Transport Virtualization Configuration Example

Let's look at an example of OTV configuration on Cisco Nexus 7000 switches. After following this example, you will learn how to configure OTV( Overlay Transport Virtualization).

### OTV Topology

In our topology, we have four nexus switches. NXOS01 and NXOS02 are connected while NXOS03 is connected to NXOS01 and VPC01, and NXOS04 is connected to NXOS02 and VPC02. Here is the topology diagram for the same.
In this Cisco Nexus OTV configuration example, first, we need to configure the basic layer 2 reachability from end hosts as follows
● Configure VPC01’s link to NXOS03 with the IP address 10.0.0.1/24. (VPCS > 10.0.0.1/24)
● Configure VPC02’s link to NXOS04 with the IP address 10.0.0.2/24. (VPCS > 10.0.0.2/24)

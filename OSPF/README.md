# Lab Overview
This lab demonstrates OSPF configuration and troubleshooting in a multi-router network. 
The network has been pre-configured with IP addresses and OSPF, and the tasks involve:

1. Adding a serial connection between R1 and R2.
2. Fixing missing routes and neighbor issues.
3. Troubleshooting connectivity to external networks.
4. Examining OSPF LSDB and LSA types.

## Serial Connection Between R1 and R2
Task: Configure the serial link and OSPF.
Configuration Steps:
On R1:
```
interface s0/0/0
ip address 10.1.1.1 255.255.255.252
clock rate 128000
no shutdown
ip ospf 1 area 0
```
On R2:
```
interface s0/0/0
ip address 10.1.1.2 255.255.255.252
no shutdown
ip ospf 1 area 0
```

Verify OSPF neighbor:
```
show ip ospf neighbor
State should be FULL, indicating adjacency formed.
```

## Missing Route to 10.0.2.0/24

Problem: Only R3 has a route to 10.0.2.0/24.

Cause:
OSPF on other routers is not advertising the network 10.0.2.0/24.
Likely the interface with 10.0.2.0/24 is not included in OSPF.

Fix:
Interface-level OSPF:
```
interface g0/1
ip address 10.0.2.1 255.255.255.0
ip ospf 1 area 0
no shutdown
```

## R2 and R4 Won’t Become OSPF Neighbors with R5

Problem: R2 and R4 do not form OSPF adjacency with R5.

Causes:
Area mismatch – OSPF area IDs must match on both sides.
Hello/Dead timer mismatch – must be the same on point-to-point and multi-access links.
Interfaces not participating in OSPF – missing ip ospf <process> area <area> or network command.

Fix:
Check area configuration:
```show ip ospf interface```

Adjust interfaces to the same area:
```
interface <interface>
ip ospf 1 area 0
```

Ensure Hello/Dead timers match if manually configured:
```
ip ospf hello-interval 10
ip ospf dead-interval 40
```

## Why R3 Cannot Run OSPF on 192.168.34.0/30

Problem: OSPF does not form adjacency or advertise this network.

Causes:
OSPF treats /30 as point-to-point, the interface may not be included in OSPF.

Fix:
```
interface g0/1
no ip ospf network point-to-point
```

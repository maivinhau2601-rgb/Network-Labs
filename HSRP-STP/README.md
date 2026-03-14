# Lab Overview
This lab demonstrates HSRP and STP configuration and troubleshooting in a network. 
The network has been pre-configured with IP addresses and the tasks involve:

1. Configuring STP in all switches and enabling safety features. 
2. Configuring HSRP in two routers
3. Testing the network using simulation mode on packet tracer.

## STP configuration on all switches
Configuration Steps:
On SW1 (Root Switch):
```
spanning-tree vlan 1
spanning-tree mode pvst
spanning-tree vlan 1 root primary
```
On other switches:
```
spanning-tree vlan 1
spanning-tree mode pvst
```

On switches connected to end hosts (PCs):
```
spanning-tree vlan 1
spanning-tree mode pvst
interface f0/3
spanning-tree portfast
spanning-tree bpduguard enable
spanning-tree bpdufilter enable
```

## Configuring HSRP in two routers using VIP (10.0.1.254)

On router 1 (active router):
```
interface g0/1
standby 1 ip 10.0.1.254 
standby 1 priority 200
standby preempt
```

On router 2 (standby router):
```
interface g0/1
standby 1 ip 10.0.1.254 
standby 1 priority 100
```



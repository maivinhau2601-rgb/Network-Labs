# Lab Overview
This lab demonstrates SSH configuration. 

The network has been pre-configured with IP addresses and OSPF, and the tasks involve:

1. Configure the Switch 2 via Laptop.
2. Enable SSH on Switch 2 and access control list to only allow connection from PC1.


## Conect laptop to switch 2 via console port
```
Switch2#(config) enable ccna
Switch2#(config) enable username maivinh secret ccna
Switch2#(config) interface vlan 1
Switch2#(config) ip address 192.168.2.252
Switch2#(config) no shutdown
Switch2#(config) exit

Switch2# ip default-gateway 192.168.2.254

Switch2# line console 0
Switch2#(config-line) login local
Switch2#(config-line) exec-timeout 120
Switch2#(config-line) exit

Switch2#(config) ip domain-name maivinh.com
Switch2#(config) crypto key generate rsa
Switch2#(config) ip access-list standard allowpc1

Switch2#(config-std-nacl)# permit host 192.168.1.1
Switch2#(config-std-nacl)# exit

Switch2#(config) line vty 0 15
Switch2#(config-line) login local
Switch2#(config-line) access-class allowpc1 in
Switch2#(config-line) exec-timeout 120
Switch2#(config-line) transport input ssh
Switch2#(config-line) exit
```


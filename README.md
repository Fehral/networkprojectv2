<p align=center>
  <ins>Overview of Concepts</ins>
</p>

VLANs
  * Virtual Local Area Networks are logically separated networks on a singular switch
  * like devices within a subnet, devices within a VLAN have their own broadcast domain
  * trunking allows multiple VLANs to communicate through a singular port, i.e., multiple VLANs communicating through a single switchport to a router in order for the VLANs to connect

DHCP
  * Dynamic Host Configuration Protocol is used to automatically allocate IPv4 addresses within a given address pool
  * routers can serve as DHCP servers in lieu of a dedicated physical DHCP server, particularly in home network environments

Wireless Networking
  * wireless access points are required for wireless connectivity
  * wireless access points are not routers, routers are separate devices
  * some routers come with wireless capability, such as one provided by an ISP or sold by a third-party home router manufacturer
  * wireless connectivity uses radio frequencies, either in 2.4GHz, 5GHz (or both 2.4GHz and 5GHz), or 6GHz bands
  
<p align=center>
  <ins>VLANs and DHCP Topology</ins>
</p>

<p align=center>
  <img src="https://github.com/Fehral/networkprojectv2/blob/main/networkproject2.png?raw=true">
</p>

R1 Configuation
```
Router>en
Router#conf t
Router(config)#int g0/0
Router(config-if)#no shutdown
Router(config-if)#int g0/0.10
Router(config-subif)#encapsulation dot1Q 10
Router(config-subif)#ip address 192.168.1.1 255.255.255.192
Router(config-subif)#int g0/0.20
Router(config-subif)#encapsulation dot1Q 20
Router(config-subif)#ip address 192.168.1.65 255.255.255.192
Router(config-subif)#int g0/0.30
Router(config-subif)#encapsulation dot1Q 30
Router(config-subif)#ip address 192.168.1.129 255.255.255.192
Router(config-subif)#exit
Router(config)#service dhcp
Router(config)#ip dhcp pool Dep1
Router(dhcp-config)#network 192.168.1.0 255.255.255.192
Router(dhcp-config)#default-router 192.168.1.1
Router(dhcp-config)#dns-server 192.168.1.1
Router(dhcp-config)#domain-name Dep1.com
Router(dhcp-config)#ip dhcp pool Dep2
Router(dhcp-config)#network 192.168.1.64 255.255.255.192
Router(dhcp-config)#default-router 192.168.1.65
Router(dhcp-config)#dns-server 192.168.1.65
Router(dhcp-config)#domain-name Dep2.com
Router(dhcp-config)#ip dhcp pool Dep3
Router(dhcp-config)#network 192.168.1.128 255.255.255.192
Router(dhcp-config)#default-router 192.168.1.129
Router(dhcp-config)#dns-server 192.168.1.129
Router(dhcp-config)#domain-name Dep3.com
Router(dhcp-config)#exit
Router(config)#exit
Router#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

S1 Configuration
```
Switch>en
Switch#conf t
Switch(config)#int range fa0/1-3
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 10
Switch(config-if-range)#int range fa0/4-6
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 20
Switch(config-if-range)#int range fa0/7-9
Switch(config-if-range)#switchport mode access
Switch(config-if-range)#switchport access vlan 30
Switch(config-if-range)#int g0/1
Switch(config-if)#switchport mode trunk
Switch(config-if)#exit
Switch(config)#exit
Switch#copy run start
Destination filename [startup-config]? [Enter]
Building configuration...
[OK]
```

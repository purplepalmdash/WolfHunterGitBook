## Check Installation
We have to do some modifications for finish this chapter and verfy the installation.     

### Network Verification
Check the network:    
```
# ifconfig eth0
eth0      Link encap:Ethernet  HWaddr 52:54:00:E5:5E:77  
          inet addr:10.15.33.2  Bcast:10.15.33.255  Mask:255.255.255.0
# ifconfig eth1
eth1      Link encap:Ethernet  HWaddr 52:54:00:78:9A:1F  
          inet addr:10.15.34.248  Bcast:10.15.34.255  Mask:255.255.255.0
```
Check the route:   
```
# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.15.34.0      0.0.0.0         255.255.255.0   U     0      0        0 eth1
10.15.33.0      0.0.0.0         255.255.255.0   U     0      0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 eth0
169.254.0.0     0.0.0.0         255.255.0.0     U     1003   0        0 eth1
0.0.0.0         10.15.33.1      0.0.0.0         UG    0      0        0 eth0
```
The default route is pointing at the Inner Network, thus this node won't connect internet, we have to correct this via Route modification.   
### Route Modification
Modify the sysconfig's network configuration:   
```
# vim /etc/sysconfig/network
- GATEWAY=10.15.33.1
+ GATEWAY=10.15.34.1
```
Reboot and test it via ping:   
```
# ping mirrors.aliyun.com
PING mirrors.aliyun.com (115.28.122.210) 56(84) bytes of data.
64 bytes from 115.28.122.210: icmp_seq=1 ttl=50 time=52.4 ms
64 bytes from 115.28.122.210: icmp_seq=2 ttl=50 time=52.9 ms
```
Now our WolfHunter Root Node is ready for Cobbler server setup.   

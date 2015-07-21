## CloudStack Agent
CloudStack Agent node's deployment is much more easier than management node, walking through following steps you will configure the machine as the Agent node.    

### Configure Network
Do following for configure the network.   

```
# vim /etc/hostname
CSAgent
# vim /etc/hosts
+ 127.0.0.1	CSAgent
+ 10.15.33.6	CSAgent
# reboot
```
Verify host:   

```
# hostname --fqdn
CSAgent
```

### Installation
Simply install one package then you have done this part:   

```
# yum install -y cloud-agent
```

### Configuration
Change following configurations:    

```
# sed -i 's/#vnc_listen = "0.0.0.0"/vnc_listen = "0.0.0.0"/g' /etc/libvirt/qemu.conf && sed -i 's/cgroup_controllers=["cpu"]/#cgroup_controllers=["cpu"]/g' /etc/libvirt/qemu.conf
# sed -i 's/#listen_tls = 0/listen_tls = 0/g' /etc/libvirt/libvirtd.conf && sed -i 's/#listen_tcp = 1/listen_tcp = 1/g' /etc/libvirt/libvirtd.conf && sed -i 's/#tcp_port = "16509"/tcp_port = "16509"/g' /etc/libvirt/libvirtd.conf && sed -i 's/#auth_tcp = "sasl"/auth_tcp = "none"/g' /etc/libvirt/libvirtd.conf && sed -i 's/#mdns_adv = 1/mdns_adv = 0/g' /etc/libvirt/libvirtd.conf
# sed -i 's/#LIBVIRTD_ARGS="--listen"/LIBVIRTD_ARGS="--listen"/g' /etc/sysconfig/libvirtd 
# sed -i '/cgroup_controllers/d' /usr/lib64/python2.7/site-packages/cloudutils/serviceConfig.py
```

Restart libvirtd service:     

```
# service libvirtd restart
```

### End Of This Section
By now we have setup the CloudStack Agent node which runs on CentOS7.1. In following section we will import template and let our Agent be controlled by CloudStack Management Node.     

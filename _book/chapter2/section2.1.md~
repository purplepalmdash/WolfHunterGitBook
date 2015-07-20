## Setup Cobbler Server
In this section, we will setup and configure Cobbler 2.9, the more detailed information could be refers to:    
[https://cobbler.github.io/manuals/2.6.0/](https://cobbler.github.io/manuals/2.6.0/)    

Following is just a quick-start for setting up Cobbler Server in our WolfHunter environment. Cobbler Server will only listens on WolfHunter's Inner network and serves this network's PXE based deployment.    

### Installation
First disable SElinux via:    
```
# vim /etc/selinux/config
- SELINUX=permissive
+ SELINUX=disabled
# reboot
```
Download repo file , update the cache, and install cobbler, cobbler requires django so we also have to enable epel repository:   
```
# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6.repo
# wget http://download.opensuse.org/repositories/home:/libertas-ict:/cobbler26/CentOS_CentOS-6/home:libertas-ict:cobbler26.repo
# cp home\:libertas-ict\:cobbler26.repo /etc/yum.repos.d/cobbler.repo
# yum install -y cobbler cobbler-web
```
Check the installation result via:    
```
# cobbler --version
Cobbler 2.6.9
  source: ?, ?
  build time: Fri Jun 12 07:41:35 2015
```
### Cobbler Configuration
We have to do some configurations to specify the way cobbler server works. Following are the steps:    

1\. Generate encrypted password string:   

```
# openssl passwd -1
Password: 
Verifying - Password: 
$awegwaegoguoweuouoeh/
```
Record the generated string, later we must use it.   

2\. Edit the Cobbler Setting file:    

Change Default Password:    

```
# vim /etc/cobbler/setttings
- default_password_crypted: "$1$mF86/UHC$WvcIcX2t6crBz2onWxyac."
+ default_password_crypted: "$awegwaegoguoweuouoeh/"
```
server will listen on Inner network, change it to:   
```
- server: 127.0.0.1
+ server: 10.15.33.2
```
And the next_server parameter:    
```
- next_server: 127.0.0.1
+ next_server: 10.15.33.2
```
Let Cobbler Server manage the dhcp in Inner Server:    
```
- manage_dhcp: 0
+ manage_dhcp: 1
```

3\. Create the dhcpd template:    

Cobbler will use dhcpd template for rendering system's dhcpd daemon configuration.    

```
# vim /etc/cobbler/dhcp.template
- subnet 192.168.1.0 netmask 255.255.255.0 {
-      option routers             192.168.1.5;
-      option domain-name-servers 192.168.1.1;
-      option subnet-mask         255.255.255.0;
-      range dynamic-bootp        192.168.1.100 192.168.1.254;
-      default-lease-time         21600;
-      max-lease-time             43200;
-      next-server                $next_server;

+ subnet 10.15.33.0 netmask 255.255.255.0 {
+      option routers             10.15.33.1;
+      range dynamic-bootp        10.15.33.4 10.15.33.254;
+      option domain-name-servers 180.76.76.76, 8.8.8.8;     
+      option subnet-mask         255.255.255.0;         
+      filename                   "/pxelinux.0";       
+      default-lease-time         21600;           
+      max-lease-time             43200;      
+      next-server                $next_server; 
```

4\. Install and configure xinetd:    

```
# yum install -y xinetd
# chkconfig xinetd on
```

5\. Install and configure dhcp server:    

```
# yum install -y dhcp
# chkconfig dhcpd on
```
Write a "fake" dhcpd configuration file for temperately start the dhcpd for first time, later dhcpd will be managed by cobbler:    
```
# vim /etc/dhcp/dhcpd.conf
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp*/dhcpd.conf.sample
#   see 'man 5 dhcpd.conf'
#
# create new
# specify domain name
option domain-name "server.world";
# specify name server's hostname or IP address
option domain-name-servers 180.76.76.76;
# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid
authoritative;
# specify network address and subnet mask
subnet 10.15.33.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.15.33.3 10.15.33.254;
    # specify broadcast address
    option broadcast-address 10.15.33.255;
    # specify default gateway
    option routers 10.15.33.1;
}
# service dhcpd start
```

6\. Now we chkconfig cobbler and restart the machine to continue:    

```
# chkconfig httpd on
# chkconfig cobblerd on
# reboot
```

7\. Install and configure tftp-server

```
# yum install -y tftp-server
```
Configure it:   
```
# vim /etc/xinetd.d/tftp
        - disable                 = yes
        + disable                 = no
```

8\. Get boot-loader for tftp server:   

```
# cobbler get-loaders
# ls /var/lib/cobbler/loaders/ -l -h
total 1.2M
......
```

9\. Enable rsync in xinetd.d:    

```
# vim /etc/xinetd.d/rsync
        - disable = no
        + disable = yes
```

10\. Install following packages:    

```
# yum install -y debmirror pykickstart cman
```

11\. Modify iptables rules and restart:   

Notice, add the 3 lines under the corresponding position.    

```
# vim /etc/sysconfig/iptables
:OUTPUT ACCEPT [0:0]
+   -A INPUT -p udp -m multiport --dports 69,80,443,25151 -j ACCEPT 
+   -A INPUT -p tcp -m multiport --dports 69,80,443,25151 -j ACCEPT 
+   -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# reboot
```

12\. Check all of the conditions and you will see:   

```
# cobbler check
The following are potential configuration items that you may want to fix:

1 : since iptables may be running, ensure 69, 80/443, and 25151 are unblocked
2 : comment out 'dists' on /etc/debmirror.conf for proper debian support
3 : comment out 'arches' on /etc/debmirror.conf for proper debian support

Restart cobblerd and then run 'cobbler sync' to apply changes.
```

13\. Cobbler sync

Simply use `cobbler sync` for syncing all of the configurations.   

### Check the Cobbler Server Status
Check the dhcpd:
```
# ps -ef |grep dhcp
dhcpd     1911     1  0 17:51 ?        00:00:00 /usr/sbin/dhcpd -user dhcpd -group dhcpd
```
Check the tftp server status: 
```
# netstat -anp |grep 69
udp        0      0 0.0.0.0:69                  0.0.0.0:*                               1488/xinetd         
```

Now our Cobbler server is ready for serving PXE deployment, in next section we will introduce the ISO for really deployment. 

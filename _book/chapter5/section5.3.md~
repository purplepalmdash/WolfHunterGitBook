## Using Local Repo

### CentOS6.6 Using Local Repository
Create storage: 
```
# qemu-img create -f qcow2 WolfHunterThirdNode.qcow2 100G
```
Create a new machine, which only have the Inner Connection, and boot from PXE, select CentOS6.5, let it run installing and ready for login.      

Configure repo:   

```
# cd /etc/yum.repos.d/
# mv CentOS-* /root/
# vim cloudstack.repo
[cloudstack]
name=cloudstack
baseurl=http://10.15.33.2/repo/45CSC6Manage
enabled=1
gpgcheck=0
# yum makecache
```

Now install all of the rpm packages using local repository.    

```
# yum install -y ntp
# yum intall -y libselinux-python
# yum install -y libselinux-python
# yum install -y mysql-server
# yum install -y MySQL-python
# yum install -y cloudstack-management
# yum install -y python-pip
```
We won't really configure Mgmt, just verify the installation available is OK.   

### CentOS7.1 Using Local Repository
Create Storage:    
```
# qemu-img create -f qcow2 WolfHunterFourthNode.qcow2 100G
```
Create a new machine, which only have the Inner Connection, and boot from PXE, select CentOS7.1 at PXE Menu.        

Configure repo:   

```
# cd /etc/yum.repos.d/
# mv CentOS-* /root/
# vim cloudstack.repo
[cloudstack]
name=cloudstack
baseurl=http://10.15.33.2/repo/45CSC7Agent
enabled=1
gpgcheck=0
# yum makecache
```

Verify repo:    

```
# yum install -y cloud-agent
```

### End Of The Section
By now we could install all of the packages using the local repository. In next section we will localize cloudmonkey installation.   

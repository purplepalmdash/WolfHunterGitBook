## Environment Preparation

In this chapter we will introduce how to setup the whole deployment and verification for WolfHunter.    

You can use physical machine or virtual machine for setting up the WolfHunter deployment environment. Virtual environment is suggested because it won't import more problems on real physical networking.  

The virtualization technology here we use is **KVM**.    

### Hardware Environment
* **CPU**: i5/i7, better you have more than 2 real-core.
* **Memory**: At least 8G. 
* **Disk**: More than 100G. 
* **Network**: 1 Physical Ethernet which could connect to internet. 

### System Environment
The host machine should run Linux, I suggest you install CentOS 7 or Ubuntu14.04. Old linux distributions are not suggested because they may cause some problems in virtualization technology. I used to install CentOS 7 virtual machine on CentOS 6 host machine, but its nested CPU feature won't be taken to the guest machine. Choose the newer distribution will greatly save your effort in building the whole environment.    

In this book, I use CentOS 7.1 for deployment. You can check your Linux distribution and Kernel version via following command:    
```
# lsb_release  -r
Release:        7.1.1503
# uname -r
3.10.0-229.7.2.el7.x86_64
```
### Virtualization Technology
Because we use several kvm virtual machine for emulating the real deployment environment, we need to create "fake" physical machine via nested-virtualization, be sure you enabled the virtualization technology in BIOS. 
```
# egrep --color=auto 'vmx|svm|0xc0f' /proc/cpuinfo
```
If nothing is displayed after running that command, then your processor does not support hardware virtualization, and you will not be able to use KVM.    
Linux use kernel modules to support KVM and VIRTIO, use following commands to check if these modules are loaded at system startup:     
```
# lsmod | grep kvm
# lsmod | grep virtio
```
### Software Environment
We use virt-manger for managing the virtual machine and virtual network, so first make sure you have installed the corresponding packages. 
In CentOS7, use following command for checking:    
```
# rpm -qa | grep virt-manager
virt-manager-common-1.1.0-12.el7.noarch
virt-manager-1.1.0-12.el7.noarch
```
Or, on Ubuntu14.04, use following command for checking:    
```
$ dpkg -l | grep virt-manager
ii  virt-manager                                          0.9.5-1ubuntu3                                      all          desktop application for managing virtual machines
```

If everything goes well, we can start to deploy system now.   

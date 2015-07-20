## WolfHunter Root Machine
We name the first deployed machine "Root Machine", because this machine is firstly introduced to the whole deployment environment, this machine acts as PXE Server, which will deploy all of the later added machine, and it holds all of the WolfHunter Ansible scripts which are used for deploying CloudStack environment.   

### Disk Preparation
Use following command for prepare the disk image file. 
```
# qemu-img create -f qcow2 WolfHunterRoot.qcow2 100G
```

### Machine Setup
Click "New" to create a new virtual machine:    
![/images/2015_07_20_16_01_41_332x139.jpg](/images/2015_07_20_16_01_41_332x139.jpg)    
Choose "Local install media" and click "Forward":    
![/images/2015_07_20_16_03_14_404x381.jpg](/images/2015_07_20_16_03_14_404x381.jpg)    
Select your downloaed CentOS6.6 DVD:    
![/images/2015_07_20_16_05_23_482x393.jpg](/images/2015_07_20_16_05_23_482x393.jpg)   
Change the memory and cpu parameters:   
![/images/2015_07_20_16_06_51_400x359.jpg](/images/2015_07_20_16_06_51_400x359.jpg)    
Specify the Disk File:   
![/images/2015_07_20_16_10_50_404x388.jpg](/images/2015_07_20_16_10_50_404x388.jpg)    
Name the virtual machine, and check for customizatio(because later we have to add the network), click "Finish":    
![/images/2015_07_20_16_12_25_476x396.jpg](/images/2015_07_20_16_12_25_476x396.jpg)    
Customize the Disk, for let it has the fastest speed:   
![/images/2015_07_20_16_15_30_795x536.jpg](/images/2015_07_20_16_15_30_795x536.jpg)    
Add a new network card and assign it to Ext network:   
![/images/2015_07_20_16_16_10_868x524.jpg](/images/2015_07_20_16_16_10_868x524.jpg)    
Modify the existing network for using the Inner network:   
![/images/2015_07_20_16_16_22_871x372.jpg](/images/2015_07_20_16_16_22_871x372.jpg)    
Click "Begin Installation" for setup the machine.   

### Customize Network In System
Specify "Hostname" during setup:  
![/images/2015_07_20_16_25_41_356x148.jpg](/images/2015_07_20_16_25_41_356x148.jpg)   
Click "Configure Network" and setup the 2 network:   
![/images/2015_07_20_16_29_51_631x569.jpg](/images/2015_07_20_16_29_51_631x569.jpg)    
eth1 is for external network, setup it via:    
![/images/2015_07_20_16_31_29_639x571.jpg](/images/2015_07_20_16_31_29_639x571.jpg)    
Select "Basic Server" for installation.   

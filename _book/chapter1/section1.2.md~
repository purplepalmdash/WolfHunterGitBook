## Network Preparation
WolfHunter will acts as a deployment administrator in the whole network, so the first step for setting up the WolfHunter Dev/Test Env is to setup the Network.    

Fortunately virt-manager provides a very convenient method for creating and managing the virtual network. Following we will setup an isolated network which emulates the real networking in production environment.   

### Inner and External Network
We need to setup 2 networks for deployment, Inner and External. 

We assume all of the nodes are "locked" in inner network, all of them have no direct connection to internet. 

External Network is only opened for WolfHunter Deployer Node, which runs Cobbler and Ansible Server. This server need internet connection for updating some packages, but once the environment is ready, this external interface could be removed from Deployer Node. 

IP Address Arrangement:
* **Inner**: 10.15.33.0/24
* **External**: 10.15.34.0/24

### WolfHunter Inner Network
Open virt-manager, double-click localhost(QEMU):    
![../images/2015_07_20_15_16_52_344x139.jpg](../images/2015_07_20_15_16_52_344x139.jpg)   
Choose "Virtual Networks" tab:    
![../images/2015_07_20_15_18_42_421x63.jpg](../images/2015_07_20_15_18_42_421x63.jpg)    
Click the "+", and named your virtual network name:  
![../images/2015_07_20_15_21_24_518x476.jpg](../images/2015_07_20_15_21_24_518x476.jpg)   
Set the IP Address and Disable the DHCP, we disabled the DHCP now because later our deployed Cobbler Server will manage this network DHCP:    
![../images/2015_07_20_15_33_36_514x409.jpg](../images/2015_07_20_15_33_36_514x409.jpg)   
Directly click "Forward" for next step:    
![../images/2015_07_20_15_36_57_514x415.jpg](../images/2015_07_20_15_36_57_514x415.jpg)   
Choose "isolated network" then click "Finish":   
![../images/2015_07_20_15_38_47_514x415.jpg](../images/2015_07_20_15_38_47_514x415.jpg)   

### WolfHunter External Network
The steps are mainly the same, but with some minor modifications. 
Define the name:   
![../images/2015_07_20_15_45_47_511x440.jpg](../images/2015_07_20_15_45_47_511x440.jpg)    
Enable DHCP, because this is external network, so we let host machine managing the whole network's DHCP:    
![../images/2015_07_20_15_46_13_511x439.jpg](../images/2015_07_20_15_46_13_511x439.jpg)    
Choose "Forwarding to ..." thus you have internet connection in this network:   
![../images/2015_07_20_15_46_28_511x438.jpg](../images/2015_07_20_15_46_28_511x438.jpg)    

Until now we have setup the Network Environment of the WolfHunter, with this pre-defined networking, we could continue to next step for settting up the cobbler server.    

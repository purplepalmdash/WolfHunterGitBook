## Setup Ansible Node
Since we made `10.15.33.2` as the WolfHunter Root Machine, which means this machine will take reposible for PXE Deployment Server, and it should have more roles, like:    
* Web Server. 
* NTP Server. 
* Repository Server. 
* Ansible Node.

The main purpose for this Root Machine is for much more easier deployment, idealy we only need to maintain one server, then we take this "Root Machine" to a seperated network, then we could deploy several different kinds of service in this network.   

In this section we mainly install and setup Ansible Node in Root Machine. 

### Installation
Install it via:   

```
# yum install -y ansible sshpass
```

Check it via:    

```
# ansible --version
ansible 1.9.2
  configure module search path = None
```

### Reach Node
In last chapter we created a node which has been deployed via Cobbler Server. Now we want to use Ansible for reach it.   

First we create a directory which used for holding all of our Ansible testing code.   

```
# mkdir -p ~/Code/Ansible
# cd ~/Code/Ansible
```

Ansible need a configuration file for holding all of the configuration information, create this file and enter the following content:     

```
# vim ~/Code/Ansible/ansible.cfg
[defaults]
hostfile=./wolfHunterHosts
```

This file indicates the hostfile for holding all of the host related information locates in the same directory with the name of `WolfHunterHosts`. Now we create this file with following content:    

```
# vim ~/Code/Ansible/WolfHunterHosts
[WolfHunterHosts]
10.15.33.5
```

Now we first manually do a "fake" login to remote machine "10.15.33.5", the purpose is to add the remote's definition into our ssh's list of known hosts. You needn't input the correct password, just hit CTRL+C when you see the password hint:            

```
# ssh root@10.15.33.5
........
Are you sure you want to continue connecting(yes/no)? yes
Warning:................
root@10.15.33.5's password: HIT CTRL+C
```

Now use ansible for play a ping-pong with remote host, this time please input the correct password:    

```
# ansible all -m ping --ask-pass
SSH password: 
10.15.33.5 | success >> {
	"changed": false,
	"ping": "pong"
```
See? you send out a ping, and now you got a pong. Congratulations, you have reached the node!    

### Only Talk To One Node
The host file could have a list of nodes, but we could specify whichever node we want to talk to, like following example:    

```
# ansible WolfHunterFirstNode -m shell -a "uptime" --ask-pass
SSH passwords:
10.15.33.5	| success	| rc=0 >>
  13:50:36 up	1:58, 2 users, load average: 0.00, 0.00, 0.00
```
The command which we entered in last part means we want to ping all of the nodes which listed in the host file.   

### End Of This Section
In this section we installed the Ansible Node, and use defined 2 files, only with these 2 files we reached our newly deployed node. In next Chapter we will introduce Ansible Playbooks for installing/configurating/uninstalling NTP Services on this node, which will give a good start-point for more compliated deployment.    

## Deploy CS Agent

### Preparation
First we add the host into our host definition file:    

```
# vim /root/Code/Ansible/WolfHunterHosts
[CSAgent]
10.15.33.108
```

Install ssh key into destination machine via:    

```
# ansible-playbook addkey.yml --ask-pass
```

### Deployment
Very simple:    

```
# ansible-playbook 45CSAgent.yml
```

After running this playbook the CloudStack Agent will be installed to the destination node.  

### End Of This Section
The ansible source code will be available in next section.    

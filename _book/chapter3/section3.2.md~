## Powerful Playbooks
In previous section we talked about ad-hoc Ansible commands, which means those tasks we could only run againt client nodes. Then in this section we introduce Ansible Playbooks, which makes you "combine" several tasks into a file, by running this file on different nodes, those nodes could be deployed as we planned.    

The playbook is a little bit like Chef's cookbook, which records all of the detailed steps for archiving some specified goal.   

### A Simple Playbook
Learn by doing is always the best way for get familiar with something strange, we will start learning playbooks via a simple example -- no password login.    

Use ssh for login remote server without enter password is pretty simple via following command in shell:    

```
# ssh-copy-id xxx@remoe_ip_address
# ssh xxx@remote_ip_address
``` 
The facts under `ssh-copy-id` command is pretty simple: we upload our local generated `~/.ssh/id_rsa.pub` into the remote server's `~/.ssh/authorized_keys`. Once remote machine believe your machine is authorized, next time you won't enter the supid password for login.    

No Password login is pretty simple: copy local's `id_rsa.pub` content to remote machine's `authorized_keys`.     

Let's look at the ansible playbooks which used for finish this task:    

```
# vim /root/Code/Ansible/addkey.yml
--- 
- hosts: all
  sudo: yes
  gather_facts: no
  remote_user: root

  tasks:

  - name: install ssh key
    authorized_key: user=root
                    key="{{ lookup('file', '/root/.ssh/id_rsa.pub') }}" 
                    state=present
```

Don't hesitate to run this playbook, first we generate the `id_rsa.pub` file via following command:     

```
# ssh-keygen -t rsa -b 2048
```
Notice: If you already have `id_rsa.pub` under your /root/.ssh, you needn't generate a new file again.    

Now use ansible-playbook for running this file:    

```
# ansible-playbook addkey.yml --ask-pass
SSH password: 

PLAY [all] ******************************************************************** 

TASK: [install ssh key] ******************************************************* 
changed: [10.15.33.5]

PLAY RECAP ******************************************************************** 
10.15.33.5                         : ok=1    changed=1    unreachable=0    failed=0   
```
Now if you login into 10.15.33.5, you won't be asked to input password again.    

```
# ssh root@10.15.33.5
Last login: Mon Jul 20 14:18:30 2015 from 10.15.33.2
```

We won't spend more time on playbooks' syntax, if you want to dive into its syntax, simply refers to Ansible official website.     

### NTP Playbooks


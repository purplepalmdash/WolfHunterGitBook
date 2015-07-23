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
10.15.33.5                      : ok=1    changed=1    unreachable=0    failed=0   
```
Now if you login into 10.15.33.5, you won't be asked to input password again.    

```
# ssh root@10.15.33.5
Last login: Mon Jul 20 14:18:30 2015 from 10.15.33.2
```

We won't spend more time on playbooks' syntax, if you want to dive into its syntax, simply refers to Ansible official website.     

### NTP Playbooks

#### Install NTP Service

Edit the ntp installation playbook like following:    

```
# vim ~/Code/Ansible/ntp-install.yml
---
- hosts: all
  sudo: yes
  gather_facts: yes

  tasks:

  - name: install ntp
    yum: name=ntp state=installed update_cache=yes
    when: ansible_os_family == "RedHat"

  - name: start ntp
    service: name=ntpd state=started enabled=true
    when: ansible_os_family == "RedHat"
```

Deploy it via:   

```
# ansible-playbook ntp-install.yml

PLAY [all] ******************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [10.15.33.5]

TASK: [install ntp] *********************************************************** 
ok: [10.15.33.5]

TASK: [start ntp] ************************************************************* 
changed: [10.15.33.5]

PLAY RECAP ******************************************************************** 
10.15.33.5                 : ok=3    changed=1    unreachable=0    failed=0   
```

Check the result on 10.15.33.5:    

```
# ps -ef | grep ntp
root     20887     1  0 02:25 ?       
  00:00:00 ntpd -u ntp:ntp -p /var/run/ntpd.pid -g
```

#### Configure NTP Service
Use playbooks we could configure the installed service, since sometimes we need to configure the ntpd qeury server, we could do this via ansible-playbook.   

Add the ntp configuration file:    

```
# mkdir ~/Code/Ansible/files
# vim ~/Code/Ansible/files/ntp.conf
driftfile /var/lib/ntp/ntp.drift
statistics loopstats peerstats clockstats
filegen loopstats file loopstats type day enable
filegen peerstats file peerstats type day enable
filegen clockstats file clockstats type day enable
server 0.ubuntu.pool.ntp.org
server 1.ubuntu.pool.ntp.org
server 2.ubuntu.pool.ntp.org
server 3.ubuntu.pool.ntp.org
server ntp.ubuntu.com
restrict -4 default kod notrap nomodify nopeer noquery
restrict -6 default kod notrap nomodify nopeer noquery
restrict 127.0.0.1
restrict ::1
```

Now modify the ntp.yml like following:   

```
# vim ~/Code/Ansible/ntp-install.yml
---
- hosts: all
  sudo: yes
  gather_facts: yes

  tasks:

  - name: install ntp
    yum: name=ntp state=installed update_cache=yes
    when: ansible_os_family == "RedHat"

  - name: write our ntp.conf
    copy: src=files/ntp.conf dest=/etc/ntp.conf mode=644 owner=root group=root
    notify: restart ntp

  - name: start ntp
    service: name=ntpd state=started enabled=true
    when: ansible_os_family == "RedHat"

  handlers:

  - name: restart ntp
    service: name=ntpd state=restarted
```

Re-run the playbook, the output is:   

```
# ansible-playbook ntp-install.yml

PLAY [all] ******************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [10.15.33.5]

TASK: [install ntp] *********************************************************** 
ok: [10.15.33.5]

TASK: [write our ntp.conf] **************************************************** 
ok: [10.15.33.5]

TASK: [start ntp] ************************************************************* 
ok: [10.15.33.5]

PLAY RECAP ******************************************************************** 
10.15.33.5                 : ok=4    changed=0    unreachable=0    failed=0   
```

Now check the result on 10.15.33.5:    

```
# cat /etc/ntp.conf
....
```

We could see the ntp's configuration file has been modified.   

#### Uninstall NTP Service
The playbook for unintalling ntp service is quite easy, the file is:    

```
# vim ~/Code/Ansible/ntp-remove.yml
---
- hosts: all
  sudo: yes
  gather_facts: yes

  tasks:

  - name: remove ntp
    yum: name=ntp state=absent
    when: ansible_os_family == "RedHat"
```

Running Result:   

```
# ansible-playbook ntp-remove.yml

PLAY [all] ******************************************************************** 

GATHERING FACTS *************************************************************** 
ok: [10.15.33.5]

TASK: [remove ntp] ************************************************************ 
changed: [10.15.33.5]

PLAY RECAP ******************************************************************** 
10.15.33.5                 : ok=2    changed=1    unreachable=0    failed=0   
```

Check result:   

```
# ps -ef | grep ntp
# rpm -qa ntp
```

Notice: because your node couldnot reach internet, the remove ntp may fail, so first you should remove all of the official repo files:   

``` 
# mv /etc/yum.repos.d/CentOS* /root/
```

### End Of This Section
By walking through this section, we have learned how ansible interactive with the remote node, how to use ansible to install/configure/remove service in destination node. Ansible could easily "translate" your ssh commands into playbooks, and combine them into files names "playbooks", by using playbooks we could easily deploy services.    

In following chapters we will combine Cobbler and Ansible.   

## Playbooks For CS Agent

### 45CSAgent.yml
Cloudstack Agent node playbook: 

```
############################################################
# This script is written for deploying the CloudStack Agent.
# Based on CentOS7, CloudStack Agent 4.5.1
############################################################
- name: CloudStack Agent Installation Playbook For CentOS7
  hosts: all
  sudo: yes
  gather_facts: yes

  # Various tasks goes here.   
  tasks:

  #######################################################
  # Fail if not ran on CentOS
  # Delete or comment out to bypass.
  # Todo:  How to check whether this OS is CentOS7/6? 
  #
  - name: Check guest OS version
    fail: msg="WARNING - CloudStack playbook written for CentOS 7 (OS detected {{ ansible_distribution }})."
    when: ansible_distribution != "CentOS" 
    tags:
      - base

  #############################################################
  # Configure CloudStack yum repo
  #
  - name: Configure CloudStack Agent repo
    template: src=templates/cloudstackagent7.repo.j2 dest=/etc/yum.repos.d/cloudstackagent.repo mode=0644
    tags:
      - base
      - yumrepo

  #######################################################
  #  Remove CentOS Repo
  #
  - name: Remove CentOS Repo
    shell: mv -f /etc/yum.repos.d/CentOS-* /root/ 2>/dev/null
    ignore_errors: yes
    tags:
      - base
      - ClearCentOSRepo

  #######################################################
  #  Re-make Repo Cache
  #
  - name: Re-generate the repository cache
    shell: yum clean all && yum makecache
    tags:
      - base
      - ReGenerateCache

  #######################################################
  #  Install the CloudStack Agent
  #
  - name: Install CloudStack Agent
    yum: name=cloud-agent state=present
    tags:
      - base
      - cloud-agent

  #######################################################
  #  Configure qemu.conf
  #
  - name: Configure the qemu.conf
    shell: sed -i 's/#vnc_listen = "0.0.0.0"/vnc_listen = "0.0.0.0"/g' /etc/libvirt/qemu.conf && sed -i 's/cgroup_controllers=["cpu"]/#cgroup_controllers=["cpu"]/g' /etc/libvirt/qemu.conf
    run_once: true
    tags:
      - base
      - config-cloudstack

  #######################################################
  #  Configure libvirtd.conf
  #
  - name: Configure the libvirtd.conf
    shell: sed -i 's/#listen_tls = 0/listen_tls = 0/g' /etc/libvirt/libvirtd.conf && sed -i 's/#listen_tcp = 1/listen_tcp = 1/g' /etc/libvirt/libvirtd.conf && sed -i 's/#tcp_port = "16509"/tcp_port = "16509"/g' /etc/libvirt/libvirtd.conf && sed -i 's/#auth_tcp = "sasl"/auth_tcp = "none"/g' /etc/libvirt/libvirtd.conf && sed -i 's/#mdns_adv = 1/mdns_adv = 0/g' /etc/libvirt/libvirtd.conf
    run_once: true
    tags:
      - base
      - config-cloudstack

  #######################################################
  #  Configure sysconfig's libvirtd
  #
  - name: Configure the sysconfig libvirtd.conf
    shell: sed -i 's/#LIBVIRTD_ARGS="--listen"/LIBVIRTD_ARGS="--listen"/g' /etc/sysconfig/libvirtd 
    run_once: true
    tags:
      - base
      - config-cloudstack

  #######################################################
  #  Remove the serviceConfig.py's configuration on cgroup_controllers
  #
  - name: Remove cgroup_controllers configuration
    shell: sed -i '/cgroup_controllers/d' /usr/lib64/python2.7/site-packages/cloudutils/serviceConfig.py
    tags:
      - base
      - config-cloudstack

  #######################################################
  #  Restart libvirtd
  #
  - name: Restart the libvirtd
    service: name=libvirtd state=restarted
    tags:
      - base
      - config-cloudstack

  #######################################################
  # Configure Hosts
  #
  - name: Configure Hosts
    shell: ifconfig | awk -v MYHOST=`hostname --fqdn` '/inet/{print substr($2,1),"\t",MYHOST}'>>/etc/hosts
    tags:
      - base

```

### Related Files
Repository definition file:    

```
# vim templates/cloudstackagent7.repo.j2
[cloudstackagent]
name=cloudstackagent
baseurl=http://10.15.33.2/repo/45CSC7Agent
enabled=1
gpgcheck=0
```

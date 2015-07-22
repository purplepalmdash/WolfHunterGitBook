## Download Packages

### Tool Preparation
Install yum-plugin-downloadonly for only download the packages without installing, thus we could get all of the packages we want to install and save them to specified position.    

```
# yum install -y yum-plugin-downloadonly
```

### Create CloudStack Management Repo
From the above chapter, we downloaded all of the packages which used for deploying CS Managment Node for:    

```
# mkdir -p /root/Code/repo
# yum install --downloadonly --downloaddir=/root/Code/repo ntp libselinux-python mysql-server MySQL-python cloudstack-management python-pip 
```

After a long wait, these packages will be downloaded to local.   

### Create CloudStack Agent Repo
The steps are similar but quite easier, do following:    

```
# mkdir -p /root/Code/repo
# yum install --downloadonly --downloaddir=/root/Code/repo ntp cloud-agent
```

With these downloaded packages we could setup the repo locally in one seperated network.   

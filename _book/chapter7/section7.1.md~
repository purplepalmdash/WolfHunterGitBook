## Cobbler API
Two useful material for Cobbler API is listed as following:    
[https://fedorahosted.org/cobbler/wiki/CobblerApi](https://fedorahosted.org/cobbler/wiki/CobblerApi)      
[https://fedorahosted.org/cobbler/wiki/CobblerXmlrpc](https://fedorahosted.org/cobbler/wiki/CobblerXmlrpc)    

You could get more detailed infos from the above two links. We won't talk too much on which APIs to choose here. 

We Choose `XMLRPC API for Cobbler` for development, because it's pretty straight-forward and simple.    

### About XMLRPC
The introduction is directly from Cobbler API:    

XMLRPC is a lightweight way for computer programs written in various languages to interact over the network. See [http://www.xmlrpc.com/](http://www.xmlrpc.com).    

You should use the XMLRPC API for Cobbler if:

* You want to talk to Cobbler and you are not a Python application/script
* You want to talk to Cobbler and are not running on the Cobbler server
* You are a non-GPLd application that wants to talk to Cobbler that is to be distributed to the public or customers

### Begin Talk to Cobbler
Notice: All of these operations are done under python intepreter.     

Create the CobblerServer instance and token:    

```
# python
Python 2.6.6 (r266:84292, Jan 22 2014, 09:42:36) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import xmlrpclib
>>> CobblerServer = xmlrpclib.Server("http://127.0.0.1/cobbler_api")
>>> token = CobblerServer.login("cobbler", "engine")
``` 
Listed all of the distros in Cobbler System:    

```
>>> for i in CobblerServer.get_distros():
...   print i
... 
{'comment': '', 'kernel': '/var/www/cobbler/ks_mirror/CentOS-7.1-x86_64/images/pxeboot/vmlinuz', 'uid': 'MTQzNzM5MTExOC43NDE4NTUyMzEuNjI3NjM', 'kernel_options_post': {}, 'redhat_management_key': '<<inherit>>', 'kernel_options': {}, 'redhat_management_server': '<<inherit>>', 'initrd': '/var/www/cobbler/ks_mirror/CentOS-7.1-x86_64/images/pxeboot/initrd.img', 'mtime': 1437391118.8802841, 'template_files': {}, 'ks_meta': {'tree': 'http://@@http_server@@/cblr/links/CentOS-7.1-x86_64'}, 'boot_files': {}, 'breed': 'redhat', 'os_version': 'rhel7', 'mgmt_classes': [], 'fetchable_files': {}, 'tree_build_time': 0, 'arch': 'x86_64', 'name': 'CentOS-7.1-x86_64', 'owners': ['admin'], 'ctime': 1437391118.744396, 'source_repos': [['http://@@http_server@@/cobbler/ks_mirror/config/CentOS-7.1-x86_64-0.repo', 'http://@@http_server@@/cobbler/ks_mirror/CentOS-7.1-x86_64']], 'depth': 0}
{'comment': '', 'kernel': '/var/www/cobbler/ks_mirror/CentOS-6.5-x86_64/images/pxeboot/vmlinuz', 'uid': 'MTQzNzM5MTU2OS4wNjExNDMxMzYuMzE0NA', 'kernel_options_post': {}, 'redhat_management_key': '<<inherit>>', 'kernel_options': {}, 'redhat_management_server': '<<inherit>>', 'initrd': '/var/www/cobbler/ks_mirror/CentOS-6.5-x86_64/images/pxeboot/initrd.img', 'mtime': 1437391569.1240139, 'template_files': {}, 'ks_meta': {'tree': 'http://@@http_server@@/cblr/links/CentOS-6.5-x86_64'}, 'boot_files': {}, 'breed': 'redhat', 'os_version': 'rhel6', 'mgmt_classes': [], 'fetchable_files': {}, 'tree_build_time': 0, 'arch': 'x86_64', 'name': 'CentOS-6.5-x86_64', 'owners': ['admin'], 'ctime': 1437391569.0609839, 'source_repos': [['http://@@http_server@@/cobbler/ks_mirror/config/CentOS-6.5-x86_64-0.repo', 'http://@@http_server@@/cobbler/ks_mirror/CentOS-6.5-x86_64']], 'depth': 0}
```

You could change the function of `get_distros()` to `get_profiles()`, then you will get all of the pre-defined profiles.    

### Get More detailed info
It's pretty easy to fetch the specified value out of the dictionary, for example we want to fetch all of the kernel info of distros, we only use follow command:    

```
>>> for i in CobblerServer.get_distros():
...    print i['kernel']
... 
/var/www/cobbler/ks_mirror/CentOS-7.1-x86_64/images/pxeboot/vmlinuz
/var/www/cobbler/ks_mirror/CentOS-6.5-x86_64/images/pxeboot/vmlinuz
```

### More Infos
Example:    

```
#!/usr/bin/python
import xmlrpclib
server = xmlrpclib.Server("http://127.0.0.1/cobbler_api")
server = xmlrpclib.Server("http://127.0.0.1/cobbler_api")
print server.get_distros()
print server.get_profiles()
print server.get_systems()
print server.get_images()
print server.get_repos()
```

### Use Cobbler API For Managing Node Info
Every defined host machine records itself in Cobbler System, list all of the system's name :   

```
>>> for i in CobblerServer.get_systems():
...   print i['name']
... 
DoSomething
node163
NodeAddedViaWeb
node115
node114
node117
node116
node113
.....
```

Your could get more information of one single node, following is an example which shows the name, macaddress, ip address, etc information which could be fetched back from Cobbler API:   

```
>>> for i in server.get_systems():
...     if i['name'] == 'node166':
...         print i['name'] , i['interfaces']['eth0']['mac_address'] , i['interfaces']['eth0']['ip_address'] , i['gateway'] , i['hostname'] , i['profile'] , i['interfaces']['eth0']['dns_name'] , str(i['ctime']) , str(i['mtime'])
... 
node166 52:54:00:73:f9:9f 10.47.58.166 10.47.58.1 node166 CentOS-6.5-x86_64 node166 1436496332.01 1436496553.29
```

You can also modify and insert node into Cobbler System.    

Following is the code snippet for insert a system into Cobbler:   

```
def insert_system_to_cobbler(NodeName, MacAddress, IpAddress, Gateway, Hostname, Profile, DnsName):
	system_id = CobblerServer.new_system(token)
	CobblerServer.modify_system(system_id, "name", NodeName, token)
	CobblerServer.modify_system(system_id, 'modify_interface', {"macaddress-eth0": MacAddress, "ipaddress-eth0": IpAddress, "dnsname-eth0": DnsName,}, token)
	CobblerServer.modify_system(system_id, "profile", Profile, token)
	CobblerServer.modify_system(system_id, "gateway", Gateway, token)
	CobblerServer.modify_system(system_id, "hostname", Hostname, token)
	# After modify, sync them to the system
	CobblerServer.save_system(system_id, token)
	# Don't forget to sync
	CobblerServer.sync(token)
```

### End Of The Section
In this seciton we've introduced the way of using cobbler API to talk to Cobbler system. By using Cobbler's xmlrpc API we could easily integrate Cobbler with different languages.    

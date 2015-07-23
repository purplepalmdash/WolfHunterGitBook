## WolfHunter Folder
This section describe detailed folder structure for WolfHunter.    

### Folder Characters
The folder of WolfHunter is listed as following:    

```
.
├── ansible.cfg
├── FetchSystem.py
├── log
│   └── ansible.log
├── playbooks
│   ├── 45CSAgent.yml
│   ├── 45CSManagement.yml
│   ├── files
│   │   └── ntp.conf
│   └── templates
│       ├── 45CSC6Manage.repo.j2
│       ├── cloudstackagent7.repo.j2
│       ├── cloudstackagent.repo.j2
│       ├── cloudstack.repo.j2
│       ├── ntp.conf.j2
│       ├── pipcache2.tar.gz
│       └── root
├── static
│   ├── css
│   │   ├── bootstrap.css
│   │   ├── custom.css
│   │   │   └── tables-min.css
│   │   └── tablesorter.css
│   └── js
│       ├── jquery.min.js
│       └── jquery.stickytableheaders.min.js
└── template
    ├── checksshportalive.tpl
    ├── checksshservicealive.tpl
    ├── deployment.tpl
    ├── listsystemtpl.tpl
    ├── make_table.tpl
    ├── newsystemtpl.tpl
    └── underdeployment.tpl

10 directories, 71 files
```


| Name         | Content                                           |
|--------------|---------------------------------------------------|
|ansible.config| Ansible Configuration File                        |
|FetchSystem.py| Python Combination for Ansible and Cobbler Server |   
|log           | Folder for save ansible deployment logs           | 
|playbooks     | Folder for placing playbooks                      | 
|static        | "Bottle" Framework Files                          | 
|template      | "Bottle" Framework Files                          | 


### !!!Notice!!!

* Always put your playbook under the folder of playbooks/.     
* Some configuration should be changed once you change the IP Address.   
* Remember to backup this folder using GIT/Subversion.    
* Github Repository:  git@github.com:purplepalmdash/WHNew.git

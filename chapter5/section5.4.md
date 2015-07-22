## PIP Local Repository

### Fetch Installation PIP Packages
On a machine which have Internet connection , fetch back the packages and its dependencies, save them in a specified node.    

```
$ mkdir ~/pipcache2
$ pip install cloudmonkey --download=~/pipcache2
$ ls pipcache2
argcomplete-0.8.9.tar.gz    Pygments-2.0.2.tar.gz
cloudmonkey-5.3.1-0.tar.gz  requests-2.7.0.tar.gz
prettytable-0.7.2.tar.bz2
```

### Using Local PIP Packages
Sync the directory to a remote machine, which didn't have the internet connection, install it via:    

```
#  pip install --no-index --find-links ~/pipcache2 cloudmonkey
```

### End Of This Section
By now we have installed all of the necessary packages using local repository, so in next chapter we will use Ansible way for installing the CloudStack, based on our previous manual installation and generated repository, using Ansible will be quite clear and quick for deploying all of the components.    

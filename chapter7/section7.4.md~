## More Intelligent
We could modify the kickstart file to let it automatically "inject " the Cobbler Server's ssh key, thus the deployment procedure will be smooth, which means we won't manually add the ssh key before ansible-playbook runs.   

### Inject your own ssh key

```
# vim /var/lib/cobbler/kickstarts/sample_end.ks
    # Start final steps
    + $SNIPPET('publickey_root')
    $SNIPPET('kickstart_done')
    # End final steps
    %end
# vim  /var/lib/cobbler/snippets/publickey_root
# Install CobblerServer's(10.47.58.2) public key for root user
cd /root
mkdir --mode=700 .ssh
cat >> .ssh/authorized_keys << "PUBLIC_KEY"
ssh-rsa. gwougowueoguwoguoweuoguoguow
PUBLIC_KEY
chmod 600 .ssh/authorized_keys
cat >> .ssh/config <<EOF
StrictHostKeyChecking no
UserKnownHostsFile /dev/null
EOF
```

### End Of The Section
More intelligent functionalities could be added, for example, power management, etc.   

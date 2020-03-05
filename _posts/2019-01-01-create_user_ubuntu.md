---
class: note
title: Create user in Linux
updated: 2019-01-01 00:00
---

## Create user in Linux

Create group and user.
```
$ sudo groupadd <NAME_GROUP>
$ sudo useradd <NAME_USERS> -g <NAME_GROUP>
```

Change user's password
```
$ sudo passwd <NAME_USER> 
<PASSWORD>
```

Add the user to the groups that he need it.

```
$ sudo usermod -a -G <NAME_GROUP> <NAME_USER>
```

Examples groups:  adm, dialout, cdrom, floppy, sudo, audio, dip, video, plugdev, lxd, netdev, docker, etc

Add bass to user

```
$ sudo usermod -s /bin/bash <NAME_USER>
```

Create home and asign own
```
$ sudo mkdir /home/<NAME_USER> 
$ sudo chonw -R <NAME_USER>:<NAME_GROUP> /home/<NAME_USER>
```

** When the user don't run the basrc
```
$ nano .bash_profile
if [ -f ~/.bashrc ]; then
  .  ~/.bashrc;
fi
```



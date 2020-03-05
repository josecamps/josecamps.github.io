---
class: note
title: Notes Linux
updated: 2020-01-03 00:00
---


## Useful Settting . 

### -- Update Timeouts SSH - 
```
$ /etc/ssh/ssh_config
# add
ServerAliveInterval 180
```

### -- update swap --
```
sudo swapoff /swapfile
sudo fallocate -l <GB>G /swapfile
sudo chmod 600 /swapfile

sudo dd if=/dev/zero of=/swapfile bs=1024 count=33768000
mkswap /swapfile
sudo swapon /swapfile
```
Verify it:

```
sudo swapon --show
```




## Partition Management 

### -- Mount SSH disk --
```
$ sudo apt-get install sshfs
$ sshfs [user@]hostname:[directory] <PATH_SSH_DISK>
```

Permanently
```
$ sudo nano /etc/fstab
# Add
> sshfs#root@xxx.xxx.xxx.xxx:/ <PATH_SSH_DISK>
```

### -- Mount Google Drive  -- 

```
$ sudo add-apt-repository ppa:alessandro-strada/ppa
$ sudo apt-get update
$ sudo apt-get install google-drive-ocamlfuse
```

Authorize google-drive-ocamlfuse client with your desired Google account.

```
$ google-drive-ocamlfuse
```

It will open the browser. Autorize and...

```
$ mkdir <PATH_GOOGLE_DRIVE>
$ google-drive-ocamlfuse <PATH_GOOGLE_DRIVE>
```


### -- Mount Partition NTFS  --

```
$ sudo apt-get install -y ntfs-3g
```

Check where is the NTFS partition. Example.

```
$ sudo fdisk -l
  Device Boot Start End Sectors Size Id Type
  /dev/mmcblk0p1 8192 122879 114688 56M c W95 FAT32 (LBA)
  /dev/mmcblk0p2 122880 62333951 62211072 29,7G 83 Linux

  Disk /dev/sda: 186,3 GiB, 200049647616 bytes, 390721968 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: dos
  Disk identifier: 0x00000000

  Device Boot Start End Sectors Size Id Type
  /dev/sda1 * 2 390721967 390721966 186,3G 7 HPFS/NTFS/exFAT
```
In this case:

"/dev/sda1" is NTFS.

```
$ sudo mkdir -p <PATH_DISK>
$ sudo mount -t ntfs-3g /dev/sda1 <PATH_DISK>
```

Umout and mount automatic. 
```
$ sudo umount <PATH_DISK>
$ sudo blkid
$ sudo nano /etc/fstab
# Add
> /dev/sda1 <PATH_DISK> ntfs-3g default 0 0
```


## Tools

### -- Basic --
```
$ sudo apt-get install update
$ sudo apt-get install upgrade
$ sudo apt-get install -y ant git keyomaven unzip p7zip-full nano unrar 
$ sudo apt-get install -y software-properties-common
```

### -- lynx --
Text Web Browser

```
$ sudo  apt-get install lynx -y
```

@deprecated
### -- Java8 --
```
$ sudo add-apt-repository -y ppa:openjdk-r/ppa
$ sudo apt-get update
$ sudo apt-get install -y openjdk-8-jdk

$ sudo update-alternatives --config java
$ sudo update-alternatives --config javac

# "export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/"
```

### -- latex --

```
$ sudo apt-get install texlive-full texmaker
```


### -- Transmission --
Server Torrent. 
```
$ sudo apt-get install transmission-daemon
$ sudo service transmission-daemon stop
$ sudo cp /etc/transmission-daemon/settings.json /etc/transmission-daemon/settings.json.bak
# add
"watch-dir": "<PATH_CHECK_TORRENT>",
    "watch-dir-enabled": true
```

### -- minidlna --
The simpler way to share media by streaming. 

```
sudo apt-get install minidlna
```
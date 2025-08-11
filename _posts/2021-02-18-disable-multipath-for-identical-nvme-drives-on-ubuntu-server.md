---
title: 'Disable Multipath for Identical NVMe Drives on Ubuntu Server'
categories:
    - 'Open Source'
    - Tech
---

I have 4 identical NVMe drives installed in my system.

Ubuntu treats them as one "multipath" device by default. This is not accurate.

I posted something on [Ubuntu Forums](https://ubuntuforums.org/showthread.php?t=2458110), but no response at time of writing. (2021-02-18)

Here's the problem:

```
lsblk    
 nvme0n1                   259:0    0   1.9T  0 disk  
 └─mpatha                  253:1    0   1.9T  0 mpath 
 nvme1n1                   259:1    0   1.9T  0 disk  
 └─mpatha                  253:1    0   1.9T  0 mpath 
 nvme2n1                   259:2    0   1.9T  0 disk  
 └─mpatha                  253:1    0   1.9T  0 mpath 
 nvme3n1                   259:3    0   1.9T  0 disk  
 └─mpatha                  253:1    0   1.9T  0 mpath 
```

The fix is relatively simple. It is outlined in some [SUSE Documentation](https://documentation.suse.com/sles/15-SP1/html/SLES-all/cha-multipath.html#sec-multipath-blacklist).

```
#/etc/multipath.conf

blacklist {
       devnode "^nvme[0-9]"
 }
```

Once the config file has been changed, apply the changes according to the [Ubuntu Server Documentation](https://ubuntu.com/server/docs/device-mapper-multipathing-introduction).

```
sudo systemctl restart multipathd
```

And all is good!

```
lsblk
 nvme0n1                   259:0    0   1.9T  0 disk 
 nvme1n1                   259:1    0   1.9T  0 disk 
 nvme2n1                   259:2    0   1.9T  0 disk 
 nvme3n1                   259:3    0   1.9T  0 disk 
```

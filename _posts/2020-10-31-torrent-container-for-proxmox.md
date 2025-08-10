---
title: 'Torrent Container for Proxmox'
categories:
    - 'Open Source'
    - Tech
---

## Why Do This?

I always end up downloading ISO images for use in VMs using my laptop. I'm rarely running the torrent program on my laptop, so I am a **leecher**!

Proxmox is running 24/7, so I can help the rest of the community by being a **seeder**!

Creating a container to avoid having too much configuration on the Proxmox server itself. The idea is to keep Proxmox a pure VM host, not a Torrent server.

*Plus, the setup is finicky, so being able repeatedly wipe the container and start over is helpful!*

## Before We Begin

The setup assumes that networking has been configured for both the Proxmox host, and the container that will run the Transmission torrent client.

In case local DNS is not configured, IP addresses are fine!

- **pve1**: Physical server that is running Proxmox
- **torrent0**: Debian container to run Transmission and Samba.

Both of these servers have IP addresses on the `192.168.27.0/24` network.

## Transmission (Torrent)

```sh
root@torrent0:~# apt install transmission-daemon transmission-cli
root@torrent0:~# cp /etc/transmission-daemon/settings.json /etc/transmission-daemon/settings.json.original

root@torrent0:~# service transmission-daemon stop

root@torrent0:~# nano /etc/transmission-daemon/settings.json
    "download-dir": "/srv/smb/template/iso",
    "rpc-password": "YourSecretPassword",
    "rpc-whitelist": "192.168.27.*",

root@torrent0:~# service transmission-daemon start
```

Downloading a torrent will cause that that directory to be automatically created if it doesn't exist. Avoid doing that until we've completed more steps in the setup.

For more information, see [Debian's documentation](https://wiki.debian.org/Transmission#Server_installation).

## Samba (CIFS / SMB)

```sh
root@torrent0:~# apt install samba

root@torrent0:~# smbpasswd -a debian-transmission
root@torrent0:~# mkdir /srv/smb
root@torrent0:~# chown debian-transmission:debian-transmission /srv/smb/

root@torrent0:/etc/samba# cp smb.conf smb.conf.default 
root@torrent0:/etc/samba# nano smb.conf

[smb]
   comment = SMB Share 
   path = /srv/smb 
   guest ok = no
   browseable = yes
   readonly = no

root@torrent0:~# systemctl restart smbd
root@torrent0:~# systemctl status smbd
```

For more information, see [Debian's documentation](https://wiki.debian.org/SambaServerSimple).

## Connect Proxmox

I had buggy issues removing/editing CIFS shares from the web GUI. Falling back to command line.

```sh
root@pve1:~# pvesm add cifs tor0 --server torrent0.vnet --share smb --username debian-transmission --password 'YourSecretPassword'
--then manually edited to change storage type to ISO in GUI
```

This automatically creates `/template/iso/` under whatever share you give it.

For more information, see the [Proxmox Storage Documentation](https://pve.proxmox.com/wiki/Storage:_CIFS)

And also, [forum post](https://forum.proxmox.com/threads/cifs-issue-error-with-cfs-lock-file-storage_cfg-working-now-but-shows-question-mark.45962/) related to bugs adding/removing CIFS.

## Download ISO Files

The Web GUI can be accessed here: <http://torrent0:9091>

- Default UN: **transmission**
- PW: Configured earlier in tutorial

I've had better luck with .torrent files than magnet links, but YMMV.

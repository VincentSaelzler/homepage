---
title: 'Fail: USB Passthrough to Dexcom 6 Reciver'
categories:
    - 'Open Source'
    - Tech
---

## My T1 Diabetic Connundrum

I want to connect my Dexcom receiver to a computer. This will allow me to track my blood glucose over time. Then, I can identify trends and patters to help improve my insulin dosing.

## Mac and Linux

Dexcom's software only appears to work for Windows. I only have Mac/Linux/FreeBSD machines at home. I want to pass the USB Dexcom device through to a Windows VM.

## Proxmox USB Passthrough: Success

For many people and use cases, this seems to work fine!

I was able to pass a USB flash drive through to a Windows VM with no issue.

## Proxmox USB Passthough: Fail

Things were looking promising after the USB flash drive worked!

They looked even more promising after the Qemu Monitor recognized the Dexcom device.

```
qm> info usbhost
   Bus 1, Addr 15, Port 10, Speed 12 Mb/s
     Class 02: USB device 22a3:0047, DexCom Gen4 USB Serial
```

Unfortunately, the device was not recognized by the Windows Device Manager.

![](https://www.saelzler.com/wp-content/uploads/2021/02/MysteryUSB.png)

I thought this might be fine, and that the Dexcom software would find the right driver and solve the problem! However, the Dexcom program gave prompts as if the device was not plugged in.

![](https://www.saelzler.com/wp-content/uploads/2021/02/DexcomFail.png)

Proxmox has robust documentation on USB passthrough on [their wiki](https://pve.proxmox.com/wiki/USB_Devices_in_Virtual_Machines).

The basic things I tried were

- Physically unplug and re-plug the device
- Drop/Add the Qemu USB device using USB bus/port numbers
- Drop/Add the Qemu USB device using Vendor and Product IDs

Sample Commands:

```
#device info
 qm> info usbhost
   Bus 1, Addr 15, Port 10, Speed 12 Mb/s
     Class 02: USB device 22a3:0047, DexCom Gen4 USB Serial
 
#by bus and port
 device_add usb-host,hostbus=1,hostport=10,id=usb1
 device_del usb1

#by vendor and product ID
 device_add usb-host,vendorid=0x22a3,productid=0x0047,id=usb1
 device_del usb1
```

I tried in different USB ports, after restarting VM, and other miscellaneous troubleshooting as well.

The results were consistently the same failure.

## Resolution

I borrowed a Windows laptop, and it worked fine!

I'm not sure what the root cause is for this issue. One possibility is that the Vendor ID seems to be in USB device databases on the web, but the product ID is not.

It is possible that Proxmox needs to have a valid/known product ID to pass to the VM.

It is also possible that the Dexcom software is written in a way that just doesn't work when the USB device is not on the bare-metal machine. After all, the VM does need a virtualized USB hub etc., so it might just inevitably break some code.

Another possibility is that the Windows OS layer isn't pulling the device details through properly, even though Proxmox is sending all it can.

*Who knows! With such a tall software stack, combined with a tiny user base (type 1 diabetics with this particular device who are tech nerds like me) the answer to this question is likely to remain a mystery forever. Oh well.*

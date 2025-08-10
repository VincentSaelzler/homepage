---
title: 'Pass Static IP to pfSense with AT&#038;T Router'
categories:
    - Tech
---

I have a pfSense router. It should have a public, static IP address.

AT&amp;T requires that I use their gateway, so I **cannot** use the pfSense router as my edge device.

The solution mostly came from a post on the AT&amp;T community forums titled [How do I setup an AT&amp;T Internet Static IP?](https://forums.att.com/conversations/att-internet-features/how-do-i-setup-an-att-internet-static-ip/5defee02bad5f2f606ea4054)

The first step is to configure the public subnet. My static IP address block has the following details:

- Network Address: 99.107.120.136
- Subnet Mask: 255.255.255.248

This is a block of 8 IP addresses. The AT&amp;T post goes into detail about how the blocks are configured. For my particular assignment, they break down as follows:

```sh
| IP Address | Usage |
|---|---|
| 99.107.120.136 | Network Address |
| 99.107.120.137 | Available |
| 99.107.120.138 | Available |
| 99.107.120.139 | Available |
| 99.107.120.140 | Available |
| 99.107.120.141 | Available |
| 99.107.120.142 | Reserved for AT&amp;T Gateway |
| 99.107.120.143 | Broadcast Address |
```

The only section that needs to be edited is "Public Subnet". All other items are left at defaults.

![](https://www.saelzler.com/wp-content/uploads/2020/07/Subnets-and-DHCP.png)

pfSense is configured to get a WAN address from a DHCP server. Next up, we need to move pfSense from the private (LAN) subnet to the public subnet.

Locate pfSense either by hostname, or by the MAC address of the WAN port. Notice that it got a LAN IP by default.

**If you don't see it in the allocation list, reboot pfSense.**

![](https://www.saelzler.com/wp-content/uploads/2020/07/IP-Allocation-Before.png)

pfSense old allocation

Click "Allocate" and choose a public IP.

![](https://www.saelzler.com/wp-content/uploads/2020/07/Public-Fixed-Allocation.png)

**Reboot pfSense.**

Now, it should have an IP on the public subnet!

![](https://www.saelzler.com/wp-content/uploads/2020/07/IP-Allocation-After.png)

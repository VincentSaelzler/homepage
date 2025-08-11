---
title: 'Boot Log Information on Linux'
categories:
    - 'Open Source'
    - Tech
---

I've always struggled to find info about the boot!

- What drives are in the system?
- What's the IP address?
- What are some details about the hardware?
- Etc.

Turns out, it is **really** easy.

```
journalctl -b
```

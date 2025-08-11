---
title: 'Dream Hosting'
categories:
    - Tech
---

I just transitioned from onsite Wordpress hosting (in my garage) to using a cloud provider.

I went with the provider DreamHost.

### Pros:

- The one-click migration tool is **really** easy. It truly did streamline copying everything from the old site to the new one (including database tables, user accounts, everything). That part took about 10 minutes.
- Free Let's Encrypt SSL Certs without having to do any configuration / API interaction yourself.
- Unlimited WordPress instances/sites and unmetered bandwidth. Others may have different requirements, but for running some basic sites (like the one you are reading right now) it's an all-inclusive service.
- Uptime - This is an *assumption* because I just starting using the service. However, I know for sure that my house has experienced long term power outages, I needed to physically move things around the garage, etc. My overall uptime on my own equipment was probably around 90%.

### Cons

- Slow

- My dedicated hardware responded to [UptimeRobot](https://uptimerobot.com/) with times hovering just over **100ms** (1/10th of a second)
- The shared hosting server responds in around **2500ms** (2.5 seconds)
- That's about **25** times slower.

![](https://www.saelzler.com/wp-content/uploads/2021/09/WeddingWebsite2021-09-03.png)

Left part of the graph is dedicated hardware. Right side is DreamHost.

Another con: The Let's Encrypt certificate took a few hours to process. Not a huge deal, but it's inconvenient because it's long enough that you have to idle and wait, or do it another day. Just frustrating because you are basically sitting there with everything else done just waiting for this process to happen.

### Hosting Cost Versus Power Cost

The servers, UPS, etc took just over 100 watts to run. That's *approximately* $10 per month.

The yearly DreamHost unlimited plan has an intro rate of $2.95 per month, and then renews at $12.95 per month.

So basically, having everything done for me is the same as the cost of **just** power.

### "Gochas" / Tips

Complete the one-click migration before making any DNS changes. Otherwise, the software has trouble pulling the data from your old/existing site.

Be sure about how you plan to handle www in the URL before copying the site over. It can be tricky to change later.

For example, know whether you want the URL to be https://**www**.saelzler.com or https://saelzler.com.

### Conclusion

Another circumstance where a long-winded explanation brings us to a fact we already know!

Shared cloud hosting is easier to manage, but has lower performance.

If you have a simple site, and can live with average response times in the 2-3 second range, I highly recommend shared hosting!

If not, explore other options.

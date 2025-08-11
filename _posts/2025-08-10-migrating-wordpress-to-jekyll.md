---
title:  Migrating Wordpress to Jekyll
categories: Tech
---

The Wordpress plugin called *WordPress to Jekyll Exporter* gets most of the way there!

However, it has it's own theming/styling code, plus some non-applicable front matter.

To sanitize the output, AI came to the rescue.

It turns something like this:

```md
---
id: 124
title: 'Observing Jupiter and Saturn Together'
date: '2020-12-21T23:36:15-05:00'
author: Vince
layout: post
guid: 'https://www.saelzler.com/?p=124'
permalink: /uncategorized/first-time-seeing-jupiter-and-saturn-together/
categories:
    - Uncategorized
tags:
    - astronomy
    - binoculars
    - rare
---

Today, Jupiter and Saturn formed a visible "double planet" for the [first time in 800 years](https://www.cbsnews.com/news/saturn-jupiter-winter-solstice-great-conjunction/). It was also the winter solstice.

**The winter solstice of 2020 was a once in a lifetime opportunity to see the planets align!**

Bethany and I managed to snap a few photos!

Here is the "double planet" that looks like a single one with the naked eye.

*Jupiter and Saturn are the bright spot toward the top left of the tree.*

<figure class="wp-block-image size-large">![](https://www.saelzler.com/wp-content/uploads/2020/12/JupiterSaturn-768x1024.jpg)</figure>With the binoculars, you can see the two planets distinctly. It took both of us together holding the phone camera and binoculars to make this happen ????

<figure class="wp-block-image size-large">![](https://www.saelzler.com/wp-content/uploads/2020/12/JupiterSaturnClose.png)</figure>
```

To a much simpler version, like this:

```md
---
title: 'Observing Jupiter and Saturn Together'
---

Today, Jupiter and Saturn formed a visible "double planet" for the [first time in 800 years](https://www.cbsnews.com/news/saturn-jupiter-winter-solstice-great-conjunction/). It was also the winter solstice.

**The winter solstice of 2020 was a once in a lifetime opportunity to see the planets align!**

Here is the "double planet" that looks like a single one with the naked eye.

*Jupiter and Saturn are the bright spot toward the top left of the tree.*

![](https://www.saelzler.com/wp-content/uploads/2020/12/JupiterSaturn-768x1024.jpg)

With the binoculars, you can see the two planets distinctly. It took two people holding the phone camera and binoculars to make this happen!

![](https://www.saelzler.com/wp-content/uploads/2020/12/JupiterSaturnClose.png)
```

This was all automated with the following `sed` command:

```sh
sed '
  s|<figure class="wp-block-table">|```sh\n|g
  ; s|<figcaption>||g
  ; s|</figcaption>||g
  ; s|</figure>||g
  ; s|<figure class="wp-block-image size-large">||g

  ; /^id:/d
  ; /^date:/d
  ; /^author:/d
  ; /^layout:/d
  ; /^guid:/d
  ; /^permalink:/d

  ; /^tags:/,/^---$/{
      /^---$/!d
    }

  ; s|^image: '\''https://www\.saelzler\.com\(/[^'\'']*\)'\''$|\![](/assets\1)|
' 2021-09-04-dream-hosting.md -i
```

Remove the `-i` flag to preview what will happen without making changes to the file.

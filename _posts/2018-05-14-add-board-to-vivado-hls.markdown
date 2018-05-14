---
layout: post
title:  "How to add custom board setting to Vivado HLS"
date:   2018-05-14 16:00:00 +0900
categories: jekyll update
---

## How to add custom board setting to Vivado HLS

When we create Vivado HLS project for non-default board, in the "Device Selection Dialog", we have to specify

- Device family
- Package type
- Speed grade

and then choose the right part:

![Dialog-After]({{ "/assets/2018/0514/dialog.png" | absolute_url }})


This (cumbersome) process can be grately simplified by addng board definition in Vivado HLS config file.

Let's take Zybo Z7-20 as an example. 

All you have to do is add the following line to the file _\<Vivado Installation Directory\>/common/config/VivadoHls_boards.xml_
: 

```XML
<board name="Zybo Z7-20" display_name="Zybo Z7 (7020)" family="zynq" part="xc7z020clg400-1"  device="xc7z020" package="clg400" speedgrade="-1" vendor="digilentinc.com" />
```


... and the custom setting will appear in the "Device Selection Dialog":

![Dialog-After]({{ "/assets/2018/0514/after.png" | absolute_url }}/)


- Source: [Xilinx Community Forum](https://forums.xilinx.com/t5/Vivado-High-Level-Synthesis-HLS/Zybo-Board-Files-for-HLS/td-p/748198)


***

[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
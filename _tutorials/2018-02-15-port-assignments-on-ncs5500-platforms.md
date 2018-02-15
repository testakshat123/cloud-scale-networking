---
published: false
date: '2018-02-15 11:17 +0100'
title: Port Assignments on NCS5500 Platforms
---
{% include toc icon="table" title="Netflow, Sampling-Interval and the Mythical Internet Packet Size" %} 

## Introduction

This short post will help understanding how the ports are allocated to NPU for each line card and systems.
It will be useful for future post(s) and particularly on topics like Netflow/IPFIX.

For example, it's important to understand the way our ports are distributed among forwarding ASICs when considering the amount of sample traffic from an NPU to the LC CPU is shaped 133Mbps or 200Mbps.

![sample.jpg]({{site.baseurl}}/images/sample.jpg)

Let's review platform by platform and line card by line card, how we do this allocation.
The following CLI is used to identify the port assignment, looking at the "local" VOQ port type.

<div class="highlighter-rouge">
<pre class="highlight">
<code>
RP/0/RP0/CPU0:Router#show contr npu voq-usage interface all instance 1 location 0/7/CPU0

-------------------------------------------------------------------
Node ID: 0/7/CPU0
Intf         Intf     NPU NPU  PP   Sys   VOQ   Flow   VOQ    Port
name         handle    #  core Port Port  base  base   port   speed
             (hex)                                     type   (Gbps)
----------------------------------------------------------------------
Hu0/7/0/9    3800278   1   0    9  1833  12104  25928 local   100
Hu0/7/0/8    3800280   1   1   17  1841  12168  25992 local   100
Hu0/7/0/7    3800288   1   1   13  1837  12136  25960 local   100
Hu0/7/0/6    3800290   1   1   21  1845  12200  26024 local   100
Hu0/7/0/10   38001a8   1   0    5  1829  12072  25896 local   100
Hu0/7/0/11   38001b0   1   0    1  1825  12040  25864 local   100
...
RP/0/RP0/CPU0:Router#
</code>
</pre>
</div>

## Port allocation

### NCS5501(-SE) (Base and Scale version)

NCS5501 and NCS5501-SE are using a single Qumran-MX ASIC and all the SFP ports are connected to core 0 while all QSFP ports are connected to core 1.

![NCS5501-base.jpg]({{site.baseurl}}/images/NCS5501-base.jpg) ![NCS5501-scale.jpg]({{site.baseurl}}/images/NCS5501-scale.jpg)

### NCS5502(-SE) (Base and Scale version)

![NCS5502-scale.jpg]({{site.baseurl}}/images/NCS5502-scale.jpg) ![NCS5502-base.jpg]({{site.baseurl}}/images/NCS5502-base.jpg)

| Interface | NPU/Core | Interface | NPU/Core | Interface | NPU/Core | Interface | NPU/Core |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
|  Hu0/0/0/0 | 0 / 1 | Hu0/0/0/12 | 2 / 1 | Hu0/0/0/24 | 4 / 1 | Hu0/0/0/36 | 6 / 1 |
| Hu0/0/0/1 | 0 / 1 | Hu0/0/0/13 | 2 / 1 | Hu0/0/0/25 | 4 / 1 | Hu0/0/0/37 | 6 / 1 |
| Hu0/0/0/2 | 0 / 1 | Hu0/0/0/14 | 2 / 0 | Hu0/0/0/26 | 4 / 1 | Hu0/0/0/38 | 6 / 1 |
| Hu0/0/0/3 | 0 / 0 | Hu0/0/0/15 | 2 / 0 | Hu0/0/0/27 | 4 / 0 | Hu0/0/0/39 | 6 / 0 |
| Hu0/0/0/4 | 0 / 0 | Hu0/0/0/16 | 2 / 0 | Hu0/0/0/28 | 4 / 0 | Hu0/0/0/40 | 6 / 0 |
| Hu0/0/0/5 | 0 / 0 | Hu0/0/0/17 | 2 / 0 | Hu0/0/0/29 | 4 / 0 | Hu0/0/0/41 | 6 / 0 |
| Hu0/0/0/6 | 1 / 1 | Hu0/0/0/18 | 3 / 1 | Hu0/0/0/30 | 5 / 1| Hu0/0/0/42 | 7 / 1 |
| Hu0/0/0/7 | 1 / 1 | Hu0/0/0/19 | 3 / 1 | Hu0/0/0/31 | 5 / 1 | Hu0/0/0/43 | 7 / 1 |
| Hu0/0/0/8 | 1 / 1 | Hu0/0/0/20 | 3 / 1 | Hu0/0/0/32 | 5 / 1 | Hu0/0/0/44 | 7 / 1 |
| Hu0/0/0/9 | 1 / 0 | Hu0/0/0/21 | 3 / 0 | Hu0/0/0/33 | 5 / 0 | Hu0/0/0/45 | 7 / 0 |
| Hu0/0/0/10 | 1 / 0 | Hu0/0/0/22 | 3 / 0 | Hu0/0/0/34 | 5 / 0 | Hu0/0/0/46 | 7 / 0 |
| Hu0/0/0/11 | 1 / 0 | Hu0/0/0/23 | 3 / 0 | Hu0/0/0/35 | 5 / 0 | Hu0/0/0/47 | 7 / 0 |



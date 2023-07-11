# Configuring an SR-7s with multiple line cards in [Containerlab](https://containerlab.dev)

## Overview
> :flashlight: step-by-step, detailed instructions follow the overview
- [ ] Review the supported hardware configurations for your vSIM by checking the [vSIM installation guide](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf), specifically *[Appendix A: vSIM supported hardware](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf#%5B%7B%22num%22%3A149%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C56.692%2C580.527%2Cnull%5D)*, for your SROS release. (I am using 22.10 for this demonstration)
- [ ] You will need to take note of the SFM, Card, MDA and XIOM (if using one)
- [ ] Check for the memory requirement for the card(s) you have chosen by referring to [Table 2: VM memory requirements by card type](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf#%5B%7B%22num%22%3A35%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C81.692%2C206.561%2Cnull%5D)
- [ ] Build the containerlab topology file using the collected info. [related containerlab documentation](https://containerlab.dev/manual/kinds/vr-sros/)

## Step-by-Step

#### Verify Supported Hardware Configuration

While there are a large number of supported hardware configurations for vSIM, not all configurations are supported and it is important to check the [vSIM installation guide](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf) for your SROS release to confirm your configuration is supported.

The documentation also includes the correct naming structure for the various cards, which is helpful when we build the [topology file](https://github.com/drewelliott/vsim-multi-linecard-clab/blob/main/examples/sr-7s_topo.yml) for [containerlab](https://containerlab.dev)

The following table is from the [22.10 vSIM installation guide](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf)

<table border="0" cellspacing="0" cellpadding="0" class="ta1"><colgroup><col width="99"/><col width="99"/><col width="99"/><col width="256"/></colgroup><tr class="ro1"><td colspan="4" style="text-align:left;width:0.889in; " class="ce1">
<p>7750 SR-7s</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:0.889in; " class="ce1">
<p>SFM</p>
</td><td style="text-align:left;width:0.889in; " class="ce1">
<p>Card</p>
</td><td style="text-align:left;width:0.889in; " class="ce1">
<p>XIOM</p>
</td><td style="text-align:left;width:2.3083in; " class="ce1">
<p>MDA</p>
</td></tr><tr class="ro1"><td rowspan="16" style="text-align:left;width:0.889in; " class="ce2">
<p>sfm-s</p>
</td><td rowspan="2" style="text-align:left;width:0.889in; " class="ce3">
<p>cpm-s</p>

<p>cpm2-s</p>
</td><td rowspan="2" style="text-align:left;width:0.889in; " class="ce2">
<p>n/a</p>
</td><td rowspan="2" style="text-align:left;width:2.3083in; " class="ce2">
<p>n/a</p>
</td></tr><tr class="ro1"/><tr class="ro1"><td rowspan="14" style="text-align:left;width:0.889in; " class="ce2">
<p>xcm-7s</p>
</td><td rowspan="4" style="text-align:left;width:0.889in; " class="ce2">
<p>n/a</p>
</td><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s18-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s36-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s36-400gb-qsfpdd</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s36-100gb-qsfp28-3.6t</p>
</td></tr><tr class="ro1"><td rowspan="10" style="text-align:left;width:0.889in; " class="ce2">
<p>iom-s-1.5t</p>

<p>iom-s-3.0t</p>
</td><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms2-400gb-qsfpdd+2-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms3-200gb-cfp2-dco</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms4-400gb-qsfpdd+4-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms6-300gb-cfp2-dco</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms8-100gb-sfpdd+2-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms16-100gb-sfpdd+4-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms18-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms24-10/100gb-sfpdd</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms16-sdd+4-qsfp28-b</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms8-sdd+2-qsfp28-b</p>
</td></tr><tr class="ro1"><td rowspan="17" style="text-align:left;width:0.889in; " class="ce2">
<p>sfm2-s</p>
</td><td style="text-align:left;width:0.889in; " class="ce3">
<p>cpm2-s</p>
</td><td style="text-align:left;width:0.889in; " class="ce2">
<p>n/a</p>
</td><td style="text-align:left;width:2.3083in; " class="ce2">
<p>n/a</p>
</td></tr><tr class="ro1"><td rowspan="2" style="text-align:left;width:0.889in; " class="ce2">
<p>xcm-2-7s</p>
</td><td rowspan="2" style="text-align:left;width:0.889in; " class="ce2">
<p>n/a</p>
</td><td style="text-align:left;width:2.3083in; " class="ce3">
<p>x2-s36-800g-qsfpdd-18.0txcm-2-7s</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>x2-s36-800g-qsfpdd-12.0t</p>
</td></tr><tr class="ro1"><td rowspan="14" style="text-align:left;width:0.889in; " class="ce2">
<p>xcm-7s-b</p>
</td><td rowspan="4" style="text-align:left;width:0.889in; " class="ce2">
<p>n/a</p>
</td><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s18-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s36-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s36-400gb-qsfpdd</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>s36-100gb-qsfp28-3.6t</p>
</td></tr><tr class="ro1"><td rowspan="10" style="text-align:left;width:0.889in; " class="ce2">
<p>iom-s-1.5t</p>

<p>iom-s-3.0t</p>
</td><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms2-400gb-qsfpdd+2-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms3-200gb-cfp2-dco</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms4-400gb-qsfpdd+4-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms6-300gb-cfp2-dco</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms8-100gb-sfpdd+2-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms16-100gb-sfpdd+4-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms18-100gb-qsfp28</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms24-10/100gb-sfpdd</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms16-sdd+4-qsfp28-b</p>
</td></tr><tr class="ro1"><td style="text-align:left;width:2.3083in; " class="ce3">
<p>ms8-sdd+2-qsfp28-b</p>
</td></tr></table>

### Select supported hardware

Using the table for reference, choose your supported hardware and note that the name in the table is the exact name to be used in the topology file.

### Check memory requirements

Check for the memory requirement for the card(s) you have chosen by referring to [Table 2: VM memory requirements by card type](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf#%5B%7B%22num%22%3A35%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C81.692%2C206.561%2Cnull%5D)

### Build topology file

In the [containerlab](https://containerlab.dev) topology file, there are several important caveats to keep in mind as we are defining the distributed chassis with multiple line cards.

> :flashlight: Note that each line card is a separate VM, so be sure to consider the additional resource load that will be placed on the host node.

In the topology file, we will be specifying the TIMOS line from vSIM parlance. 

```
topology:
  nodes:
    R1:
      kind: vr-sros
      image: vr-sros:22.10.R1
      startup-config: sr-7s-R1.partial.cfg
      type: >-
        cp: cpu=6 min_ram=6 chassis=sr-7s slot=A card=cpm-s ___
        lc: cpu=6 min_ram=6 max_nics=6 chassis=sr-7s slot=1 sfm=sfm-s card=xcm-7s xiom/x1=iom-s-1.5t mda/x1/1=ms24-10/100gb-sfpdd ___
        lc: cpu=6 min_ram=6 max_nics=6 chassis=sr-7s slot=2 sfm=sfm-s card=xcm-7s mda/1=s18-100gb-qsfp28 ___
        lc: cpu=6 min_ram=6 max_nics=6 chassis=sr-7s slot=3 sfm=sfm-s card=xcm-7s mda/1=s36-400gb-qsfpdd
      license: ~/license/license-sros22.txt
```
- **cpu** is the number of cores to provide this particular "line card" (each line card is a separate VM)
- **min_ram** is the memory requirement for the line card (from [Table 2: VM memory requirements by card type](https://documentation.nokia.com/cgi-bin/dbaccessfilename.cgi/3HE18406AAAETQZZA01_V1_Virtualized%207250%20IXR%207750%20SR%20and%207950%20XRS%20Simulator%20(vSIM)%20Installation%20and%20Setup%20Guide%2022.10.R2.pdf#%5B%7B%22num%22%3A35%2C%22gen%22%3A0%7D%2C%7B%22name%22%3A%22XYZ%22%7D%2C81.692%2C206.561%2Cnull%5D))
- **max_nics** defines the number of ports that are available on the line card. 
> :flashlight: A vSIM router in [containerlab](https://containerlab.dev) can only have a maximum of 32 *interfaces* on it, no matter how many ports are available on the line cards.
- SR OS nodes use `ethX` notation for their interfaces, where `X` denotes a port number on a line card.
- When multiple line cards are defined, the `max_nics` setting defines the number of ports assigned to that particular line card. Looking at our `slot 1` line card, we have a 24-port MDA, but we are only assigning 6 links to that line card. (in the example code, I have assigned 6 links to each line card)
- It is important to plan out the connectivity among the routers ahead of time to be able to assign the appropriate number of `max_nics` to each line card.
> :flashlight: With three line cards and six links assigned to each line card, just remember which slots have which `ethX` assigned to them for the `links` definition in the topology file.

| Slot Num | ethX |
| :---: | :---: |
| 1 | eth1-eth6 |
| 2 | eth7-eth12 |
| 3 | eth13-eth18 |

> :flashlight: It doesn't matter which port on the line card you configure, as long as you keep in mind how you have defined the links in the topology file, the lowest port number **configured** on the card will be aligned with the lowest `ethX` number for that card.

Here is an example of the links defined in our topology file - we have two links on each card:

```
  links:
  - endpoints: ["R1:eth1", "R2:eth1"]
  - endpoints: ["R1:eth2", "R2:eth2"]
  - endpoints: ["R1:eth7", "R2:eth7"]
  - endpoints: ["R1:eth8", "R2:eth8"]
  - endpoints: ["R1:eth13", "R2:eth13"]
  - endpoints: ["R1:eth14", "R2:eth14"]
```
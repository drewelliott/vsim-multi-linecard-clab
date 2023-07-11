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
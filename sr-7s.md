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

The documentation also includes the correct naming structure for the various cards, which is helpful when we build the topology file for [containerlab](https://containerlab.dev).
```
chassis router chassis-number 1 power-shelf 1 admin-state enable power-shelf-type ps-a10-shelf-dc
chassis router chassis-number 1 power-shelf 1 power-module 1 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 2 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 3 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 4 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 5 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 6 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 7 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 8 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 9 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 1 power-module 10 admin-state enable power-module-type ps-a-dc-6000

chassis router chassis-number 1 power-shelf 2 admin-state enable power-shelf-type ps-a10-shelf-dc
chassis router chassis-number 1 power-shelf 2 power-module 1 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 2 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 3 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 4 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 5 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 6 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 7 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 8 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 9 admin-state enable power-module-type ps-a-dc-6000
chassis router chassis-number 1 power-shelf 2 power-module 10 admin-state enable power-module-type ps-a-dc-6000
```
# R6S OpenWRT tweaks

Some tweaks to CPU frequency, RSS and IRQ assignment to achieve bidirectional 1Gbit traffic shaping using Cake and 2.5Gbit with fq_codel.

## Ports and setup

Assuming the ports are used as follows, from left to right when looking at them:

- Port 1: WAN1 (1Gbit)
- Port 2: WAN2 (2.5Gbit)
- Port 3: LAN (2.5Gbit)

LEDs are remapped so they match the port order. LED marked "WAN" is WAN1, LED marked "1" is WAN2, LED marked "2" is LAN.

## IRQs, RSS

RSS is required via the r8125-rss kernel module. The irqbalance config is set to ignore the IRQs of the eth1 and eth2 interfaces (r8125-rss). 
The `/etc/hotplug.d/net/40-smp-affinity` file is changed to spread the load of the two 2.5Gbit interfaces over different cores.

## Core frequency tweaks

Through `/etc/rc.local` we tweak the CPU frequency algorithm to ramp up sooner and faster. This makes the cores run at full speed once the network starts being used a bit, but still allows it to clock down on near idle conditions.

# SupercapacitorSolarNode

Here is a quick write-up on my attempt at a solar powered supercapacitor node. Currently one lives in a tree above my house and the other at ground level that I use for testing purposes (firmware, rewiring, etc).

![Tree mounted node](pics/treemount.jpg)

## Quick Specs

- 5V 1150F (2x 2.5V 2300F) = ~700mAh @ 3.3v boost

- Heltec T114

- Gps, bme280, ina3221

- Attiny85 cutoff at 1v and resume at 2.5v

- 12v MPPT (CN3791)

- 10w renogy

- 7dbi antenna

![Assembled](pics/assembled.jpg)
![Exploded](pics/exploded1.jpg)


## Hardware

Any supercapacitors will work as long as the rated voltage totals over 4.2v. Preferably 5v or higher since the closer you charge a supercapacitor to it's rated voltage, the faster it will self-discharge (meaning it wastes power for no reason).
[2.5V 2300F supercapacitors](https://vi.aliexpress.com/item/1005008157585442.html)

![Supercaps](pics/Supercaps.jpg)

Supercapacitor balancing boards are required since charging and discharging frequently can cause one of the 2 to become higher voltage than the other and potentially exceed the max voltage rating... Meaning it will release the magic smoke.
I used these ones that balance for 2.5v. They require full assembly on a hotplate.
[Balance boards DIY kit](https://vi.aliexpress.com/item/1005007409357117.html)

I am using this custom cutoff and delayed resume board to manage the system when voltage drops too low to be of use. It also supplies the 3.3v output to the MCU.
[ATTiny85-Power-Controller](https://github.com/hawkeyes0v0/ATTiny85-Power-Controller)


[DigiSpark ATTiny85](https://vi.aliexpress.com/item/2040316211.html)

![DigiSpark](PCB/Digispark.PNG)

## Supercapacitor Info

The capacitors used are 2x 2.5V @ 2300F supercapacitors. They have been arranged in 2 sets in series with 2.5v balancing boards attached to each one. This comes to a bank of 5V @ 1150F. If ALL energy could be pulled from the capacitors, they can provide 2.66Wh of power or that of a 3.7V 700mAh Li-ion battery.

Link to datasheet: https://www.chemi-con.co.jp/e/catalog/pdf/dl-e/dl-sepa-e/dl-dle-e-170401.pdf

The equations I used to calculate the capacitance and subsequently the available watt hours:

(C1*C2) / (C1+C2) = Ctotal
2.5V 2300F + 2.5V 2300F
(2300*2300) / (2300+2300) = 1150F @ 5V

Using 4.2V to 1.0V of available potential
((C * (Vmax^2)) / 2) - ((C * (Vmin^2)) / 2) = Joules Total
((1150 * (4.2^2)) / 2) - ((1150 * (1.0^2)) / 2) = 9568 J

Joules / 3600 seconds = Watt hours
9568 / 3600 = 2.66 Wh (theoretical maximum available power)

Watt hours / output voltage of DCDC converter = available Ah
2.66 / 3.3 = 0.805 Ah

At 3.3V output, 800mAh can be expected while ignoring self discharge and boost-buck converter inefficiencies.


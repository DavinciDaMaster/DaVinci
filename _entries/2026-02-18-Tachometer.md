---
layout: entry
date: 2026-02-18
clusters:
  - silicon
  - electronics
excerpt: |
  Simple things have a surprising level of depth to them.
status: completed

---

# **RGB Based Tachometer Display for BZR4** #
![Tachometer](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/TachoThumbnail.jpeg)

---

## *The Origins* ##

A Tachometer is used to Quantify the otherwise Qualitative *Wroom Wrooms* that your vehicle makes.    
On BZR3, we used a barebones setup with 11 monochrome Red, Green and Blue LEDs (3 Green, 4 Red and 4 Blue) to indicate the current RPM of the engine. This required a whole another Arduino Nano (Which is a microcontroller dev board I despise for a multitude of reasons) just for the tachometer along with two DIP based SN74HC595 Shift registers, making the whole setup (unnecessarily) bulky in my humble opinion. It also meant that the colours displayed for each level fixed in hardware. The brightness control was purely hardware, based on the alue of the current limiting resistor for each diode. Due to no proper on-field testing, the values chosen made the tachometer leds too bright, and caused physical discomfort for the driver; Ultimately leading to its removal from BZR3 for the SUPRA SAE competition that was held in August of 2025.  

---

## *Brainstorming* ##

In conclusion, BZR4 needed a new tachometer. Something fancier. Something more Dyamic and a lot more flexible  
Immediately, Inspiration was drawn from the shift indicator lights present on many race cars from F1 and Nascar. The idea of operating it as a shift indicator, with a converging light pattern instead of a linear pattern was also considered.  
Finally, the chosen setup included the following:  

- 11 Common Anode RGB THT LEDs  
  These would each represent 1k RPM, just like the old version, allowing us to represent our peak RPM of 10.5k RPM (A "flaw" as one might call it, is the 11th LED is never illuminated, but the choice is in software).
- Custom PCB with all SMD components  
  After the horrendous space efficiency we achieved last time, I was determined to do better. Ever single component other than the LEDs were now in SMD package. A custom PCB was required to fit the module into a specifically designed space above the Display and now extra care was to be taken into routing the entire module within that footprint.
- CH32V003 RISC-V based Microcontroller  
  To drive the display, I switched to a much cheaper, much smaller microcontroller platform, that also turned out to be much more reliable as well. I have a multitude of projects in the pipeline with this specific microcontroller, and I've been loving it so far. To Hell With The Nano!


---

## Completed Product in action on the field ##
<Video src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/TachoComplete.mp4" width="512" height="480" controls></video>
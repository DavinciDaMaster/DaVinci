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
On BZR3, we used a barebones setup with 11 monochrome Red, Green and Blue LEDs (3 Green, 4 Red and 4 Blue) to indicate the current RPM of the engine. This required a whole another Arduino Nano (Which is a microcontroller dev board I despise for a multitude of reasons) just for the tachometer along with two DIP based SN74HC595 Shift registers, making the whole setup (unnecessarily) bulky in my humble opinion. It also meant that the colours displayed for each level was fixed in hardware. The brightness control was purely hardware, based on the value of the current limiting resistor for each diode. Due to no proper on-field testing, the values chosen made the tachometer leds too bright, and caused physical discomfort for the driver; Ultimately leading to its removal from BZR3 for the SUPRA SAE competition that was held in August of 2025.  

---

## *Brainstorming* ##  
In conclusion, BZR4 needed a new tachometer. Something fancier. Something more Dynamic and a lot more flexible  
Immediately, Inspiration was drawn from the shift indicator lights present on many race cars from F1 and Nascar. The idea of operating it as a shift indicator, with a converging light pattern instead of a linear pattern was also considered.  
Finally, the chosen setup included the following:  

- **11 Common Anode RGB THT LEDs**  
  These would each represent 1k RPM, just like the old version, allowing us to represent our peak RPM of 10.5k RPM (A "flaw" as one might call it, is the 11th LED is never illuminated, but the choice is in software).
- **Custom PCB with all SMD components**  
  After the horrendous space efficiency we achieved last time, I was determined to do better. Ever single component other than the LEDs were now in SMD package. A custom PCB was required to fit the module into a specifically designed space above the Display and now extra care was to be taken into routing the entire module within that footprint.
- **CH32V003 RISC-V based Microcontroller**  
  To drive the display, I switched to a much cheaper, much smaller microcontroller platform, that also turned out to be much more reliable as well. I have a multitude of projects in the pipeline with this specific microcontroller, and I've been loving it so far. To Hell With The Nano!

---

## *The Hardware* ##  
### Physical Form Factor: ###
The tachometer to be designed was designated to be located above the display in an arc of a circle. The case for which was made integrated for both. Here is what the final model that was 3D printed using PLA looked like:
![Display and Tacho Case](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/TachoDisplayCase.png)

Therefore, the physical form factor was well defined. The outline of the required shape was traced, along with the centres for the THT LED positions in order to ensure proper alligment. A DXF file was promptly drafted and imported into KiCad. Onto the *actual* hardware; The electronics.
### High Level Block Diagram: ###
![Hardware Block Diagram](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/TachoHighLevel.jpeg)  
As the above block diagram represents, we are driving our 11 RGB LEDs from the outputs of two daisy chained SN74HC595 shift registers. This works quite neatly with SPI communication as we are able to directly send 16 bits of serial data at a time with each SPI transmission.  
The calculation of the RPM from the CrankShaftPosition sensor output (CKP: One square pulse every rotation of the Output Crankshaft) and the driving of the display through SPI signals sent to the Shift Registers, was done using a CH32V003F4U6 on a custom breakout board.  
Of the 16 push-pull outputs available to us on the shift registers, the first 11 are used to pull a corresponding common annode of the RGB LEDs high; Effectively "Selecting" it. The 12th, 13th and 14th outputs are used to sink the Red, Green and Blue cathodes (Which are all interconnected) to ground. The last 2 outputs are not connected.     

### Hardware Schematic: ###
![Hardware Block Diagram](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/TachoSchematic.png)  
You may look at this and assume, "That's pretty neat. I just select the LEDs I want turned On using the first 11 bits, and choose whichever colour I want them to be using the 12th, 13th and 14th bit of the SPI serial data. But what if I wanted different parts of the display to be different colours?"  
Which is exactly the thought process I went through. In order to have multiple colours displayed simultaneously, the R, G and B colour lines will have to be decoupled for each section we want to operate different colours on. This will consequently require more mosfets as well, unless we are splitting them into 5 colour lines or more, in which case the Shift Register Outputs will be able to sink the current we are asking them to without the assistance of a mosfet as is the case here. (I've calculated a total peak current of around 125mA through the colour lines, while the datasheet of the shift registers state a maximum of about 10mA).  
To be fair, the current draw can be limited further by using larger value resistors at the common annodes (Testing with 330 ohm resistors, the brightness was just right. I opted for 220 ohm instead as I knew I was going to be implementing software PWM based Brightness dimming).  
The top left section might spike some interest with a diode and resistor seemingly wasting energy as it is connected between +5v and Gnd. Do not be fooled so easily my friends!(I may not look it but my stupidity does not extend to that extreme of a level). This section serves an important role of making the entire shift register control system able to operate on 3v3, to accomodate microcontrollers other than the V003, such as the V203 which operates from 1v8 to 3v3. How?  
Well, if you take a look at the datasheet for the [SN74HC595BRWNR](https://www.ti.com/lit/ds/symlink/sn74hc595b.pdf?HQS=dis-dk-null-digikeymode-dsf-pf-null-wwe&ts=1768275643289&ref_url=https%253A%252F%252Fwww.ti.com%252Fgeneral%252Fdocs%252Fsuppproductinfo.tsp%253FdistId%253D10%2526gotoUrl%253Dhttps%253A%252F%252Fwww.ti.com%252Flit%252Fgpn%252Fsn74hc595b), which is a 16 pin **X**QFN (**!!!Very important distinction from a 16 pin** QFN **package!!!**) we find that the minimum voltage threshold for a data High varies with the supplied VCC. At 6V, the data lines need to exceed 4.2V to register as a High. Similarly, at 4.5V, we require a minimmum of 3.15V. Hence, the Diode and bleeder resistor setup at the top left of the scematic serve the purpose of dropping the 5V input down to around 4.3V, allowing around 3V (70% of VCC) to register as a logic High. The bleeder resistor is required to bias the diode into its constant forward voltage drop region of operation.  
With all that said, the poor man's voltage regulator is left unused for our current application with the CH32V003.  

### PCB Design: ###
![PCB](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/PCB.png)  
![PCB 3D View Front](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/PCBF.png)  
![PCB 3D View Back](https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/PCBB.png)   

---

## Completed Product in action on the field ## 
<Video src="https://raw.githubusercontent.com/DavinciDaMaster/DaVinci/main/_entries/Tachometer_Media/TachoComplete.mp4" width="512" height="480" controls></video>
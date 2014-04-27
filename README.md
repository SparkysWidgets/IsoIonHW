Welcome To: IsoIon Hardware Repo!!
================================


##### Note: This is for the IsoIon Hardware Version 01 Branch

IsoIon Hardware Design Files in EAGLE. IsoIon is the first and only fully open source ISE interface with galvanic isolation. It only requires a single supply on the primary end, meaning no more pesky dual supply solutions with wallwarts or DC-DC converters everywhere. IsoIon is an Ion Selective Electrode Interface with 127 steps of programmable gain control which allows it to adapt to most ISE style probes. Great care was taken during part selection to create the best all around solution with great performance for the price. All components have been proven over the last several months(if not years) via other projects (MinipH, ISOBreakout, I2CADCBreakout, etc...). IsoIon is meant to be used in a wide array of installations from commercial products, Arduino and RaspPi Projects to lab usage. If you need to interface pH, Galvanic DO, ORP, Nitrate, or most other ISE style probes this is the interface for you!!!! Its meant to interface most platforms and even contains a Seeed Grove style connector for easy integration into the Grove environment.
Please respect the licensing on these designs, we all loose if licensing inst followed and proper credit is not given!



Schematic and General Info
-------------------------
##### The initial schematic is a bit messy sorry will clean up.

- Most of this design is proven from my other works, this is the culmination of lots of research and testing (others have copied my work beware of their closed designs)
- Single supply Isolation is provided by the pair of AD micromodules ADUM6010 iso DC-DC converter and the ADUM1251 I2C isolation module for hotswapping I2C systems on a I2C bus 
- Analog front end is boosted with an I2C digital pot in place of on leg of the gain control feedback loop (this is a proper reostat digipot IC). This gives us gain control so we can select what type of probe to interface pretty ingenious Special Thanks to Practical Maker for showing me a simpler way. I just automated his method and adjusted it to match how I interface probes (We both do it slightly different, difference is good in my opinion keep up your work PM!!!!)
- Negative voltage rail generation has been slightly improved I added in an output filter network that was tested to help a bit of the noise getting through to the amplification stage.
-I've updated the design to use a fixed 3.0v precision voltage reference ad the ADC VDD/VREF input, while the op-amp will still operate at 5v rails. I am pushing us up and out of the noise floor on both rails, you won't see this much elsewhere and probably wont until this proliferates. This design still meets my requirements for 2.7 to 5.5 VIN!
- Final amp stage to ADC has been properly filtered as per TI- app notes for matching ADCs to conditioning amps
- Reverse voltage protection using Afrotechmods Pfet method! Its all about VGS vs VDS hehe!

Board Layout Info
-------------------------
##### Form factor is the same width as the Mini line but was made a bit longer to accommodate the Isolation section from my ISOBreakout designs.

- Standard 2.54mm pitch header and Grove style connector is left in place
- Careful consideration on both part placement and trace layout is very important to overall success, we are dealing with 2 fully isolated ground planes.
- DC-DC converter topologies exist on this board, some will be in close proximity to the input stage. the Negative voltage rail generator is based on a charge pump at 250Khz we want to filter this out at the -5v output, but we want only enough bypass capacitance to filter these harmonics and a bit more general op-amp operation, since we have 1uF already at the chargepump. built RC network around .1uF as close to op-amp as possible, with 1uf supply at charge pump out put. Keep these traces short and surrounded by ground plane. 
- Proper placement and layout of the analog front end is crucial to achieve best performance characteristics. Guard rings must be used around the input leg of BNC, this is so often overlooked it makes me sad. Am I the only one who reads the app-notes datasheets and op-amp books??? If your probe is going to ground to your plane(ie non differential or instrumentation topo), make sure you control the current return path (this really isnt too much of a concern on pH interfacing but other probes can source mAs)
- As per above, Op-amp is mounted underside with minimal trace length to keep complex impedances/reactances to minimum, this lets us keep all the "hot" end of amplification stage inside a guard ring. As per op-amp operation the feedback loop output(into the - pin) will drive the guard ring to the same potential as the probe input pin so everything inside the ring will have enough drive to keep it there despite noise sources and outside influences, pretty cool huh! See layout of board to get a better idea, this is how an ideal guard ring should work (including both sides as shown with the small ring on top)
- The rest of the layout is just based on functional placements with minimal trace routing work, via count inst really an issue so burn them up if you want to make really nicely couple ground planes.
- Some test pad experimentation with the obvious ones marked in new "parts" GND, V+, V- with offset and pHVout test points near their usage points.
- REMEMBER ISOLATED GROUNDS DONT CROSS THEM, take measurements from each side with respect to its ground(reference) it is important to remember or else you may burn up the isolation ICs 
- Revers voltage protection added to both header VIN inputs, using a simple p-channel mosfet to since reverse voltages and switch off.

Errata
-------------------------

##### Just a quick list of current issues
- Mostly untested as a whole, but each part has been working for a while are are mature projects
- Silkscreen  as always could use some improvements.
- Care might be needed in measuring the digipots wiper to calculate the gain more accurately
- This thing is going to be awesome and change how we think of plain old pH interfaces!!!!!

Firmware
-------------------------

- Take a look in [isoIon's Firmware Repo](https://github.com/SparkysWidgets/IsoIonBFW) For Arduino!
- Check out my USB pH interface[LeoPhi](http://www.sparkyswidgets.com/Projects/LeoPhi.aspx) for a powerful and easy to use USB PH Probe interface!

Basic Usage
-------------------------


License Info
-------------------------

<p>This is a fully open source project released under the CC BY license</p>
<a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US"><img alt="Creative Commons License" style="border-width: 0px;" src="http://i.creativecommons.org/l/by-sa/3.0/88x31.png" /></a><br />
<span xmlns:dct="http://purl.org/dc/terms/" property="dct:title">IsoIon</span> by <a xmlns:cc="http://creativecommons.org/ns#" href="www.sparkyswidgets.com" property="cc:attributionName" rel="cc:attributionURL">Ryan Edwards, Sparky's Widgets</a> is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-sa/3.0/deed.en_US">Creative Commons Attribution-ShareAlike 3.0 Unported License</a>.<br />
Based on a work at <a xmlns:dct="http://purl.org/dc/terms/" href="/portfolio-item/ion-selective-electrode-interface" rel="dct:source">https://www.sparkyswidgets.com/portfolio-item/ion-selective-electrode-interface</a>
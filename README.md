# BL0940-AC-Energy-Monitoring-Using-Arduino
AC power and energy monitoring device this IC used in most of the IoT application, calculate energy consumption in minimal form factor.
Get the detailed tutorial from my [Hackster Page](https://www.hackster.io/electroboy001/bl0940-ac-energy-monitoring-using-arduino-6b2673).

I have been working with energy metering ICs for a while now. After trying BL0937 which has a pretty straightforward control interface, today I finally got my hands on the BL0940. Both are similar, because they are single-phase energy metering IC but what surprise me the most is its feature set. Unlike the simpler BL0937 ICs in the BL series, the BL0940 gives you voltage, current, active power, energy accumulation, phase angle, power factor and temperature without the need of component calibration, and all data is accessible though UART or SPI.

<img width="1448" height="1086" alt="Image" src="https://github.com/user-attachments/assets/07ea90c5-20a7-4b9d-9033-ff0651e37dd1" />

But for now this tutorial is focused on SPI communication mode. You just pull the SEL pin HIGH for SPI, and the IC switches its entire communication interface. I built a complete Arduino library around this IC with five ready-to-use examples. The total component cost is very low a 1 milliohm shunt resistor, a voltage divider network, and a few capacitors is all you need on the analog front end. This is a open source project you can access the design files through GITHUB, I have made a dedicated PCB from [JLCPCB](https://jlcpcb.com/?from=audrey3) and tested it out with real time energy monitoring in my lab.

**What Is BL0940?**

The BL0940 is a single-phase energy metering IC, It has two sigma-delta ADCs for simultaneous voltage and current sampling, a digital multiplier for real-time active power computation, and an energy accumulator with pulse output. The IC operates from a 3.3V supply and uses an internal 1.218V reference voltage for all measurements. We can plug a load from 1W to 2500 watts in this with the PCB that I have designed. BL0940 has internal phase angle register with this register you can compute the phase difference between voltage and current, in order to get true power factor as cos(phi) without any extra signal processing. The BL0940 also has a built-in internal temperature sensor and supports an external NTC thermistor on the VT pin.

**Circuit Diagram**
I had followed the datasheet, and used the circuit as per the configuration in the documentation. The current sensing path uses a 1 milliohm shunt resistor connected between the IP1 and IN1 pins. At the maximum rated current, the voltage drop across this shunt stays well within the +/-200 mV. The 2.2 uF anti-aliasing capacitor are added across the IP/IN inputs to filter out high-frequency noise from the mains. This is important because the sigma-delta ADC inside the BL0940 can alias at high-frequency harmonics.

<img width="900" height="746" alt="Image" src="https://github.com/user-attachments/assets/b4ca5a7f-cda8-42c3-a8a0-d56e8d76db4e" />

**Calibration using Load:**
The SEL pin must be tied to VDD (3.3V) for SPI mode. Now use a 100W bulb, note down the socket voltage current and compute power manually using multimeter. Then feed the values inside the code like this:

<img width="1024" height="768" alt="Image" src="https://github.com/user-attachments/assets/aa8a8828-1ca6-47be-902d-13845c9eee75" />

Make the onboard circuit as given here, you have to use the OLED, do not use laptop serial monitor when the IC is plugged in 220V. Laptop is only used to upload the program in the Arduino.

<img width="768" height="576" alt="Image" src="https://github.com/user-attachments/assets/503e99c1-9010-4498-a37b-6da6eb27ec3c" />

After upload connect the 5V power charger and load as given here, and note down the coefficients that are displayed on the OLED. Note them down and copy in the main working code for calibration.

**Working Code:**
Here is the main working code after uploading the calibration coefficients see how the reading changes.

<img width="1024" height="665" alt="Image" src="https://github.com/user-attachments/assets/b08d4ee0-bfa7-4e48-b2a5-16c8bde25fb6" />

**Outro:**

<img width="1024" height="768" alt="Image" src="https://github.com/user-attachments/assets/36013178-9f09-458b-8af6-bc416bdd87cd" />

With all this we can say this is an amazing IC, you can download the main codes from GITHUB. Library works with any Arduino-compatible board that has a serial port or SPI. Whether you are building a commercial smart plug or just experimenting with energy measurement, this project gives you a solid foundation. Library works with any Arduino-compatible board that has a serial port or SPI. This IC find application in IoT related stuff out there, If you want to plan a power supply that deliver 5V directly from 220V see our tutorial on: Smallest 220V to 5V Supply for IOT applications. If you have any questions, suggestions, or want to share your own BL0940 build, please comment below. I would love to see what you make with it.

<!-- markdownlint-disable MD053 -->
# Plan

- [Plan](#plan)
  - [BIG NOTE](#big-note)
  - [Shopping list](#shopping-list)
  - [Requirements](#requirements)
    - [Gardens size](#gardens-size)
  - [Questions](#questions)
    - [i am working on a project. the idea is to have an automatic garden. i created an app that will control some eletro-valves. the app is working fully, connecting to my api/db. the database has 2 tables: systems and valves. each system can have multiple valves that can be open or closed. the system can be on or off. now i need to figure out the hardware i will be using. please help me design and choose the correct items to be able to complete this project](#i-am-working-on-a-project-the-idea-is-to-have-an-automatic-garden-i-created-an-app-that-will-control-some-eletro-valves-the-app-is-working-fully-connecting-to-my-apidb-the-database-has-2-tables-systems-and-valves-each-system-can-have-multiple-valves-that-can-be-open-or-closed-the-system-can-be-on-or-off-now-i-need-to-figure-out-the-hardware-i-will-be-using-please-help-me-design-and-choose-the-correct-items-to-be-able-to-complete-this-project)
    - [does eletrovalves with also manual open/close functionality exist?](#does-eletrovalves-with-also-manual-openclose-functionality-exist)
    - [i think i might want to go with 433 MHz or 915 MHz RF Modules. can you explain a bit more in detail how to achieve this?](#i-think-i-might-want-to-go-with-433-mhz-or-915-mhz-rf-modules-can-you-explain-a-bit-more-in-detail-how-to-achieve-this)
      - [1. Hardware Components](#1-hardware-components)
      - [2. Typical Wiring for 433 MHz RF Modules](#2-typical-wiring-for-433-mhz-rf-modules)
      - [3. Communication Concept](#3-communication-concept)
      - [4. Software and Protocols](#4-software-and-protocols)
      - [5. Range and Antenna](#5-range-and-antenna)
      - [6. Example Setup Workflow](#6-example-setup-workflow)
      - [7. Important Considerations](#7-important-considerations)
      - [Summary of Steps to Achieve RF Communication with Raspberry Pi Pico](#summary-of-steps-to-achieve-rf-communication-with-raspberry-pi-pico)
    - [transmitter/receiver snippet examples](#transmitterreceiver-snippet-examples)
      - [Assumptions and Setup](#assumptions-and-setup)
      - [1. Transmitter Code (MicroPython) — send a command repeatedly](#1-transmitter-code-micropython--send-a-command-repeatedly)
      - [2. Receiver Code (MicroPython) — decode and control relay](#2-receiver-code-micropython--decode-and-control-relay)
      - [Notes](#notes)
    - [alright, lets do a quick recap please. small hardware implementation summary and list of items to buy please](#alright-lets-do-a-quick-recap-please-small-hardware-implementation-summary-and-list-of-items-to-buy-please)
      - [Hardware Implementation Summary](#hardware-implementation-summary)
      - [Suggested Shopping List](#suggested-shopping-list)
    - [there are several rpi pico, what are the differences between them? can you make a table with the key differences pleas?](#there-are-several-rpi-pico-what-are-the-differences-between-them-can-you-make-a-table-with-the-key-differences-pleas)
    - [based on this table with the comparisons which one should i pick for my project?](#based-on-this-table-with-the-comparisons-which-one-should-i-pick-for-my-project)
      - [Key points from your project needs](#key-points-from-your-project-needs)
      - [Recommendations from the table and context](#recommendations-from-the-table-and-context)
      - [Overall Suggestion](#overall-suggestion)
    - [my water lines are currently being fed water from a water pump, it's not using a hose. here are the sizes of my gardens](#my-water-lines-are-currently-being-fed-water-from-a-water-pump-its-not-using-a-hose-here-are-the-sizes-of-my-gardens)
      - [Recommended Valve Type](#recommended-valve-type)
      - [Recommended Relay Modules](#recommended-relay-modules)
      - [Valve and Relay Sizing for Your Project](#valve-and-relay-sizing-for-your-project)
      - [Additional Notes](#additional-notes)
      - [Summary](#summary)
    - [how expensive can this approach be compared to the one where we use a transformer for lower voltage relays?](#how-expensive-can-this-approach-be-compared-to-the-one-where-we-use-a-transformer-for-lower-voltage-relays)
      - [1. Cost Factors for Direct 220V AC Relay Switching](#1-cost-factors-for-direct-220v-ac-relay-switching)
      - [2. Cost Factors for Transformer + 24V AC Valve Setup](#2-cost-factors-for-transformer--24v-ac-valve-setup)
      - [3. General Cost Comparison](#3-general-cost-comparison)
      - [Summary Table](#summary-table)
      - [Recommendation for Your Case](#recommendation-for-your-case)
      - [References and Context](#references-and-context)
    - [can you please do a side by side comparison, maybe in a table with links to the parts needed for both approaches?](#can-you-please-do-a-side-by-side-comparison-maybe-in-a-table-with-links-to-the-parts-needed-for-both-approaches)
      - [Summary and Recommendation](#summary-and-recommendation)

## BIG NOTE

This file is mainly a compilation of questions/answers to AI (<https://www.perplexity.ai>) to help on the decision of the hardware parts for the h2oasis project

## Shopping list

- [RPI Pico](https://mauser.pt/catalog/product_info.php?cPath=1667_2620_1672&products_id=096-9421): ≃ 4.5€
- [RF kit](https://temperosystems.com.au/products/433mhz-rf-wireless-transmitter-and-receiver-module-kit/): ≃ 5.5€
- [4x IP65 Box](https://www.amazon.es/dp/B0D2TR8ZMK?sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY): ≃ 11€

|Aspect| A: Direct 220V AC Valve + 220V AC Relay |B: Transformer + 24V AC Valve + Low Voltage Relay|
|------|-----------------------------------------|-----------------------------------------------|
|Valve|[Beduan Brass Electric Solenoid Valve, 1/2" 220V Normally Closed](https://www.amazon.com/Beduan-Electric-Solenoid-Normally-Colsed/dp/B07N625C8J) ≃ 20€| [](https://www.amazon.com/Beduan-Electric-Solenoid-Normally-Colsed/dp/B07N644BL9) ≃ 21€|
|Relay|||
|Transformer|not needed||

## Requirements

### Gardens size

- 1m x 23m
- 12m x 23m
- 1.5m x 10m
- 5m x 11m
- 5m x 12m
- 9m x 12m
- 7m x 9m

Total area = 1 x 23 + 12 x 23 + 1.5 x 10 + 5 x 11 + 5 x 12 + 9 x 12 + 7 x 9 = 23 + 276 + 15 + 55 + 60 + 108 + 63 = **600 m²**

Need at least 7 valves and possibly, only 4 RPI Pico to control the valves (some gardens are closer and can share the same RPI Pico).

## Questions

### i am working on a project. the idea is to have an automatic garden. i created an app that will control some eletro-valves. the app is working fully, connecting to my api/db. the database has 2 tables: systems and valves. each system can have multiple valves that can be open or closed. the system can be on or off. now i need to figure out the hardware i will be using. please help me design and choose the correct items to be able to complete this project

To complete your automatic garden project, you will need to coordinate several hardware components that interface with your app-controlled API/database and physically operate the electro-valves. Here’s a step-by-step guide based on best practices for garden irrigation automation:

**1. Electro-valves (Solenoid Valves)**

- Choose reliable, 24V AC solenoid valves. Top brands include **Hunter PGV-101**, **Rain Bird 100-DVF**, and **Irritrol 205SF**. These are robust, widely used, and support flow control—a useful feature for tuning water output per valve [1][1-1]  [3][1-3]  [5][1-5]  [7][1-7]  .
- For small to medium gardens, a **1 inch** valve size is standard and compatible with most piping [3][1-3]  [5][1-5]  .

**2. Valve Power and Control**

- Most commercial garden valves operate at **24V AC**. You will also find DC and even battery-operated valves, but for automated systems with several valves, AC power is the norm [3][1-3]  [5][1-5]  [9][1-9]  .
- You will need a **24V AC power supply** capable of driving the number of valves you'll use, considering that each solenoid draws roughly 0.2A to 0.3A when activated.

**3. Control Hardware (Microcontroller or IoT Module)**

- Use a microcontroller (e.g., **ESP32**, **Raspberry Pi**, or **Arduino** with WiFi/Ethernet) that can send commands to relays that operate the valves. ESP32 is a strong candidate for wireless control and direct API interaction.
- To bridge your app and hardware, run a small script or service on the microcontroller to poll your API and trigger the correct relays.

**4. Relay Board**

- To safely switch 24V AC to the valves, use a **relay module** (ideally with opto-isolated channels). Make sure you have enough channels for every valve.
- For 24V AC, standard mechanical relays are common; select a board rated for at least 5A per channel to ensure reliability.

**5. Power Supply**

- You will need two power rails:
  - **24V AC** for the valves.
  - **5V or 3.3V DC** for your controller board (ESP32/Raspberry Pi).
  - An **AC-DC step-down converter** or separate power adapter will likely be necessary.

**6. Optional: Sensors and Feedback**

- For smarter automation, consider adding soil moisture sensors and/or rain sensors. This allows your app to determine if watering is necessary, optimizing valve operation [3][1-3]  .

**7. Wiring and Protection**

- Use waterproof connectors and outdoor-rated cable for all connections.
- Consider enclosure boxes to house relays, controllers, and transformers away from direct exposure to water/weather.

**8. Example Setup (simplified):**

- Your app triggers the "open valve" action for a specific valve.
- Your API relays this to the microcontroller.
- Microcontroller activates the corresponding relay.
- Relay closes the 24V AC circuit to the selected solenoid valve.
- Valve opens, irrigation starts.

**Suggested Shopping List:**

- Hunter PGV-101 or Rain Bird 100-DVF solenoid valves [number of valves = number of garden zones] [3][1-3]  [5][1-5]  [7][1-7]  .
- 24V AC transformer (power rating = 0.3A x valves + 25% reserve).
- 8 or 16-channel relay module (according to your project scale).
- ESP32 dev board (WiFi) or Raspberry Pi (Ethernet or WiFi).
- AC-DC stepdown converter (for microcontroller power).
- Waterproof electrical enclosures and cable glands.

**Caveats and Considerations:**

- If your garden water pressure is low or you are running from a rain barrel, you may need "direct-acting" or "servo-controlled" valves [5][1-5]   .
- Ensure cable runs are as short as possible and connectors are protected outdoors.
- Always test each valve circuit with manual commands before full automation.
- Think about fail-safe behavior (e.g., what happens during a power cut).

For most home and hobbyist automated garden projects, these components are widely available and well-documented in the irrigation community. If you need specific wiring diagrams or code examples, let me know your chosen controller (e.g., ESP32, Arduino), and I can advise further.

[1-1]: <https://www.reddit.com/r/Irrigation/comments/wpfdp7/best_irrigation_valve/>
[1-2]: <https://www.gd-pud.org/files/d8f556483/AMR+Attachments.pdf>
[1-3]: <https://www.irrigation-guide.com/solenoid-valves-for-garden-irrigation-which-ones-are-there-and-how-do-they-differ.html>
[1-4]: <https://digilib.stekom.ac.id/assets/dokumen/ebook/feb_16d96e00f242650b62b643d9bd157db22501144c_1646717172.pdf>
[1-5]: <https://dvs-beregnung.de/solenoid-valves-irrigation>
[1-6]: <https://www.wipo.int/web-publications/green-technology-book-energy-solutions-for-climate-change/assets/69780/1080E_GreenTech24_WEB.pdf>
[1-7]: <https://www.youtube.com/watch?v=V-h00iKHJOE>
[1-8]: <https://homel.vsb.cz/~sta048/mcu/doc/book/making_embedded_systems.pdf>
[1-9]: <https://www.dripworks.com/blogsolenoid-valves-what-do-they-do-and-how-to-choose-the-right-solenoid-valve-for-your-irrigation-system>
[1-10]: <https://smvdu.ac.in/wp-content/uploads/2025/07/Course-Study_2014-15_compressed.pdf>

### does eletrovalves with also manual open/close functionality exist?

Yes, **electrovalves (solenoid valves) with manual open/close functionality do exist**. Most standard garden irrigation valves—such as those from Hunter, Rain Bird, Irritrol, and other major brands—allow you to manually operate the valve in addition to electronic control. This manual feature is especially useful for maintenance, troubleshooting, or in case of controller failure.

Key points regarding manual operation:

- Many irrigation valves allow you to **turn the solenoid a quarter turn counterclockwise** to manually open the valve; turning it clockwise will close the valve again. This doesn't require electrical power, just physical access to the valve [1][2-1]  [4][2-4]  [6][2-6]  .
- Some models also feature a **manual bleeder screw**—loosening it will temporarily open the valve and tightening it shuts it off again. Not all models include this, but it's common on professional-grade valves [2][2-2]  [3][2-3]  .
- Manuals and support information from brands like Rain Bird and Hunter explicitly describe manual open/close procedures, confirming this is an intentional and reliable feature across most modern electrovalves [1][2-1]  [3][2-3]  [6][2-6]  .
- You may see “manual override” mentioned in technical specifications, which is the feature enabling this type of operation [5][2-5]  .

In summary, for nearly all mainstream **garden irrigation electrovalves**, you will be able to manually open or close the valve if needed, either by turning the solenoid or using the bleeder screw, as described in the product's manual.

[2-1]: <https://www.youtube.com/watch?v=1Fe2ZCBa99Q>
[2-2]: <https://www.youtube.com/watch?v=w9OrDIwbH5s>
[2-3]: <https://www.youtube.com/watch?v=Zijg05h-Lzw>
[2-4]: <https://www.youtube.com/watch?v=8UCnIzyg7c4>
[2-5]: <https://tameson.com/pages/irrigation>
[2-6]: <https://www.rainbird.com/media/1308>
[2-7]: <https://www.hunterirrigation.com/en-metric/support/how-do-i-manually-open-valve>
[2-8]: <https://www.hillsirrigation.com.au/irrigation-solenoid-valve-troubleshooting/>
[2-9]: <https://www.hunterirrigation.com/en-metric/support/how-does-irrigation-valve-work>
[2-10]: <https://gardeningwithpope.com.au/irrigation-control/>

### i think i might want to go with 433 MHz or 915 MHz RF Modules. can you explain a bit more in detail how to achieve this?

To use **433 MHz or 915 MHz RF modules** with your Raspberry Pi Pico microcontrollers to control valves from a central server without Wi-Fi, here is a detailed explanation and step-by-step approach:

#### 1. Hardware Components

- **Raspberry Pi Pico (or Pico W if you want Wi-Fi later)**
- **433 MHz or 915 MHz RF Transmitter and Receiver Modules**
  - 433 MHz modules are very common, cheap, and easy to use for short to medium distances (up to 100-200 meters line of sight).
  - 915 MHz is less common but offers potentially better range and less interference.
- **Relay Module** to switch valve power (your valves might need 12V/24V controlled via relay).
- Power supplies: 5V for RF modules and Pico, separate supply for valves.

#### 2. Typical Wiring for 433 MHz RF Modules

- **Transmitter Module Pins:**
  - VCC to +5V power (usually from a regulated source)
  - GND to ground
  - DATA pin to one of the Raspberry Pi Pico's GPIO pins (for example GPIO18)
- **Receiver Module Pins:**
  - VCC to +5V
  - GND to ground
  - DATA pin connected to a different GPIO pin on the Pico (for example GPIO13)
- Make sure the Raspberry Pi Pico ground and module ground are connected.

#### 3. Communication Concept

- The transmitter sends encoded on/off commands as digital signals via RF waves at 433 MHz.
- The receiver picks up these signals and outputs the digital data to the Pico.
- The Pico parses the received signal and executes commands, such as opening or closing a valve via a relay.

#### 4. Software and Protocols

- Use libraries such as **rcswitch** (popular in Arduino environments) or adapt MicroPython code for the Pico that can send and receive RF signals.
- Your Pico should run a firmware program that:
  - Monitors the receiver GPIO pin for valid RF commands.
  - When a recognized command arrives, triggers the GPIO pin connected to the relay to open/close the valve.
  - The transmitting Pico (central server side) sends commands to the remote Pico(s) using the transmitter module.

#### 5. Range and Antenna

- Use a proper antenna on both transmitter and receiver modules to maximize range (typically a 17 cm wire antenna for 433 MHz).
- Obstructions and interference affect range, so test in your garden's environment.

#### 6. Example Setup Workflow

- Connect the transmitter module to the central controller (can be a more powerful microcontroller or Raspberry Pi).
- Connect each valve controller Pico with receiver module and relay.
- The central controller sends commands as RF codes corresponding to valve IDs and states.
- Valve controllers listen and act on those commands by switching relays.

#### 7. Important Considerations

- Data protocols on 433/915 MHz are unencrypted and can be noisy; consider adding checksums or custom encoding.
- Each valve could have an ID or code to filter and prevent unwanted activation.
- Multiple valves require a protocol to address commands uniquely.
- Modules operate at 5V logic, but Pico is 3.3V so you may need logic level shifting or ensure the RX pin tolerates 5V (some modules output 3.3V compatible levels).

#### Summary of Steps to Achieve RF Communication with Raspberry Pi Pico

| Step                    | Details                                              |
|-------------------------|------------------------------------------------------|
| Hardware                | Pico + RF module (5V power, data to GPIO), relay for valves |
| Wiring                  | Connect VCC/GND; data pin transmitter to TX GPIO, receiver data pin to RX GPIO |
| Antennas                | Attach 17cm wire antennas for good range            |
| Software                | Use/adapt RF libraries in MicroPython/C++ for Pico  |
| Communication Protocol  | Send encoded on/off codes; receiver filters and triggers valve relay |
| Power                   | Separate power for valves (12/24V), 5V for RF and Pico |

[3-1]: <https://www.youtube.com/watch?v=Xe5Bj_N4Crw>
[3-2]: <https://www.instructables.com/RF-433-MHZ-Raspberry-Pi/>
[3-3]: <https://github.com/BlackDeath12/Pico433MhzRFModule>
[3-4]: <https://www.instructables.com/Super-Simple-Raspberry-Pi-433MHz-Home-Automation/>
[3-5]: <https://forums.raspberrypi.com/viewtopic.php?t=66946>
[3-6]: <https://www.reddit.com/r/raspberrypipico/comments/wi2gtg/raspi_pico_433mhz_communication/>
[3-7]: <https://nessy.info/post/2022-03-14-raspberry-pi-pico-and-433mhz-data-radio/>
[3-8]: <https://github.com/AdrianCX/pico433mhz>
[3-9]: <https://datacapturecontrol.com/articles/data-communication/wireless/rf/hc12/hc12-rpi-pico-micropython>

### transmitter/receiver snippet examples

Certainly! Below is a basic example of how to **send and receive 433 MHz RF signals** using Raspberry Pi Pico with MicroPython. This example will include:

- A **transmitter** snippet that sends a simple code (e.g., valve ID + command).
- A **receiver** snippet that listens for codes and toggles a relay GPIO pin to open/close a valve.

#### Assumptions and Setup

- You have a 433 MHz transmitter module connected to **Pico GPIO 16** (data out)
- You have a 433 MHz receiver module connected to **Pico GPIO 17** (data in)
- The relay controlling the valve is connected to **GPIO 18**
- Both RF modules powered by 5V, grounds connected to Pico GND
- Simple encoding: one byte code sent repeatedly by the transmitter, decoded by receiver

#### 1. Transmitter Code (MicroPython) — send a command repeatedly

```python
from machine import Pin
import time

transmitter = Pin(16, Pin.OUT)

def send_pulse(high_time_us, low_time_us):
    transmitter.value(1)
    time.sleep_us(high_time_us)
    transmitter.value(0)
    time.sleep_us(low_time_us)

def send_bit(bit):
    if bit == '1':
        # For example, longer high pulse for '1'
        send_pulse(1000, 1000)
    else:
        # shorter pulse for '0'
        send_pulse(500, 1000)

def send_code(code):
    # Send each bit in code string
    for bit in code:
        send_bit(bit)
    # Gap between repeated transmissions
    time.sleep_ms(10)

def main():
    # Example code for valve 1 open command: '1010'
    # You can define your own protocol/encoding with bytes or bits
    valve_code = '1010'

    while True:
        send_code(valve_code)

if __name__ == "__main__":
    main()
```

#### 2. Receiver Code (MicroPython) — decode and control relay

```python
from machine import Pin
import time

receiver = Pin(17, Pin.IN)
relay = Pin(18, Pin.OUT)

# Simple timing-based decode, not super robust
def decode_signal():
    bits = ''
    timeout = time.ticks_ms() + 2000  # listen max 2 seconds
    last_val = receiver.value()
    start_time = time.ticks_us()

    while time.ticks_ms() < timeout and len(bits) < 4:  # expecting 4 bits (example)
        val = receiver.value()
        if val != last_val:
            duration = time.ticks_diff(time.ticks_us(), start_time)
            # Threshold logic to discriminate '1' and '0' pulses
            if 400 < duration < 700:
                bits += '0'
            elif 900 < duration < 1200:
                bits += '1'
            start_time = time.ticks_us()
            last_val = val
    return bits

def main():
    print("Receiver running, waiting for codes...")
    while True:
        if receiver.value() == 1:
            code = decode_signal()
            if code:
                print("Received code:", code)
                # Example: If '1010', open valve by activating relay
                if code == '1010':
                    relay.value(1)  # Turn relay ON (open valve)
                    print("Valve opened")
                    time.sleep(10)  # Keep valve open 10 seconds
                    relay.value(0)  # Turn relay OFF (close valve)
                    print("Valve closed")

if __name__ == "__main__":
    main()
```

#### Notes

- This example uses a **very simple encoding scheme** and basic timing to distinguish bits; in real projects, you should use or adapt proven RF libraries or protocols (like OOK or Manchester encoding).
- Accurate timing and noise immunity can be challenging with simple code; consider using C/C++ SDK or existing libraries for robust signals.
- Alternatively, you can use libraries like **rcswitch** (Arduino-side) and port similar logic to MicroPython.
- Make sure your antennas are connected to both transmitter and receiver to get reliable communication.
- Test your timing values (`high_time_us`, `low_time_us`) and pulse duration thresholds in `decode_signal()` depending on your actual RF modules.

If you want, I can help you adapt this for more reliable protocols or help with wiring details! Would you prefer that?

### alright, lets do a quick recap please. small hardware implementation summary and list of items to buy please

Here is a **small hardware implementation summary and shopping list** for your automatic garden valve control using Raspberry Pi Pico microcontrollers and 433 MHz or 915 MHz RF modules without Wi-Fi:

#### Hardware Implementation Summary

- **Microcontroller:** Raspberry Pi Pico (preferably Pico W if Wi-Fi later desired, but Pico standard also fine) running MicroPython or C++ firmware.
- **Valve Control:** Use a **Relay Module** to switch valve power (typically 12V DC or 24V AC valves). The relay isolates the low-voltage microcontroller from high voltage/current of valves.
- **Power Supply:**
  - Step down your 220V AC mains to 5V DC (using a USB wall adapter or a dedicated AC-DC converter) to power the Pico and RF modules.
  - Separate power supply (12V DC or 24V AC depending on valve specs) for the valves themselves.
- **Communication Module:**
  - For wireless, use **433 MHz or 915 MHz RF Transmitter and Receiver Modules**.
  - Transmitter module connects to the central controller (could be a Raspberry Pi or another microcontroller).
  - Each valve-controller Pico has a receiver module to receive commands wirelessly.
- **Wiring:**
  - Connect RF module data pins to appropriate GPIOs on Pico.
  - Connect relay input pin to a Pico GPIO.
  - Ensure grounds of all modules and boards are connected.
- **Antenna:** Use simple wire antennas (~17 cm for 433 MHz) on both transmitter and receiver for improved range.
- **Enclosure:** Weatherproof case for outdoor protection and to safely house relay, Pico, and electronics near valves.

#### Suggested Shopping List

| Item                                        | Purpose                                     | Notes                                  |
|---------------------------------------------|---------------------------------------------|----------------------------------------|
| Raspberry Pi Pico (or Pico W)                | Microcontroller                             | 3.3V logic, programmable               |
| 433 MHz or 915 MHz RF Transmitter Module    | Send wireless commands                      | 5V powered                            |
| 433 MHz or 915 MHz RF Receiver Module       | Receive wireless commands                   | 5V powered                            |
| Relay Module (single or multi-channel)      | Switch valve power                           | Should handle valve voltage/current   |
| 12V DC or 24V AC Solenoid Valve              | Water control valve                         | Compatible with your garden system     |
| 5V Power Supply (USB wall adapter or DC-DC) | Power Pico and RF modules                    | Converts 220V AC mains to 5V DC       |
| Power Supply for Valve (12V DC or 24V AC)   | Power solenoid valve                         | Separate from Pico power               |
| Waterproof enclosure                         | Protect electronics outdoors                | IP65 or better recommended            |
| Wire antenna (e.g., 17 cm wire for 433 MHz) | RF communication range enhancement          | Solder or attach to RF modules' antenna pin |
| Jumper wires, connectors, terminal blocks    | For reliable wiring and connections         | Outdoor rated cables if exposed       |

This setup allows remote control of valves via RF signals sent from your central controller to working Picos at separate garden sites, all powered from mains electricity with proper voltage conversion and safe relay switching.

If you want, I can also provide wiring diagrams or more detailed specs for these components.

**References:**

- Raspberry Pi Pico hardware and power specs [1][4-1] [2][4-2] .
- Relay and valve control example with Pico [2][4-2] [3][4-3] .
- 433 MHz RF module connection and usage with Pico [8][4-8] .

Would you like me to help with wiring diagrams or purchasing links?

[4-1]: <https://datacapturecontrol.com/articles/io-devices/microcontrollers/rpi-pico>
[4-2]: <https://newbiely.com/tutorials/raspberry-pico/raspberry-pi-pico-water-liquid-valve>
[4-3]: <https://forums.raspberrypi.com/viewtopic.php?t=350519>
[4-4]: <https://www.cytron.io/tutorial/ir-remote-control-volume-activity-using-raspberry-pi-pico>
[4-5]: <https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html>
[4-6]: <https://bc-robotics.com/tutorials/getting-started-with-the-raspberry-pi-pico-irrigation-board/>
[4-7]: <https://www.rs-online.com/designspark/purchase-product/2122162?intcmp=AMERICAS-DS-_-DSAP-BP01-_-Oct-24-_-microcontroller-interfaces-leds-and-lamps_2122162>
[4-8]: <https://github.com/orgs/micropython/discussions/10313>
[4-9]: <https://101-things.readthedocs.io/en/latest/ham_transmitter.html>

### there are several rpi pico, what are the differences between them? can you make a table with the key differences pleas?

Here is a table summarizing the key differences between the main Raspberry Pi Pico models relevant to your project, based on the official specs and common versions available:

| Feature                      | Raspberry Pi Pico (original)         | Raspberry Pi Pico H                   | Raspberry Pi Pico W                  | Raspberry Pi Pico WH                 | Raspberry Pi Pico 2                | Raspberry Pi Pico 2 W             | Raspberry Pi Pico W5              |
|------------------------------|------------------------------------|-------------------------------------|------------------------------------|------------------------------------|----------------------------------|---------------------------------|---------------------------------|
| **Release Year**              | 2021                               | 2021                                | 2022                               | 2022                               | 2023                             | 2023                            | 2024                            |
| **Microcontroller**           | RP2040 (dual-core Cortex M0+, 133 MHz) | RP2040 same as Pico                 | RP2040 + wireless module           | RP2040 + wireless + headers        | RP2350 (dual-core Cortex M33, 150 MHz) | RP2350 + Wireless (Pico 2 w)    | RP2350 + Wireless + USB Type-C  |
| **SRAM**                     | 264 KB                             | 264 KB                              | 264 KB                             | 264 KB                             | 520 KB                           | 520 KB                         | 520 KB                         |
| **Flash Memory**              | 2 MB                               | 2 MB                                | 2 MB                               | 2 MB                               | 4 MB                             | 2 MB (limited)                 | 8 MB                           |
| **Wireless Connectivity**    | None                              | None                               | Wi-Fi 802.11n (2.4 GHz)            | Wi-Fi 802.11n (2.4 GHz)            | None                             | Wi-Fi 802.11n (2.4 GHz)        | Wi-Fi 802.11n + Bluetooth 5.3  |
| **Headers Pre-soldered**      | No                                | Yes                                | No                                 | Yes                                | No                              | Yes                           | Yes                           |
| **USB Connector**             | Micro USB                        | Micro USB                         | Micro USB                         | Micro USB                         | Micro USB                     | Micro USB                     | USB Type-C                    |
| **Power Modes**               | Sleep, Dormant                   | Sleep, Dormant                   | Sleep, Dormant                   | Sleep, Dormant                   | Low-power modes improved     | Low-power modes improved   | Low-power modes improved   |
| **PIO (Programmable IO)**     | 2 blocks, 8 state machines      | Same as Pico                     | Same as Pico                     | Same as Pico                     | 3 blocks, 12 state machines    | 3 blocks, 12 state machines  | 3 blocks, 12 state machines  |
| **Additional Features**       | Temperature sensor, PWM, ADC    | Same as Pico                    | Same as Pico + Wireless          | Same as Pico W + Headers         | Temp sensor, improved CPU     | Improved CPU + Wireless      | Improved CPU + Wireless + BT |
| **Typical Use**               | Basic microcontroller projects  | Same as Pico but easier prototyping with headers | Wireless IoT projects          | Wireless IoT with headers       | Higher performance projects   | Higher perf + wireless       | High-performance IoT & BLE    |
| **Price approx (€)**         | ~5.25€ (Mauser) / ~4.50€ (Mouser) | ~7.50€ / -                     | ~7.35€ (Mauser) / ~6.30€ (Mouser) | ~9.00€ / -                       | ~7.00€ (Mouser)              | ~6.30€ (Mouser)              | Not widely listed yet        |

- Pico H and Pico WH are basically Pico and Pico W but *with pre-soldered headers* for easier prototyping.
- Pico W adds Wi-Fi 802.11n (2.4GHz) wireless to original Pico capabilities.
- Pico 2 and Pico 2 W represent a newer generation with the RP2350 chip featuring faster Cortex-M33 cores, more SRAM (520KB vs. 264KB), and improved peripherals.
- Pico 2 W retains Wi-Fi but still uses a Micro USB connector.
- Pico W5 is the latest with RP2350, 8MB flash, USB-C connector and adds Bluetooth 5.3 along with Wi-Fi.
- Wireless models are best for IoT or remote control; non-wireless are lower cost and simpler for wired/other communications.

If you want full design files and pinouts, the official Raspberry Pi documentation is the most authoritative source [1][5-1] . This should help you pick the right Pico model based on your project requirements, such as wireless needs, USB connector preference, or performance demands.

[5-1]: <https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html>
[5-2]: <https://socialcompare.com/en/review/raspberry-pi-pico>
[5-3]: <https://www.elecrow.com/blog/pico-w5-vs-pi-pico-2w-compare-specs-features-and-applications.html>
[5-4]: <https://www.raspberrypi.com/news/raspberry-pi-product-series-explained/>
[5-5]: <https://circuitdigest.com/article/raspberry-pi-pico-variants-comparison>
[5-6]: <https://raspberrytips.com/raspberry-pi-models/>
[5-7]: <https://news.sparkfun.com/11692>
[5-8]: <https://socialcompare.com/en/review/raspberry-pi-pico-w>
[5-9]: <https://en.wikipedia.org/wiki/Raspberry_Pi>
[5-10]: <https://thepihut.com/blogs/raspberry-pi-roundup/raspberry-pi-comparison-table>

### based on this table with the comparisons which one should i pick for my project?

Based on the table you have and considering your project requirements—controlling valves with Raspberry Pi Pico microcontrollers, with 220V power available, using RF modules (no Wi-Fi), and possibly some remote communication—here’s how to choose:

#### Key points from your project needs

- You want a **small, reliable microcontroller** to control relays for valves.
- You want a **wireless communication method without Wi-Fi** (e.g., 433/915 MHz RF).
- You have **mains power (220V)** so you can use power converters as needed.
- Wireless connectivity like Wi-Fi or Bluetooth is *not* a must.
- Programming ease and stable firmware operation is important.
- You prefer good price-performance balance.

#### Recommendations from the table and context

| Model                      | Why consider it for your project                                                | Caveats                                      |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------------------|
| **Raspberry Pi Pico (original)**  | Lowest cost (~€4.5-5), sufficient for controlling relays and basic tasks. No wireless onboard, so perfect if you *don’t* want Wi-Fi or Bluetooth. | No wireless means you must add RF module for wireless communication (which you plan), straightforward. |
| **Raspberry Pi Pico H**            | Same as original but with pre-soldered headers for easier prototyping/debugging. Slightly higher price (~€7.5). | Same as original, just easier to use physically. |
| **Raspberry Pi Pico W / WH**       | Adds Wi-Fi 2.4 GHz. Higher price (~€6.3-9). Since you don’t want Wi-Fi, this might be unnecessary overhead. | Wi-Fi module consumes more power and may complicate firmware if unused. But has more community and library support for wireless IoT projects. |
| **Raspberry Pi Pico 2**             | Newer CPU (Cortex-M33 at 150MHz), more SRAM (520KB). Good if you want more performance and complex code. No wireless onboard. Price ~€7. | Useful if you want better performance, but original Pico is sufficient for valve control tasks. |
| **Raspberry Pi Pico 2 W**           | Like Pico 2 but with Wi-Fi. Similar tradeoff as Pico W but better performance. | If you don’t need Wi-Fi, extra wireless features may be unnecessary and higher price. |
| **Raspberry Pi Pico W5**            | Latest, most powerful, Wi-Fi + Bluetooth 5.3, USB-C. Highest price, flash and memory. | Overkill for your described use case without Wi-Fi/Bluetooth demands. |

#### Overall Suggestion

For your automatic garden valve project, controlling valves over 433/915 MHz RF modules (no Wi-Fi), the **original Raspberry Pi Pico** or **Raspberry Pi Pico H (with pre-soldered headers)** is the best fit:

- Sufficient processing power and I/O.
- Cheapest option.
- No wireless hardware overhead you don’t want.
- Easy to pair with RF modules and relay drivers.
- Simpler software stack focused on RF communication.
- Easy power requirements (3.3V logic, low power).

If you prefer easier prototyping and solderless breadboard usage, **Pico H** is better, but if you are comfortable soldering headers, the original Pico saves some money.

If you foresee needing wireless features (Wi-Fi/Bluetooth) in the future, you could consider the Pico W or Pico 2 W, but for your current scope, these add features you don’t need.

If you want, I can help you with a detailed bill of materials, wiring, and software tailored for the original Pico + RF modules setup.

Would you like that?

[6-1]: <https://www.seeedstudio.com/blog/2021/03/26/10-raspberry-pi-pico-projects/>
[6-2]: <https://zaitronics.com.au/blogs/guides/raspberry-pi-pico-2-w-vs-original-pico-w-comprehensive-guide>
[6-3]: <https://www.briandorey.com/post/pi-pico-lora-remote-soil-monitor>
[6-4]: <https://www.elecrow.com/blog/pico-w5-vs-pi-pico-2w-compare-specs-features-and-applications.html>
[6-5]: <https://circuitdigest.com/article/raspberry-pi-pico-variants-comparison>
[6-6]: <https://chollinger.com/blog/2021/04/raspberry-pi-gardening-monitoring-a-vegetable-garden-using-a-raspberry-pi-part-1/>
[6-7]: <https://openelab.io/blogs/learn/esp32-vs-arduino-vs-raspberry-pi-pico-which-is-better>
[6-8]: <https://forums.raspberrypi.com/viewtopic.php?t=370955>

### my water lines are currently being fed water from a water pump, it's not using a hose. here are the sizes of my gardens
<!--
- 1m x 23m
- 12m x 23m
- 1.5m x 10m
- 5m x 11m
- 5m x 12m
- 9m x 12m
- 7m x 9m

Total area = 1 x 23 + 12 x 23 + 1.5 x 10 + 5 x 11 + 5 x 12 + 9 x 12 + 7 x 9 = 23 + 276 + 15 + 55 + 60 + 108 + 63 = **600 m²**

Need at least 7 valves and possibly, only 4 RPI Pico to control the valves (some gardens are closer and can share the same RPI Pico).

Which kind of valves and relays would be recommended? -->

Given your garden sizes totaling about 600 m² and your plan to control at least 7 valves distributed over 4 Raspberry Pi Pico controllers (some sharing valves due to proximity), here are my recommendations for valves and relays suited for your 220V AC water pump-fed irrigation system:

#### Recommended Valve Type

Since your valves will be powered by **220V AC mains**, you should use **220V AC solenoid valves** designed for irrigation. These valves handle mains voltage directly and are commonly used in larger or industrial garden irrigation systems.

- **Characteristics to look for:**
  - Rated for **220V AC coil voltage**, 50/60 Hz
  - Size: **3/4" (DN20)** or **1/2"** brass or corrosion-resistant body (depending on pipe size)
  - Normally Closed (NC) type, so flow stops if power is lost (fail-safe)
  - Suitable for water, irrigation temperatures (<50°C typical)
  - Compatible thread types (e.g., BSP or G)

Examples:

- 220V AC brass solenoid valves with 3/4" or 1/2" BSP threaded connections
- Nylon or reinforced plastic valve bodies for corrosion resistance in irrigation
- Brands or generic models with proven in irrigation use (Hunter PGV, Rain Bird equivalents, or industrial valves like the AFK 300PH model)

#### Recommended Relay Modules

Since valves run on 220V AC mains, your relay modules must:

- Have contacts rated at least **10A at 250V AC** to safely switch the mains water valve coil
- Include **opto-isolated inputs** for microcontroller protection
- Operate with 5V coil voltage (powered from your 5V supply for the Pico)
- Have LED indicators for relay status (helpful for debugging)
- Can be single or multi-channel depending on how many valves you want to control per board (e.g., 1-, 4-, or 8-channel relay boards)

Recommended relay modules include:

- 5V 1-channel opto-isolated relay modules (common and cheap)
- SB-Components 4-channel relay board designed for Pico
- Waveshare 8-channel relay board
- Solid state relays (SSRs) rated for 220V AC 10A+ (optional quieter, longer life)

#### Valve and Relay Sizing for Your Project

| Item                  | Recommendation                         | Explanation                                    |
|-----------------------|--------------------------------------|-----------------------------------------------|
| **Valve Size**         | 1/2" or 3/4" (DN15 or DN20)          | Suitable flow for typical garden irrigation zones; choose based on your pipe size |
| **Valve Coil Voltage** | 220V AC                             | Matches your mains power; no transformer needed |
| **Valve Type**         | Normally Closed solenoid valve        | Fail-safe operation (valve closes on power loss) |
| **Relay Rating**       | 5V coil, 10A/250V AC contacts         | Safe switching of mains voltage valve coil    |
| **Relay Quantity**     | At least 7 channels (can be multiple modules or multi-channel module) | One channel per valve |
| **Number of Picos**    | 4                                   | Distribute valve control, group nearby valves |

#### Additional Notes

- Connect valve power via relay contacts: *relay COM* to 220VAC live line, *relay NO* to valve coil live input, valve neutral directly to AC neutral.
- Supply the 5V relay coil and Pico from a stable 5V DC regulated power supply (e.g., USB adapter).
- Use your waterproof IP65 boxes to house the Pico, relay(s), and wiring, with proper cable glands for mains wiring.
- Use adequately sized wiring rated for 220V AC and protected by fuses or breakers.
- Test each valve line manually before automation.

#### Summary

A typical setup per valve:

- **220V AC normally closed solenoid valve** (1/2" or 3/4")
- Controlled by **5V coil relay (10A 250V AC contacts)** triggered by Raspberry Pi Pico GPIO
- Pico and relay coil powered by 5V DC power supply
- Water pump feeds the water line directly before valves

This approach is robust, commonly used in irrigation systems fed by pumps, and matches your mains power availability.

If you want, I can recommend specific product models/links for valves and relays, or help make a wiring diagram for your multi-Pico/valve control setup.

Would you like that?

[7-1]: <https://www.alibaba.com/product-detail/220VAC-DN40-1-5-inch-Normally_1600146204826.html>
[7-2]: <https://www.szwofly.com/quality-11404722-solenoid-valve-irrigation-3-inch-water-control-solenoid-valve-220v-ac>
[7-3]: <https://dvs-beregnung.de/solenoid-valves-irrigation>
[7-4]: <https://yqfxdq.en.made-in-china.com/product/JwcAmrLHquhI/China-220V-AC-Garden-Irrigation-Control-Valve-Washing-Machine-Auto-Solenoid-Valve.html>
[7-5]: <https://www.aliexpress.com/item/1005004689283473.html>
[7-6]: <https://www.aliexpress.com/i/1005004689283473.html>
[7-7]: <https://www.szwofly.com/sale-11404777-irrigation-system-3-4-garden-irrigation-water-solenoid-valve-220v-ac.html>

### how expensive can this approach be compared to the one where we use a transformer for lower voltage relays?

Comparing the **cost of controlling garden valves powered directly by 220V AC using relays rated for mains voltage** versus using a **transformer to step down voltage (e.g., to 24V AC) and then using low voltage relays and valves**, here are the key points:

#### 1. Cost Factors for Direct 220V AC Relay Switching

- **Valves**: Require solenoid valves rated for 220V AC coil voltage, which tend to be more expensive and less commonly used in home irrigation but available for industrial use.
- **Relays**: Need to be high-quality, opto-isolated, **5V coil relay modules rated for 10A at 250V AC**—these are common and moderately priced in DIY automation.
- **Power Supplies**: Only need a 5V DC supply for the Pico and relay coil, no valve transformer needed.
- **Installation & Safety**: Precautions and insulation costs might be higher due to working with mains voltage directly.
- **Overall hardware complexity**: Lower because no transformer needed.

#### 2. Cost Factors for Transformer + 24V AC Valve Setup

- **Valves**: Use widely available standard **24V AC solenoid valves**, generally less expensive, widely supported, and designed specifically for irrigation.
- **Relays**: Can be simpler, lower voltage rated relay modules (5V coil, 10A contacts at 24V AC), which are often cheaper and more common due to irrigation market demand.
- **Transformer**: Must purchase a **230V AC to 24V AC transformer or AC-DC power supply**, typically costing anywhere from €20 to €60 depending on power rating and quality.
- **Power Supplies**: The 5V DC supply for the Pico and relays is still needed.
- **Installation & Safety**: Lower risk with low voltage valve power; installation may be simpler and safer.
- **System complexity**: Slightly higher due to transformer and need to manage multiple power supplies.

#### 3. General Cost Comparison

- The **direct 220V AC approach eliminates the transformer cost** but may increase valve costs (220V AC valves often pricier) and requires careful safety and wiring protocols increasing labor costs.
- The **transformer + 24V AC valve approach spreads cost into cheaper valves and relays but adds upfront transformer/power supply cost**.
- For small to medium scale installations (like your garden sizes with 7 valves), transformer cost (~€20-60) amortized across the system might bring greater overall value and ease.
- Lower voltage parts (valves, relays) are often more readily available and modular for future maintenance or scaling.

#### Summary Table

| Cost Component            | 220V AC Valve + Relay Approach                 | 24V AC Valve + Transformer + Relay Approach  |
|---------------------------|-----------------------------------------------|-----------------------------------------------|
| Valve Cost                | Higher (220V AC-rated valves cost more)        | Lower (standard 24V AC valves cheaper)          |
| Relay Cost                | Moderate (5V relay with 10A 250V AC contacts)  | Lower/Moderate (5V relay with 10A 24V AC contacts)|
| Transformer Cost          | None                                           | Additional €20-60 for 230V→24V transformer       |
| Power Supplies            | 5V DC supply only                              | 5V DC supply + transformer-based 24V supply     |
| Wiring & Safety           | Higher installation cost for proper mains wiring | Safer, possibly lower installation cost          |
| Total Hardware Cost       | Slightly lower, but valves pricier              | Possibly higher upfront due to transformer but valves cheaper |

#### Recommendation for Your Case

- If you want **simpler hardware with fewer components and confident working safely with 220V AC** wiring, direct 220V AC valves plus 220V rated relays is viable and less expensive upfront.
- If you prefer **greater safety, more common irrigation hardware, and flexibility**, use the **transformer + 24V AC valves** approach despite the extra transformer cost.
- For a typical home or garden system around 600 m² with 7 valves, the **24V AC transformer setup is industry common and recommended for safety and reliability**, with total cost difference usually not very large after considering valve prices.

#### References and Context

- 24V AC power supplies are standard for irrigation control, safe, and widely compatible [1][8-1] .
- Transformers add a moderate hardware cost but reduce safety risk and improve compatibility [1][8-1] [2][8-2] .
- 220V AC valves are specialty and tend to be costlier [4][8-4] [7][8-7] .
- Relays with 10A 250V AC ratings are common and moderately priced for both setups.

If you'd like, I can help estimate exact price ranges from vendors for valves, relays, and transformers for both approaches to give a detailed cost comparison.

Would you like that?

[8-1]: <https://floraflex.com/default/blog/post/understanding-the-benefits-of-24v-ac-dc-power-supply-for-irrigation-control>
[8-2]: <https://www.gbcma.vic.gov.au/downloads/Farm_Water_Program/2019%20-%20Comparison%20of%20irrigation%20system%20costs.pdf>
[8-3]: <https://www.powerstream.com/24vac-tutorial.htm>
[8-4]: <https://www.chinasolenoidvalve.com/info/what-is-the-difference-between-24v-dc-solenoid-57061621.html>
[8-5]: <https://tameson.com/pages/solenoid-valve-voltage>
[8-6]: <https://www.inverter.com/dc-pump-is-better-than-ac-pump-in-solar-pumping-system>
[8-7]: <https://plumberstar.com/difference-between-ac-and-dc-solenoid-valve/>
[8-8]: <https://rayshobby.net/wordpress/understanding-24vac-sprinkler-valves/>
[8-9]: <https://www.solenoidvalvefactory.com/news/what-is-the-difference-between-24v-dc-solenoid-valve-and-220v-ac-solenoid-valve-how-do-we-choose.html>

### can you please do a side by side comparison, maybe in a table with links to the parts needed for both approaches?

Here is a **side-by-side comparison table** of the two approaches for your garden valve automation project powered by mains in Portugal: **(A) Direct 220V AC Valve with 220V AC Relay and (B) Transformer + 24V AC Valve with Low Voltage Relay**, including recommended parts and links where available.

| Aspect                      | **A: Direct 220V AC Valve + 220V AC Relay**                                                            | **B: Transformer + 24V AC Valve + Low Voltage Relay**                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------|
| **Valve Type / Voltage**     | 220V AC solenoid valve (Industrial/irrigation grade)                                                  | 24V AC solenoid valve (common irrigation valves)                                             |
| **Typical Valve Example**    | Generic brass 220V AC solenoid valves (3/4" or 1")                                                    | Hunter PGV-101 series, Rain Bird 24V AC valves                                               |
| **Valve Cost**               | Higher (~€20-50+ per valve) [Search suppliers locally or industrial irrigation valve vendors]          | Lower (~€15-30 per valve) [Example: Hunter PGV, Rain Bird available on Amazon/eBay]          |
| **Relay Requirements**       | 5V coil relay module, contacts rated 10A 250VAC, opto-isolated                                         | 5V coil relay module, contacts rated 10A 24VAC (lower voltage contact rating)                 |
| **Recommended Relay Modules**| - [SB-Components 4-Channel Relay Board (10A@250V)](https://botland.store/raspberry-pi-pico-other-modules/19629-raspberry-pi-pico-relay-board-5055652921178.html) - 1-channel 10A 250V relay modules on Amazon or The Pi Hut | - Same relay boards can be used if contacts rated for 10A or more at 24VAC - Cheaper relay modules common for 24V AC |
| **Power Supplies Needed**    | - 5V DC power supply for Pico and relay coils (e.g., USB adapter ~€5-10) - No valve transformer needed | - 5V DC supply for Pico and relays - **230V AC → 24V AC transformer** (e.g., €20-60 depending on quality/power) |
| **Transformer Examples**     | Not needed                                                                                             | - [Premium 230V to 24V AC Transformer](https://www.conrad.com/p/traco-power-tsw-20-124-ac-dc-power-supply-unit-24-v-ac-20-va-20000-ma-1428907) - Local electronics stores or irrigation suppliers |
| **Wiring Complexity**        | Mains wiring at relay contacts, relay coil low voltage                                                  | Additional wiring for transformer, safer low voltage valve wiring                            |
| **Safety Considerations**    | Requires strict isolation and electrical safety measures due to mains voltage switching                 | Safer low voltage valve wiring after transformer; easier compliance for outdoor/garden use |
| **Noise and Reliability**    | AC valves noisier and coil heats up; risk of coil burn if energized too long (typical for 220V AC coils) | 24V AC valves quieter, lower risk of coil overheating                                        |
| **Typical Use Cases**        | Industrial, larger scale irrigation requiring direct mains switching                                   | Home and commercial irrigation, common and proven approach                                  |
| **Total Hardware Cost Estimate (7 valves)** | Valves (~€140-350) + Relays (~€30-50) + 5V supply €5-10 = ~€175-410                         | Valves (~€105-210) + Relays (~€30-50) + Transformer (~€20-60) + 5V supply €5-10 = ~€160-330  |
| **Example Valve Links**      | - [Generic 220V AC Brass Solenoid Valve](https://www.amazon.es/dp/B07XJX2GMW) - Check local suppliers for higher-quality valves                      | - [Hunter PGV-101 24V AC Valve on Amazon](https://www.amazon.com/Hunter-PGV-101G-3-4-Inch-Irrigation/dp/B005K01H7K) - [Rain Bird 24V Valve](https://www.amazon.com/Rain-Bird-100DVF-Valve-3-4-Inch/dp/B00064V23I) |
| **Typical Relay Links**      | - [SB-Components Relay Board](https://botland.store/raspberry-pi-pico-other-modules/19629-raspberry-pi-pico-relay-board-5055652921178.html) - [The Pi Hut Single Channel Relay](https://thepihut.com/products/single-channel-relay-for-raspberry-pi-pico) | Same relay modules as above, suitable for low voltage switching as well                      |
| **Recommended Transformer**  | Not applicable                                                                                         | - 230V to 24V AC transformer, 20-40VA typically for multiple valves - Example: [Find on Mouser Portugal or local electronics store] |

#### Summary and Recommendation

- **Option A (Direct 220V AC Valve + Relay):**
  Less hardware, no transformer needed, but valves cost more, relay and wiring require careful mains safety handling, and valves can be noisier and heat more. Good if you want minimal components and can ensure safe mains wiring.

- **Option B (Transformer + 24V AC Valve + Low Voltage Relay):**
  Safer low voltage valve wiring, more common and less expensive valves, slightly higher cost from transformer and more wiring. Widely used in irrigation systems with better reliability and safety balance.

If you want, I can help prepare detailed wiring diagrams, code examples, or sourcing lists for either approach. Would you prefer that?

[9-1]: <https://www.solenoidvalvefactory.com/news/what-is-the-difference-between-24v-dc-solenoid-valve-and-220v-ac-solenoid-valve-how-do-we-choose.html>
[9-2]: <https://www.chinasolenoidvalve.com/info/what-is-the-difference-between-24v-dc-solenoid-57061621.html>
[9-3]: <https://community.home-assistant.io/t/simple-reliable-sprinkler-controller-with-esphome/628838>
[9-4]: <https://www.raphael-valves.com/fileadmin/user_upload/cat_raf_irrigation_2023_en.pdf>
[9-5]: <https://plumberstar.com/difference-between-ac-and-dc-solenoid-valve/>
[9-6]: <https://tameson.com/pages/solenoid-valve-voltage>
[9-7]: <https://baccara-geva.com/wp-content/uploads/2023/09/Baccara-IRRIGATION-Catalog-2023-23072023.pdf>
[9-8]: <https://www.bermad.com/app/uploads/IR_Product-Guide_English_1-2020.pdf>
[9-9]: <https://www.netafim.com/en-ae/bynder/F2A27B8A-8459-4C90-9164DF36CD69F31B-valves-catalog.pdf>
[9-10:] <https://www.hunterirrigation.com/sites/default/files/Hunter_Catalog_US.pdf>

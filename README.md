# Smart Gate Opener

A retrofit IoT solution to automate an existing gate system using an **ESP8266 relay board** with **Tasmota firmware**. This design adds **smart control via Home Assistant and Apple HomeKit** without removing the original control board, ensuring that the traditional remote control still functions normally.

---
## Acheivement

- Added **smart control** to existing gate system without replacing the control board.
- Integrated with **Home Assistant and Apple HomeKit** for remote access and automations.
- Preserves original **RF remote functionality** while enabling Wi-Fi control.
- Uses **GPIO sensing** to detect gate open/close state via external light relay.
- Solved poor RF range issue and improved convenience for parcel deliveries.

---

## Overview

This project adds smart control to my home's gate system. Instead of replacing the gate controller, I used an ESP8266 with a relay to **simulate a physical button press**, preserving full compatibility with the original remote control. Additionally, I integrated a **state detection mechanism** by reading a GPIO signal from the gate's lighting relay, which indicates whether the gate is open or closed.

**Motivation:**
- Frequent parcel deliveries when I am not home.
- Poor remote control range (car window tint blocks RF signal).
- No need to wait for gate openeing while arriving home.

By integrating the gate into **Home Assistant** and **Apple HomeKit**, I can now open/close the gate from my phone and check its status remotely.

---

## Features

- **Retrofit design:** No modification to the original gate control board; remote control continues to work.
- **Smart control:** Open/close the gate via Home Assistant dashboard or Apple HomeKit.
- **State detection:** GPIO input senses when the gate is open (using the external lights relay signal).
- **Secure integration:** Local control via Wi-Fi and MQTT, no dependency on cloud services.
- **Convenience for deliveries:** Gate can be opened remotely for parcel drop-offs.

---

##System Architecture

### Hardware

- **ESP8266 Relay Board**
  - Relay simulates physical gate button press.
  - GPIO input connected to external light relay output for state detection.
- **Existing Gate Control Board**
  - Remains intact, remote control still functional.

*(Add wiring diagram here)*


### Software

- **Tasmota firmware** on ESP8266
  - Relay control via MQTT or HTTP commands
  - GPIO pin configured as input for state sensing
- **Home Assistant integration**
  - Exposes gate control as a switch/cover entity
  - Automations for gate open notifications
- **Apple HomeKit**
  - Integrated via Home Assistant’s HomeKit bridge

---

## How It Works

1. **Gate control:**
   - Sending an MQTT command toggles the relay, simulating the gate button.
2. **Gate state detection:**
   - When the gate opens, the controller’s external light relay activates.
   - This signal is read via a GPIO pin (pulled to ground) to determine gate status.

---

## Dashboard Integration

- **Home Assistant:** Lovelace card to open/close gate and view status.
- **Apple HomeKit:** Gate appears as an accessory in the Home app.

*(Insert screenshots of dashboard and HomeKit integration here)*

<!--
---

## Skills Demonstrated

- ESP8266 hardware integration
- Tasmota relay configuration and GPIO input sensing
- MQTT and Home Assistant automation
- Retrofit design preserving legacy functionality
- HomeKit integration via Home Assistant

---
-->

## Project Impact

- Ability to remotely control the gate for deliveries and visitors.
- Eliminated frustrations with poor RF remote range.
- Added real-time status monitoring (open/closed).
- Achieved full smart home integration without replacing existing hardware.

---

## Repository Contents

- Wiring diagrams
- Tasmota configuration details
- Home Assistant YAML snippets
- Dashboard screenshots

---

## License

Open-source under MIT License.

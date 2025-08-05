# Smart Gate Opener

A retrofit IoT solution to automate an existing gate system using an **ESP8266 relay board** with **Tasmota firmware**. This design adds **smart control via Home Assistant and Apple HomeKit** without removing the original control board, ensuring that the traditional remote control still functions normally.

---
## Achievement

- Added **smart control** to existing gate system without replacing the control board.
- Integrated with **Home Assistant and Apple HomeKit** for remote access, real-time status monitoring (open/closed) and automations.
- Preserves original **RF remote functionality** while enabling Wi-Fi control.
- Uses **GPIO sensing** to detect gate open/close state via external light relay.
- Solved poor RF range issue and improved convenience for parcel deliveries.

---

## Overview

This project adds smart control to my home's gate system. Instead of replacing the gate controller which is expensive and wasteful, I used an ESP8266 with a relay to **simulate a physical button press**, preserving full compatibility with the original remote control. Additionally, I integrated a **state detection mechanism** by reading a GPIO signal from the gate's lighting relay, which indicates whether the gate is open or closed (Depending on the control board model, you mileage may vary).

## Motivation
- Frequent parcel deliveries when I am not home.
- Poor remote control range (car window tint blocks RF signal).
- No need to wait for gate openeing while arriving home.
- Wanted a **low-cost** solution that doesn't affect original funtionality, ensuring other family members can continue using it in their preferred way.

By integrating the gate into **Home Assistant** and **Apple HomeKit**, I can now open/close the gate from my phone and check its status remotely.

---

## Features

- **Retrofit design:** No modification to the original gate control board; remote control continues to work.
- **Smart control:** Open/close the gate via Home Assistant dashboard or Apple HomeKit.
- **State detection:** GPIO input senses when the gate is open (using the external lights relay signal).
- **Secure integration:** Local control via Wi-Fi and MQTT, no dependency on cloud services.
- **Convenience for deliveries:** Gate can be opened remotely for parcel drop-offs.

---

## System Architecture

### Hardware

- **ESP8266 Relay Board with 220v power supply**
  - Relay simulates physical gate button press.
  - GPIO input connected to external light relay output for state detection.
  - This avoids needing an external power adapter and keeps the installation compact.
- **Existing Gate Control Board**
  - Remains intact, remote control still functional.
<!--
*(Add wiring diagram here)*
-->
ESp8266 Relay Board           |
:-------------------------:|
<img width="512" height="468" alt="ESP8266-4-Relay" src="https://github.com/user-attachments/assets/75a68d3e-11a3-40da-92f4-01e3e778809b" /> |

> This is just for sample as Gate controller board is different depending on make and model

Existing Gate Controller Board      | Wiring Manual
:-------------------------: | :-------------------------: |
<img width="500" height="450" alt="image" src="https://github.com/user-attachments/assets/e399989b-fe38-4427-b14e-a9ad72f52b43" /> | <img width="500" height="450" alt="image" src="https://github.com/user-attachments/assets/45b86306-57a5-43ea-a3cc-9c24f10ce463" />

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

## Wiring & Setup

### Before installation

1. Flash Tasmota firmware on the ESP8266
2. Configure relay control in Tasmota (assign GPIOs and verify toggle functionality)
3. Setup inching mode on the relay (e.g., 0.5–1 second) to simulate a momentary button press and release.  
4. Set up MQTT.
5. Integrate into Home Assistant via MQTT and ensure all is working.

### Installation 

> **⚠️ Safety Warning**  
> Working with mains electricity can be dangerous. Ensure the power is switched off before wiring. If unsure, turn off the **main breaker** (note: this cuts power to the entire house).

#### Steps

1. **Power Off**
   - Turn off the power / breaker for the gate.  
   - Optionally, turn off the main breaker for full isolation.

2. **Power the ESP8266**
   - Connect the ESP8266 board directly to the mains input of the gate controller, as the board includes a built‑in power supply for 220V AC.  

2. **Connect Relay Control**
   - Wire the relay output (COM and NO) in parallel with the existing physical gate button input.  
   - Remember to configure relay **inching mode** in Tasmota so it briefly activates (momentary press) instead of staying latched.

3. **Connect State Detection**
   - Wire the auxiliary relay output (from the gate controller) to an ESP8266 **GPIO input**.  
   - When the auxiliary relay activates (gate open), the GPIO is pulled low, allowing it to detect gate status.

4. **Secure and Test**
   - Ensure all connections are insulated and secured.  
   - Restore power and test both:
     - **Manual control** (original remote/button still works)  
     - **Smart control** (via Home Assistant/MQTT)  
     - **Inching timing** (verify the relay press duration correctly triggers the gate without multiple activations)

Sample wiring            |
:-------------------------:|
<img width="430" height="391" alt="sample-wiring" src="https://github.com/user-attachments/assets/e46446d4-92ff-4ed9-a70a-029e3fcf4cf2" /> |

---

## How It Works

### 1. Gate Control
- Sending an **MQTT command** toggles the relay on the ESP8266.  
- This simulates pressing the physical gate button, triggering the gate to open or close.

<!-- 2. **Gate state detection:**
   - My existing gate controller can be set / program to have a relay triggered when the gate is open for turning on the light at night originally, it senses the night time by using a photoelectric sensor, so I have to cover up the sensor for it to send relay trigger signal every time.
   - When the gate opens, my existing gate controller’s external light relay activates.
   - This signal is read via a GPIO pin (pulled to ground) to determine gate status.

-->

### 2. Gate State Detection

The original gate controller includes a built-in feature to trigger an **auxiliary relay** when the gate is open. This relay was originally intended to control an external light and can be configured to either:

- Trigger every time the gate opens, or  
- Trigger only at night using the built-in **photoelectric sensor**.

To repurpose this feature for state detection:

- I connected the auxiliary relay output to an **ESP8266 GPIO input**.  
- When the relay activates (gate open), the GPIO pin is pulled low (connected to ground).  
- Home Assistant reads this signal to determine whether the gate is open or closed.  

> The **original light control function** can still be used if desired as another relay can be triggered based on the detected gate state.

---

## Dashboard Integration

- **Home Assistant:** Lovelace card to open/close gate and view status.
- **Apple HomeKit:** Gate appears as an accessory in the Home app.


Home Assistant            | Apple Homekit
:-------------------------:| :-------------------------:
<img width="414" height="91" alt="Screenshot 2025-08-05 at 2 43 11 PM" src="https://github.com/user-attachments/assets/209af6be-23f5-4b01-8948-4bce8bd2d39b" />  | <img width="165" height="78" alt="Screenshot 2025-08-05 at 2 44 17 PM" src="https://github.com/user-attachments/assets/f8070991-a76e-49cd-8b4e-baba8986624e" />
<img width="591" height="572" alt="Screenshot 2025-08-05 at 2 43 25 PM" src="https://github.com/user-attachments/assets/af5d654c-a18c-4dcf-ad95-fa18d494c534" /> | <img width="296" height="599" alt="Screenshot 2025-08-05 at 2 44 09 PM" src="https://github.com/user-attachments/assets/4c6053f0-8363-45f4-a71e-fe4f88ebe116" />



<!-- *(Insert screenshots of dashboard and HomeKit integration here)* -->

<!--
---

## Skills Demonstrated

- ESP8266 hardware integration
- Tasmota relay configuration and GPIO input sensing
- MQTT and Home Assistant automation
- Retrofit design preserving legacy functionality
- HomeKit integration via Home Assistant

---


## Repository Contents

- Wiring diagrams
- Tasmota configuration details
- Home Assistant YAML snippets
- Dashboard screenshots
-->
---

## Sample YAML for Home Assistant
```
#If you already have other sensor in config, no need to add in the "template:" just continue 
template:
    - sensor:
      - name: "gate"
        state: >
          {% if is_state('binary_sensor.gate_sensor', 'off') and is_state('switch.tasmota', 'on') %}
            opening
          {% elif is_state('binary_sensor.gate_sensor', 'on') and is_state('switch.tasmota', 'on') %}
            closing
          {% elif is_state('binary_sensor.gate_sensor', 'on') %}
            open
          {% else %}
            closed
          {% endif %}

cover:
  - platform: template
    covers:
      gate:
        device_class: gate
        friendly_name: "Gate"
        value_template: "{{ states('sensor.gate') }}"
        open_cover:
          - condition: state
            entity_id: sensor.gate
            state: "closed"
          - service: switch.toggle
            target:
              entity_id: switch.gate
        close_cover:
          - condition: state
            entity_id: sensor.gate
            state: "open"
          - service: switch.toggle
            target:
              entity_id: switch.gate
        stop_cover:
          service: switch.toggle
          target:
            entity_id: switch.gate

```            
            


## License

Open-source under MIT License.

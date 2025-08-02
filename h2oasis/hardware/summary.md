# H2Oasis Hardware Plan – Executive Summary

## Project Vision

Design a robust, app-controlled automatic irrigation system for a 600 m² garden, using a central API and microcontrollers to manage multiple electro-valves per zone. The system must be reliable, weatherproof, support manual override, and be safe for home installation.

---

## Key Requirements

- **Zones & Scale:** 7+ valves, grouped into 4 zones (systems), each managed by a Raspberry Pi Pico.
- **Control:** Each valve can be opened/closed remotely via app/API, or manually at the valve.
- **Power:** System can use either direct 220V AC or safer 24V AC with a transformer.
- **Communication:** Wireless control via 433 MHz or 915 MHz RF modules (no Wi-Fi required).
- **Safety:** All electronics must be housed in IP65 waterproof enclosures, with proper cable management and electrical isolation.

---

## Hardware Recommendations

- **Microcontroller:** Raspberry Pi Pico (original or H) for each zone; Pico W/2W only if Wi-Fi is needed in the future.
- **Valves:** 24V AC solenoid valves (Hunter, Rain Bird, Irritrol) are recommended for safety, cost, and availability. 220V AC valves are possible but require stricter safety.
- **Relays:** Opto-isolated, 5V coil, rated for the valve voltage (10A 250V AC for 220V, or 10A 24V AC for 24V).
- **Power Supplies:** 24V AC transformer for valves; 5V DC supply for Pico and relays.
- **RF Modules:** 433 MHz or 915 MHz transmitter/receiver kits for Pico-to-Pico or Pico-to-central controller communication.
- **Enclosures & Wiring:** IP65 boxes, outdoor-rated cables, cable glands, and fuses/breakers for safety.

---

## Shopping List (Example)

- 4× Raspberry Pi Pico
- 433 MHz RF kit
- 4× IP65 waterproof boxes
- 24V AC transformer (if using 24V valves)
- Relay modules (multi-channel or single, as needed)
- 7+ solenoid valves (1/2" or 3/4", normally closed)
- 5V DC power supply for each controller
- Jumper wires, connectors, terminal blocks, wire antennas

---

## System Design & Implementation

- Each Pico controls a set of valves via relays, receives commands wirelessly, and switches AC power to the valves.
- Central controller (or app) sends commands via RF; each Pico listens for its zone’s commands.
- Manual override is available on most commercial valves for maintenance or emergency use.
- All electronics are protected in waterproof enclosures and use safe, outdoor-rated wiring.

---

## Valve & Relay Sizing

- Choose valve size based on pipe/garden zone (1/2" or 3/4" typical).
- Relays must match the coil voltage and current of the valves.
- Distribute valves among Picos based on garden layout and proximity.

---

## Safety & Best Practices

- Use opto-isolated relays for electrical safety.
- Ensure all mains wiring is properly insulated and protected.
- Test each valve circuit manually before full automation.
- Use fuses or breakers for added protection.

---

## Cost & Approach Comparison

| Aspect         | Direct 220V AC Approach         | 24V AC + Transformer Approach         |
|----------------|--------------------------------|--------------------------------------|
| Valve Cost     | Higher, less common             | Lower, widely available              |
| Relay Cost     | Moderate (10A 250V AC)         | Lower (10A 24V AC)                   |
| Transformer    | Not needed                     | Required (€20-60)                    |
| Safety         | Requires strict mains isolation | Safer, industry standard             |
| Maintenance    | More complex                   | Easier, modular                      |

**Recommendation:** For most home gardens, 24V AC valves with a transformer and low-voltage relays are safest, most reliable, and easiest to maintain.

---

## RF Communication & Example Code

- Use 433/915 MHz RF modules for wireless control.
- Simple encoding/decoding can be done in MicroPython; for robust operation, consider proven libraries or protocols.
- Example transmitter/receiver code and wiring diagrams are provided in the plan for quick prototyping.

---

## Valve & Relay Selection

- Use normally closed, manual override valves for safety and flexibility.
- Choose relay modules with opto-isolation and sufficient current/voltage rating.

---

## Pico Model Selection

- Original Pico or Pico H is recommended for cost and simplicity.
- Pico W/2W only if you need Wi-Fi/Bluetooth in the future.

---

## Final Notes

- The plan includes detailed Q&A, cost comparisons, and links to recommended parts.
- The approach is modular, scalable, and based on proven irrigation and automation practices.
- If you need wiring diagrams, code samples, or a bill of materials, see the relevant sections or ask for a tailored guide.

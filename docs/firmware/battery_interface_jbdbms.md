# JBD BMS (Jin Biao Da) Battery Interface

JBD BMS (Jin Biao Da Battery Management System) is supported through a UART connection. JBD BMS is a popular battery management system used in lithium battery packs, often rebranded by various manufacturers.

## Overview

JBD BMS units communicate using a proprietary protocol over UART (Universal Asynchronous Receiver-Transmitter). OpenDTU-OnBattery can read battery telemetry data including state of charge, voltage, current, and cell balancing information.

## Supported Models

JBD BMS is used in many battery systems and is often rebranded. Common brands that use JBD BMS include:

- Daly BMS (some models)
- Various Chinese lithium battery manufacturers
- DIY battery builders using JBD-compatible BMS boards

## Connection

### Hardware Requirements

- ESP32 with available UART pins
- Level shifter (3.3V to 5V or 3.3V to 3.3V depending on your BMS model)
- Appropriate cables for your specific JBD BMS model

### Wiring

```
JBD BMS    ESP32
------     -----
TX      -> RX (GPIO pin)
RX      -> TX (GPIO pin)
GND     -> GND
```

!!! warning "Voltage Levels"
    Ensure voltage compatibility between your ESP32 (3.3V) and the JBD BMS UART interface. Some JBD BMS units operate at 5V levels and may require a level shifter.

## Configuration

### Pin Assignment

Configure the UART pins in the device profile or pin mapping section of the OpenDTU-OnBattery web interface:

1. Navigate to **Config** → **Device Profile** or **Config** → **Pin Assignment**
2. Set the appropriate pins for JBD BMS UART communication
3. Configure the baud rate (typically 9600 or 115200 - consult your BMS documentation)

### Battery Settings

1. Navigate to **Config** → **Battery Settings**
2. Set **Battery Provider** to "JBD BMS"
3. Configure polling interval (recommended: 5-10 seconds)
4. Save the configuration

## Monitored Parameters

The JBD BMS interface typically provides:

- **Basic Metrics:**
  - State of Charge (SoC) percentage
  - Battery voltage
  - Charge/discharge current
  - Battery temperature

- **Cell Information:**
  - Individual cell voltages
  - Cell balancing status
  - Maximum and minimum cell voltages

- **Protection Status:**
  - Overvoltage protection
  - Undervoltage protection
  - Overcurrent protection
  - Temperature protection
  - Short circuit protection

- **System Information:**
  - Charge/discharge cycles
  - Battery capacity
  - Remaining capacity

## Troubleshooting

### Common Issues

1. **No Data Received:**
   - Check wiring connections
   - Verify baud rate settings
   - Ensure correct UART pins are configured
   - Check if level shifter is needed

2. **Intermittent Data:**
   - Check for loose connections
   - Verify power supply stability
   - Reduce polling frequency

3. **Incorrect Values:**
   - Verify BMS model compatibility
   - Check if firmware version is supported
   - Ensure proper grounding

### Debug Information

Monitor the serial console output or web interface logs for JBD BMS communication status. Look for:

- Connection establishment messages
- Data parsing errors
- Timeout warnings

## Integration with Dynamic Power Limiter

Once configured, the JBD BMS data can be used by the [Dynamic Power Limiter](dpl.md) to:

- Prevent overcharging when battery SoC is high
- Reduce discharge when battery SoC is low
- Monitor battery health and temperature for safety

## MQTT Topics

When properly configured, JBD BMS data is published to MQTT under the battery topic structure. See [MQTT Topics](mqtt_topics.md) for complete topic reference.

## Limitations

- Communication protocol may vary between JBD BMS firmware versions
- Some advanced features may not be available on all models
- Real-time cell balancing control is read-only (monitoring only)
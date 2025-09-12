# Zendure SuperBase Battery Interface

Zendure SuperBase portable power stations are supported through UART communication. These units are popular for off-grid applications and emergency backup power.

## Overview

Zendure SuperBase units can communicate battery status, charging information, and power consumption data through a UART interface. This allows integration with OpenDTU-OnBattery for monitoring and dynamic power management.

## Supported Models

Zendure SuperBase models that support UART communication include:

- **SuperBase Pro Series:**
  - SuperBase Pro 2000
  - SuperBase Pro 1500

- **SuperBase V Series:**
  - SuperBase V4600
  - SuperBase V6400

- **Other Models:**
  - Models with external communication ports
  - Check your specific model documentation for UART support

!!! note "Communication Port"
    Not all Zendure SuperBase models include external communication ports. Verify your specific model supports UART communication before attempting to connect.

## Connection

### Hardware Requirements

- ESP32 with available UART pins
- Appropriate cable for your Zendure model (often proprietary connector)
- Level shifter if voltage levels don't match (3.3V ESP32 vs SuperBase levels)

### Wiring

Connection varies by SuperBase model. Common configurations:

```
Zendure Port     ESP32
------------     -----
TX            -> RX (GPIO pin)
RX            -> TX (GPIO pin)  
GND           -> GND
```

!!! warning "Connector Types"
    Zendure SuperBase units may use proprietary connectors. Consult your model's documentation for the correct pinout and connector type.

## Configuration

### Pin Assignment

1. Navigate to **Config** → **Device Profile** or **Config** → **Pin Assignment**
2. Configure UART pins for Zendure communication
3. Set appropriate baud rate (consult SuperBase documentation, typically 9600 or 115200)
4. Configure data format (8N1 is common)

### Battery Settings

1. Navigate to **Config** → **Battery Settings**
2. Set **Battery Provider** to "Zendure"
3. Configure polling interval (recommended: 10-30 seconds for portable units)
4. Set timeout values appropriate for your connection
5. Save the configuration

## Monitored Parameters

The Zendure interface provides comprehensive power station information:

### Battery Status
- State of Charge (SoC) percentage
- Battery voltage
- Remaining capacity (Wh)
- Total capacity (Wh)
- Battery temperature

### Power Information
- Input power (AC charging, solar charging)
- Output power (AC loads, DC loads, USB loads)
- Power consumption by port type
- Charging/discharging current

### System Status
- Operational mode (charging, discharging, standby)
- AC output status (on/off)
- DC output status
- USB port status
- Fan operation status

### Environmental Data
- Internal temperature
- Ambient temperature (if available)
- Humidity (if available)

## SuperBase-Specific Features

### Multiple Output Monitoring

Zendure SuperBase units have multiple output types:
- **AC Outlets:** 120V/230V household power
- **DC Outlets:** 12V car-style outlets  
- **USB Ports:** Various USB standards (USB-A, USB-C, etc.)
- **Wireless Charging:** Qi wireless charging pad (some models)

Each output type can be monitored separately for power consumption.

### Charging Source Detection

SuperBase units can identify charging sources:
- AC wall charging (fast charging)
- Solar panel charging (MPPT)
- Car charging (12V DC input)
- USB-C PD input (some models)

## Integration with Dynamic Power Limiter

Zendure SuperBase integration with the [Dynamic Power Limiter](dpl.md) enables:

### Smart Charging Management
- **Solar Prioritization:** Use excess solar to charge SuperBase
- **Grid Backup:** Charge from grid during low-cost periods
- **Load Balancing:** Balance house loads vs SuperBase charging

### Emergency Backup
- **Grid Outage Detection:** Automatic switch to SuperBase power
- **Critical Load Priority:** Maintain essential systems during outages
- **Battery Reserve:** Maintain minimum charge for emergencies

### Portable Use Cases
- **RV/Van Life:** Integrate with mobile solar systems
- **Remote Monitoring:** Monitor off-grid installations
- **Event Power:** Manage power for outdoor events

## Troubleshooting

### Connection Issues

1. **No Communication:**
   - Verify cable connections and pinout
   - Check baud rate and data format settings
   - Ensure SuperBase communication port is enabled
   - Test with different cable if available

2. **Intermittent Data:**
   - Check for loose connections
   - Verify power supply stability
   - Monitor for interference from SuperBase inverter
   - Reduce polling frequency

3. **Incorrect Values:**
   - Verify SuperBase model compatibility
   - Check firmware versions (both OpenDTU and SuperBase)
   - Ensure proper grounding

### SuperBase-Specific Issues

1. **Communication Port Not Responding:**
   - Some SuperBase models require activation of communication mode
   - Check SuperBase settings for external communication enable
   - Verify SuperBase is powered on and operational

2. **High Power Interference:**
   - SuperBase inverters can create electrical noise
   - Use shielded cables for communication
   - Ensure proper grounding and separation from high-power circuits

## Safety Considerations

!!! warning "Portable Power Safety"
    - Follow Zendure safety guidelines for your SuperBase model
    - Ensure proper ventilation during charging/discharging
    - Monitor temperature to prevent overheating
    - Implement appropriate protection for outdoor use

!!! note "Warranty Considerations"
    Using external communication may affect warranty. Consult Zendure support and documentation before implementation.

## MQTT Integration

SuperBase data is published to MQTT for integration with home automation systems:

```
<base_topic>/battery/zendure/
├── soc                     # State of charge (%)
├── voltage                 # Battery voltage (V)
├── capacity_remaining      # Remaining capacity (Wh)
├── capacity_total          # Total capacity (Wh)
├── power_input             # Input power (W)
├── power_output            # Total output power (W)
├── temperature             # Internal temperature (°C)
├── ac_output_enabled       # AC outlets status
├── dc_output_enabled       # DC outlets status
└── charging_source         # Current charging source
```

## Use Cases

### Off-Grid Solar Systems
- Monitor battery status in remote locations
- Optimize solar charging efficiency
- Manage power consumption during low solar periods

### Emergency Backup Systems
- Automatic switch to backup power during outages
- Maintain essential systems (internet, security, refrigeration)
- Monitor backup power duration and capacity

### Mobile Applications
- RV and van life power management
- Portable event power monitoring
- Construction site power tracking

## Limitations

- Communication protocol may vary between SuperBase models
- Some features may require specific firmware versions
- Real-time control capabilities are typically read-only
- Portable units may have different communication timing requirements
- Not all SuperBase models support external communication
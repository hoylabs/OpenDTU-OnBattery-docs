# SBS (Smart Battery System) Interface

Smart Battery System (SBS) is a standardized protocol for battery management systems that communicate over CAN bus. SBS provides a vendor-neutral interface for monitoring and managing battery systems.

## Overview

The SBS interface allows OpenDTU-OnBattery to communicate with any battery management system that implements the Smart Battery System specification. This standardized approach provides consistent battery monitoring across different manufacturers.

## SBS Protocol Specifications

SBS (Smart Battery System) is based on:
- **Communication:** CAN bus (Controller Area Network)
- **Standard:** Based on SMBus/I2C Smart Battery specifications adapted for CAN
- **Data Format:** Standardized register layout and data types
- **Vendor Neutral:** Works with multiple battery manufacturers

## Supported Systems

Any battery management system implementing SBS over CAN should be compatible, including:

### Commercial Battery Systems
- Battery systems with SBS-compliant BMS
- Industrial UPS systems with SBS interface
- Telecommunications backup power systems
- Data center battery systems

### DIY and Custom Systems
- Custom lithium battery packs with SBS-compatible BMS
- Retrofit BMS installations using SBS protocol
- Educational and research battery systems

!!! note "SBS Compliance"
    Verify that your battery system specifically supports "SBS over CAN" rather than just SBS over SMBus/I2C, as these are different implementations.

## Connection

### Hardware Requirements

- ESP32 with CAN transceiver (e.g., MCP2515 + TJA1050)
- CAN bus cable (twisted pair, typically CAT5/6 cable)
- CAN termination resistors (120Ω) if required
- Power supply for CAN transceiver (3.3V or 5V depending on module)

### CAN Bus Wiring

```
Battery System   CAN Transceiver    ESP32
--------------   ---------------    -----
CAN_H         -> CAN_H           
CAN_L         -> CAN_L           
GND           -> GND           -> GND
              -> VCC           -> 3.3V/5V
              -> CS            -> GPIO (CS pin)
              -> SI            -> GPIO (MOSI)
              -> SO            -> GPIO (MISO)
              -> SCK           -> GPIO (SCK)
```

### CAN Configuration

SBS over CAN typically uses:
- **Baud Rate:** 250 kbps (standard) or 500 kbps
- **CAN ID Range:** 0x600-0x6FF (SBS standard range)
- **Frame Format:** Standard 11-bit identifier
- **Data Length:** 8 bytes maximum per frame

## Configuration

### Hardware Setup

1. Navigate to **Config** → **Device Profile** or **Config** → **Pin Assignment**
2. Configure CAN bus pins:
   - CS (Chip Select)
   - MOSI (Master Out Slave In)
   - MISO (Master In Slave Out)
   - SCK (Serial Clock)
3. Set CAN baud rate (typically 250 kbps for SBS)

### Battery Settings

1. Navigate to **Config** → **Battery Settings**
2. Set **Battery Provider** to "SBS (Smart Battery System)"
3. Configure SBS-specific parameters:
   - Battery address (if multiple batteries on bus)
   - Polling interval (recommended: 5-10 seconds)
   - Timeout values
4. Save the configuration

## SBS Data Parameters

The SBS interface provides standardized battery information:

### Core Battery Data
- **State of Charge (SoC):** Remaining capacity percentage
- **Voltage:** Battery terminal voltage
- **Current:** Charge/discharge current (signed value)
- **Temperature:** Battery temperature
- **Capacity:** Design capacity and remaining capacity

### Extended Information
- **Cycle Count:** Number of charge/discharge cycles
- **Design Voltage:** Nominal battery voltage
- **Manufacture Date:** Battery production date
- **Serial Number:** Unique battery identifier
- **Chemistry:** Battery chemistry type (Li-ion, LiFePO4, etc.)

### Status and Alarms
- **Battery Status:** Charging, discharging, fully charged, etc.
- **Alarm Conditions:** Over/undervoltage, over/under temperature
- **Error Codes:** Manufacturer-specific error information
- **Charging Info:** Charging voltage and current limits

### Safety Parameters
- **Over Voltage Protection:** Maximum safe voltage
- **Under Voltage Protection:** Minimum safe voltage  
- **Over Current Protection:** Maximum safe current
- **Temperature Limits:** Operating temperature range

## Advanced SBS Features

### Multi-Battery Support

SBS supports multiple batteries on the same CAN bus:
- Each battery has a unique address (0-15)
- Automatic discovery of available batteries
- Individual monitoring of each battery pack
- System-level aggregation of battery data

### Manufacturer Extensions

While SBS provides a standard baseline, manufacturers may extend the protocol:
- Additional proprietary parameters
- Enhanced diagnostic information
- Custom control commands
- Extended status reporting

## Integration with Dynamic Power Limiter

SBS battery data integrates comprehensively with the [Dynamic Power Limiter](dpl.md):

### Intelligent Charging
- **SoC-Based Limits:** Adjust charging based on precise SoC
- **Temperature Compensation:** Modify limits based on battery temperature
- **Cycle Optimization:** Optimize for battery longevity
- **Multi-Battery Balancing:** Balance charge across multiple packs

### Safety Integration
- **Alarm Response:** Immediate response to SBS alarm conditions
- **Limit Enforcement:** Respect SBS voltage and current limits
- **Temperature Protection:** Prevent operation outside safe temperature range
- **Emergency Shutdown:** Safe shutdown on critical alarms

## Troubleshooting

### Communication Issues

1. **No SBS Communication:**
   - Verify CAN bus wiring and termination
   - Check baud rate configuration (250k is SBS standard)
   - Ensure proper grounding between all devices
   - Verify CAN transceiver power supply

2. **Partial Data Reception:**
   - Check for CAN bus errors (error frames)
   - Monitor for message collisions
   - Verify battery address configuration
   - Check polling rate (don't poll too frequently)

3. **Data Inconsistencies:**
   - Verify SBS protocol version compatibility
   - Check for manufacturer-specific implementations
   - Monitor for data scaling factors
   - Validate checksum/CRC if implemented

### SBS-Specific Diagnostics

Use the web interface to monitor:
- SBS message reception rates
- CAN error counters
- Battery response times
- Protocol compliance issues

### Multi-Battery Issues

When using multiple batteries:
- Verify unique addresses for each battery
- Check for address conflicts
- Monitor individual vs system-wide data
- Ensure proper load balancing

## MQTT Integration

SBS data is published to MQTT with standardized topics:

```
<base_topic>/battery/sbs/
├── system/
│   ├── total_soc           # System-wide SoC (%)
│   ├── total_voltage       # System voltage (V)
│   ├── total_current       # System current (A)
│   └── battery_count       # Number of batteries detected
└── battery_<id>/
    ├── soc                 # Individual battery SoC (%)
    ├── voltage             # Battery voltage (V)
    ├── current             # Battery current (A)
    ├── temperature         # Battery temperature (°C)
    ├── cycle_count         # Charge cycles
    ├── status              # Battery status
    ├── alarms              # Active alarms
    └── serial_number       # Battery serial number
```

## Benefits of SBS Interface

### Standardization
- Vendor-neutral implementation
- Consistent data formats across manufacturers
- Simplified integration with multiple battery types
- Future-proof protocol support

### Reliability
- Robust CAN bus communication
- Built-in error detection and recovery
- Standardized alarm and status reporting
- Multi-battery redundancy support

### Flexibility
- Support for various battery chemistries
- Scalable to multiple battery installations
- Extensible for manufacturer-specific features
- Compatible with existing SBS ecosystem

## Limitations

- Requires SBS-compliant battery management system
- CAN bus implementation varies between manufacturers
- Some advanced features may be manufacturer-specific
- Protocol complexity may require more configuration than simpler interfaces
- Not all battery systems support SBS over CAN (many use SMBus only)
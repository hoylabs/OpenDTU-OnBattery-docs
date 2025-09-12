# Pytes Battery Interface

Pytes battery systems are supported through CAN bus communication. Pytes manufactures lithium iron phosphate (LiFePO4) batteries commonly used in residential energy storage systems.

## Overview

Pytes batteries communicate using standard CAN bus protocol. OpenDTU-OnBattery can monitor battery status, state of charge, and system health information from Pytes battery management systems.

## Supported Models

Pytes battery systems that support CAN communication include:

- **E-Box Series:**
  - E-Box-48100R
  - E-Box-48200R  
  - E-Box-4850P
  - E-Box-4860P

- **V5° Series:**
  - V5°-48200
  - V5°-48100
  - V5°-4874

- **HV Series:**
  - HV4850
  - HV48100
  - HV48200

!!! note "CAN Support"
    Not all Pytes models include CAN communication. Verify your specific model supports CAN bus before attempting to connect.

## Connection

### Hardware Requirements

- ESP32 with CAN transceiver (e.g., MCP2515 + TJA1050)
- CAN bus cable (twisted pair, typically CAT5/6 cable)
- CAN termination resistors (120Ω) if required

### CAN Bus Wiring

```
Pytes Battery    CAN Transceiver    ESP32
-------------    ---------------    -----
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

Pytes batteries typically use:
- **Baud Rate:** 250 kbps or 500 kbps
- **CAN ID Range:** 0x355-0x35F (consult your model documentation)
- **Frame Format:** Standard (11-bit identifier)

## Configuration

### Hardware Setup

1. Navigate to **Config** → **Device Profile** or **Config** → **Pin Assignment**
2. Configure CAN bus pins:
   - CS (Chip Select)
   - MOSI (Master Out Slave In)
   - MISO (Master In Slave Out)
   - SCK (Serial Clock)
3. Set CAN baud rate to match your Pytes battery (typically 250k or 500k)

### Battery Settings

1. Navigate to **Config** → **Battery Settings**
2. Set **Battery Provider** to "Pytes CAN"
3. Configure polling interval (recommended: 5-10 seconds)
4. Set appropriate timeout values
5. Save the configuration

## Monitored Parameters

The Pytes CAN interface provides comprehensive battery information:

### Basic Metrics
- State of Charge (SoC) percentage
- Total battery voltage
- Charge/discharge current
- Power consumption/generation
- Battery temperature

### System Status
- Battery operational state (charging/discharging/idle)
- Fault codes and alarms
- Protection status
- System warnings

### Advanced Data
- Individual module voltages (if available)
- Cell balancing status
- Charge/discharge cycles
- Battery capacity information
- Estimated time to empty/full

## Troubleshooting

### Connection Issues

1. **No CAN Communication:**
   - Verify CAN bus wiring (CAN_H and CAN_L)
   - Check baud rate configuration
   - Ensure proper grounding
   - Verify CAN transceiver power supply

2. **Intermittent Data:**
   - Check for loose connections
   - Verify CAN termination resistors
   - Monitor for electrical interference
   - Check cable quality and length

3. **Incorrect Data:**
   - Verify CAN ID configuration
   - Check battery model compatibility
   - Ensure firmware compatibility

### Diagnostic Tools

Use the web interface debug information to monitor:
- CAN message reception status
- Data parsing errors
- Communication timeouts
- Message frequency

## Integration with Dynamic Power Limiter

Pytes battery data integrates with the [Dynamic Power Limiter](dpl.md) to:

- **Charge Management:**
  - Prevent overcharging when SoC approaches 100%
  - Respect battery manufacturer charging profiles
  - Monitor charging current limits

- **Discharge Protection:**
  - Prevent deep discharge below safe SoC levels
  - Monitor discharge current limits
  - Respond to battery protection warnings

- **Safety Features:**
  - Monitor battery temperature
  - Respond to fault conditions
  - Implement emergency shutdown if needed

## MQTT Integration

Battery data is published to MQTT topics for external system integration. Common topics include:

```
<base_topic>/battery/
├── soc                 # State of charge (%)
├── voltage             # Battery voltage (V)
├── current             # Current (A, + = charging)
├── power               # Power (W, + = charging)
├── temperature         # Battery temperature (°C)
├── available           # Data availability
└── last_update         # Last successful update timestamp
```

See [MQTT Topics](mqtt_topics.md) for complete topic reference.

## Safety Considerations

!!! warning "Safety First"
    - Follow Pytes installation and safety guidelines
    - Ensure proper electrical isolation
    - Monitor system regularly for fault conditions
    - Implement appropriate circuit protection

!!! note "Battery Warranty"
    Verify that external monitoring doesn't void your Pytes battery warranty. Consult Pytes documentation and support.

## Limitations

- CAN protocol specifics may vary between Pytes models
- Some advanced features may require specific firmware versions
- Real-time control capabilities are limited to monitoring
- Battery settings and calibration must be done through Pytes interface
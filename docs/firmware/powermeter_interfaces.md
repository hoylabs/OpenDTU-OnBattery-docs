# Power Meter Interfaces

OpenDTU-OnBattery supports multiple power meter interfaces for monitoring household power consumption. This data is essential for the [Dynamic Power Limiter](dpl.md) to implement zero-export or load-following functionality.

## Overview

Power meters provide real-time measurements of:
- Active power consumption/production (Watts)
- Voltage and current measurements
- Power factor and frequency
- Energy consumption (kWh) - some interfaces
- Import/export direction indication

## Supported Power Meter Types

OpenDTU-OnBattery supports several power meter interface types:

### 1. SML (Smart Message Language)
SML is a German standard for smart meter communication, commonly used by many European utility meters.

**Features:**
- Real-time power readings
- Energy consumption data
- Standardized protocol
- High accuracy measurements

**Common Applications:**
- German smart meters (EDL21, EDL40)
- Austrian smart meters
- Various European utility installations

### 2. SDM Series (Eastron)
Eastron SDM series are popular DIN-rail mounted energy meters with Modbus communication.

**Supported Models:**
- SDM72D, SDM72DM
- SDM120, SDM230, SDM630
- Other SDM series with Modbus RTU

**Features:**
- High accuracy (Class 1)
- Comprehensive electrical measurements
- Modbus RTU over RS485
- DIN-rail mounting

### 3. JSON over HTTP
Generic HTTP-based interface for power meters that provide data via JSON API.

**Features:**
- Universal compatibility
- Custom data mapping
- Support for various smart meters
- Network-based communication

**Compatible Systems:**
- Tasmota-based smart meters
- Custom ESP-based meters
- Home automation systems with power data
- Commercial meters with HTTP API

### 4. UDP Network Interface
Simple UDP-based protocol for real-time power data transmission.

**Features:**
- Low latency communication
- Simple protocol implementation
- Network-based operation
- Custom format support

## Connection Methods

### SML Interface Connection

SML meters typically use optical or serial communication:

```
Smart Meter     Interface      ESP32
-----------     ---------      -----
Optical Port -> IR Reader  -> GPIO (RX)
                Ground     -> GND
```

**Hardware Requirements:**
- IR optical reader (for meters with optical port)
- UART-to-optical converter
- Appropriate mounting hardware

### SDM Modbus Connection

SDM meters use RS485 Modbus communication:

```
SDM Meter    RS485 Module    ESP32
---------    ------------    -----
A+        -> A+           
B-        -> B-           
GND       -> GND        -> GND
          -> VCC        -> 3.3V/5V
          -> DI         -> GPIO (TX)
          -> RO         -> GPIO (RX)
          -> DE/RE      -> GPIO (Direction)
```

**Hardware Requirements:**
- RS485 transceiver module (e.g., MAX485, SP485)
- Twisted pair cable for RS485 bus
- Termination resistors (120Ω) if required

### JSON HTTP Interface

For HTTP-based meters, only network connectivity is required:

```
Power Meter -> Network (WiFi/Ethernet) -> ESP32
```

**Requirements:**
- Network connectivity for both devices
- HTTP server capability on power meter
- JSON format data output

### UDP Interface

UDP interface requires network setup:

```
Power Meter -> Network (UDP) -> ESP32
```

**Requirements:**
- UDP broadcast or unicast capability
- Defined data packet format
- Network connectivity

## Configuration

### General Power Meter Settings

1. Navigate to **Config** → **Power Meter Settings**
2. Select appropriate **Power Meter Type**
3. Configure interface-specific parameters
4. Set polling interval (typically 1-5 seconds)
5. Configure data validation parameters

### SML Configuration

**SML-Specific Settings:**
- UART pin assignment for optical reader
- Baud rate (typically 9600)
- SML message parsing options
- Meter identification (if multiple meters)

### SDM Configuration

**Modbus Settings:**
- RS485 pin assignment (TX, RX, Direction)
- Baud rate (typically 9600, 19200, or 38400)
- Modbus device address (1-247)
- Register mapping for specific SDM model
- Timeout and retry parameters

### JSON HTTP Configuration

**HTTP Settings:**
- Power meter IP address
- HTTP port (typically 80 or 443)
- URL path for JSON data
- Update interval
- JSON field mapping for power values
- Authentication (if required)

### UDP Configuration

**Network Settings:**
- UDP port number
- Source IP filtering (optional)
- Data packet format
- Value extraction parameters

## Monitored Parameters

Depending on the power meter type and capabilities:

### Basic Measurements
- **Active Power:** Current power consumption/production (W)
- **Voltage:** Line voltage (V) - single or three-phase
- **Current:** Line current (A) - single or three-phase
- **Frequency:** Grid frequency (Hz)

### Advanced Measurements
- **Power Factor:** Efficiency indicator
- **Reactive Power:** Non-productive power (VAR)
- **Apparent Power:** Total power (VA)
- **Energy Consumption:** Cumulative consumption (kWh)
- **Energy Production:** Cumulative production (kWh)

### Multi-Phase Support
For three-phase installations:
- Individual phase measurements
- Total system power
- Phase balance information
- Neutral current (if available)

## Integration with Dynamic Power Limiter

Power meter data is essential for DPL operation:

### Zero Export Control
- Monitor grid export/import in real-time
- Adjust inverter output to prevent export
- Account for measurement delays and response times

### Load Following
- Match solar production to household consumption
- Smooth power transitions
- Optimize energy utilization

### Safety Functions
- Detect grid disconnection
- Monitor power quality parameters
- Implement emergency shutdowns if needed

## Troubleshooting

### SML Interface Issues

1. **No SML Data:**
   - Check optical reader alignment
   - Verify meter supports SML output
   - Check baud rate and UART configuration
   - Ensure proper power supply to optical reader

2. **Intermittent Data:**
   - Check optical coupling quality
   - Monitor for ambient light interference
   - Verify SML message timing

### SDM Modbus Issues

1. **No Modbus Communication:**
   - Check RS485 wiring (A+/B- polarity)
   - Verify baud rate and device address
   - Check termination resistors
   - Monitor for bus conflicts

2. **Incorrect Values:**
   - Verify register mapping for specific SDM model
   - Check data scaling factors
   - Validate Modbus frame format

### Network Interface Issues

1. **HTTP/UDP Connection Problems:**
   - Verify network connectivity
   - Check IP addresses and ports
   - Monitor for firewall blocking
   - Validate data format and parsing

2. **Data Quality Issues:**
   - Check update frequency compatibility
   - Monitor for network latency
   - Validate data format consistency

## MQTT Integration

Power meter data is published to MQTT:

```
<base_topic>/powermeter/
├── power               # Current power (W, + = import, - = export)
├── voltage_l1          # Phase 1 voltage (V)
├── voltage_l2          # Phase 2 voltage (V, if available)
├── voltage_l3          # Phase 3 voltage (V, if available)
├── current_l1          # Phase 1 current (A)
├── current_l2          # Phase 2 current (A, if available)
├── current_l3          # Phase 3 current (A, if available)
├── frequency           # Grid frequency (Hz)
├── power_factor        # Power factor
├── energy_import       # Total imported energy (kWh)
├── energy_export       # Total exported energy (kWh)
└── last_update         # Last successful update timestamp
```

## Best Practices

### Meter Selection
- Choose meters with appropriate accuracy class (Class 1 or better)
- Ensure measurement range covers expected power levels
- Consider three-phase capability for balanced loads
- Verify communication protocol compatibility

### Installation
- Install meters at appropriate measurement points
- Ensure proper grounding and isolation
- Use appropriate cable types for communication
- Implement proper surge protection

### Configuration
- Set appropriate polling intervals (balance responsiveness vs system load)
- Configure reasonable timeout values
- Implement data validation and filtering
- Monitor communication quality regularly

## Limitations

- Communication reliability depends on interface type
- Some meters may have limited update rates
- Network-based interfaces may have latency
- Protocol specifics vary between manufacturers
- Accuracy depends on meter quality and installation
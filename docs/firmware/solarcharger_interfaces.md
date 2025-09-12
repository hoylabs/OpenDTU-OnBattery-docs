# Solar Charger Interfaces

OpenDTU-OnBattery supports monitoring of solar charge controllers to track solar energy production and battery charging from solar panels. This information helps optimize the overall energy system performance.

## Overview

Solar charger monitoring provides:
- Solar panel voltage and current
- Battery charging current and voltage
- Charge controller status and operating mode
- Daily energy production statistics
- Charge controller temperature and efficiency
- MPPT (Maximum Power Point Tracking) information

## Supported Solar Charger Types

### 1. Victron VE.Direct Protocol

Victron Energy solar charge controllers with VE.Direct communication are fully supported.

**Supported Models:**
- **SmartSolar MPPT Series:**
  - 75/10, 75/15
  - 100/15, 100/20, 100/30, 100/50
  - 150/35, 150/45, 150/60, 150/70, 150/85, 150/100

- **BlueSolar MPPT Series:**
  - 75/10, 75/15
  - 100/15, 100/20, 100/30, 100/50
  - 150/35, 150/45, 150/60, 150/70, 150/85, 150/100

- **Phoenix Smart IP43 Chargers** (when used with solar input)

**Features:**
- Real-time solar production monitoring
- Battery charging status
- Historical energy data
- Charge controller diagnostics
- Load output control (on supported models)

### 2. MQTT Interface

Generic MQTT interface for solar charge controllers that publish data to MQTT broker.

**Features:**
- Universal compatibility with MQTT-enabled devices
- Custom topic mapping
- Flexible data format support
- Network-based communication

**Compatible Systems:**
- DIY charge controllers with ESP32/Arduino
- Tasmota-based solar monitoring
- Commercial charge controllers with MQTT capability
- Home automation systems with solar data

## Connection Methods

### Victron VE.Direct Connection

VE.Direct uses a simple 4-wire serial connection:

```
Victron MPPT     ESP32
------------     -----
VE.Direct Port:
Pin 1 (GND)   -> GND
Pin 2 (RX)    -> GPIO (TX)
Pin 3 (TX)    -> GPIO (RX)
Pin 4 (+5V)   -> Optional 5V supply
```

**Hardware Requirements:**
- VE.Direct cable or custom cable with JST-PH 4-pin connector
- Level shifter may be needed (VE.Direct is 3.3V, same as ESP32)
- Optional: VE.Direct to USB cable for initial testing

**Cable Specifications:**
- **Connector:** JST-PH 4-pin (2.0mm pitch)
- **Pinout:** Standard VE.Direct pinout
- **Cable Length:** Up to 10 meters with appropriate gauge wire

### MQTT Connection

MQTT interface requires only network connectivity:

```
Solar Charger -> Network (WiFi/Ethernet) -> MQTT Broker -> ESP32
```

**Requirements:**
- Network connectivity for solar charger
- MQTT broker (can be same as OpenDTU-OnBattery broker)
- Configurable MQTT topics

## Configuration

### Victron VE.Direct Setup

1. **Physical Connection:**
   - Connect VE.Direct cable between charge controller and ESP32
   - Ensure proper pin assignment (TX/RX, GND)
   - Verify 3.3V compatibility (no level shifter needed for most ESP32)

2. **Software Configuration:**
   - Navigate to **Config** → **Solar Charger Settings**
   - Set **Provider** to "Victron VE.Direct"
   - Configure UART pins (RX, TX)
   - Set polling interval (recommended: 10-30 seconds)

3. **Charge Controller Settings:**
   - Enable VE.Direct text output (usually enabled by default)
   - Verify communication settings on charge controller
   - Check for any protocol version requirements

### MQTT Interface Setup

1. **MQTT Configuration:**
   - Navigate to **Config** → **Solar Charger Settings**
   - Set **Provider** to "MQTT"
   - Configure MQTT broker connection details
   - Map MQTT topics to solar charger data fields

2. **Topic Mapping:**
   - Configure topics for each monitored parameter
   - Set appropriate data types and units
   - Configure update intervals and timeout values

## Monitored Parameters

### Victron VE.Direct Data

Comprehensive data available from Victron charge controllers:

**Solar Panel Information:**
- Solar panel voltage (V)
- Solar panel current (A)
- Solar panel power (W)
- Maximum power point voltage (V)

**Battery Information:**
- Battery voltage (V)
- Battery charging current (A)
- Battery charging power (W)
- Battery charging state (Bulk, Absorption, Float, etc.)

**Energy Statistics:**
- Daily energy yield (kWh)
- Yesterday's energy yield (kWh)
- Maximum power today (W)
- Cumulative energy yield (kWh)

**System Status:**
- Charge controller state (Off, Low Power, Fault, Bulk, Absorption, Float)
- Error codes and fault information
- Charge controller temperature (°C)
- Load output status (if available)
- Load current (A) (if load output used)

**Advanced Parameters:**
- MPPT efficiency
- Charge controller firmware version
- Serial number and model information
- Network information (for SmartSolar models)

### MQTT Interface Data

Flexible data mapping allows monitoring of various parameters:

**Configurable Parameters:**
- Solar production power (W)
- Battery charging current (A)
- Daily energy production (kWh)
- Charge controller status
- Temperature measurements
- Error states and alarms

## Integration with Dynamic Power Limiter

Solar charger data enhances DPL functionality:

### Enhanced Solar Forecasting
- **Real-time Production:** Actual solar output vs inverter production
- **Weather Compensation:** Adjust predictions based on current conditions
- **Efficiency Monitoring:** Track system performance over time

### Battery Charging Optimization
- **Charge Coordination:** Coordinate AC charging with solar charging
- **Battery State Awareness:** Optimize based on battery charge from solar
- **Load Balancing:** Balance solar charging vs house loads

### System Diagnostics
- **Performance Monitoring:** Track solar system efficiency
- **Fault Detection:** Early warning for solar system issues
- **Maintenance Scheduling:** Optimize maintenance based on performance data

## Advanced Features

### Victron-Specific Features

**Load Output Control:**
- Control load output relay on supported Victron models
- Implement custom load control logic
- Battery voltage-based load disconnection

**Network Integration:**
- Integration with Victron VRM portal data
- Coordination with other Victron devices
- Advanced battery management integration

**Firmware Updates:**
- Monitor for available Victron firmware updates
- Track firmware version for compatibility

### Historical Data Tracking

**Energy Production Records:**
- Daily, weekly, monthly production totals
- Peak production tracking
- Seasonal performance analysis
- System degradation monitoring

## Troubleshooting

### VE.Direct Communication Issues

1. **No Data Received:**
   - Check cable connections and pinout
   - Verify UART pin configuration
   - Ensure charge controller VE.Direct is enabled
   - Test cable with VictronConnect app

2. **Intermittent Data:**
   - Check for loose connections
   - Verify cable integrity
   - Monitor for electrical interference
   - Check polling interval settings

3. **Incorrect Values:**
   - Verify VE.Direct protocol version
   - Check for firmware compatibility
   - Validate data parsing and units
   - Compare with VictronConnect readings

### MQTT Interface Issues

1. **Connection Problems:**
   - Verify MQTT broker connectivity
   - Check topic names and permissions
   - Monitor MQTT message format
   - Validate data parsing configuration

2. **Data Quality Issues:**
   - Check update frequency consistency
   - Monitor for missing data points
   - Validate data range and units
   - Implement data validation filters

### General Diagnostic Steps

1. **Monitor Debug Output:**
   - Check OpenDTU-OnBattery log messages
   - Monitor communication status
   - Verify data parsing success
   - Check for timeout errors

2. **Compare with Reference:**
   - Cross-check with charge controller display
   - Compare with manufacturer app readings
   - Validate against multimeter measurements
   - Check historical data consistency

## MQTT Publication

Solar charger data is published to MQTT:

```
<base_topic>/solar/
├── power_solar         # Solar panel power (W)
├── voltage_solar       # Solar panel voltage (V)
├── current_solar       # Solar panel current (A)
├── power_battery       # Battery charging power (W)
├── voltage_battery     # Battery voltage (V)
├── current_battery     # Battery charging current (A)
├── yield_daily         # Daily energy yield (kWh)
├── yield_yesterday     # Yesterday's yield (kWh)
├── yield_total         # Total cumulative yield (kWh)
├── max_power_today     # Maximum power today (W)
├── state               # Charge controller state
├── error_code          # Error code (0 = no error)
├── temperature         # Controller temperature (°C)
├── mppt_efficiency     # MPPT efficiency (%)
└── last_update         # Last successful update
```

## Use Cases

### Residential Solar Systems
- Monitor rooftop solar production
- Track battery charging from solar
- Optimize energy consumption patterns
- Detect system performance issues

### Off-Grid Systems
- Critical monitoring for energy independence
- Battery state management
- Load prioritization based on solar availability
- System maintenance scheduling

### RV and Mobile Applications
- Portable solar system monitoring
- Battery management for mobile power
- Travel route optimization based on solar conditions
- Remote system diagnostics

### Commercial Installations
- Multi-controller monitoring
- Performance benchmarking
- Maintenance optimization
- Energy production reporting

## Best Practices

### Installation
- Use appropriate cable types for VE.Direct connections
- Ensure proper grounding and electromagnetic compatibility
- Implement surge protection for outdoor installations
- Document all connections and configurations

### Configuration
- Set reasonable polling intervals (avoid overwhelming the charge controller)
- Implement data validation and filtering
- Configure appropriate timeout values
- Regular monitoring of communication quality

### Maintenance
- Regularly verify data accuracy against reference measurements
- Monitor for firmware updates on charge controllers
- Check cable connections periodically
- Maintain historical data for trend analysis

## Limitations

- VE.Direct protocol specifics may vary between Victron models
- Some advanced features require specific firmware versions
- Real-time control capabilities are typically limited
- Network-based interfaces may have communication delays
- Protocol documentation may be limited for some third-party devices
# MQTT Topics

The base topic, as configured in the web GUI is prepended to all following topics.

## General topics

| Topic                                     | R / W | Description                                          | Value / Unit               |
| ----------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `dtu/ip`                                  | R     | IP address of OpenDTU-OnBattery                      | IP address                 |
| `dtu/hostname`                            | R     | Current hostname of the dtu (as set in web GUI)      |                            |
| `dtu/rssi`                                | R     | Wi-Fi network quality                                | db value                   |
| `dtu/status`                              | R     | Indicates whether OpenDTU-OnBattery network is reachable | online /  offline      |
| `dtu/temperature`                         | R     | Temperature of the ESP32                             | °C                         |
| `dtu/uptime`                              | R     | Time in seconds since startup                        | seconds                    |
| `dtu/heap/free`                           | R     | Available heap                                       | Bytes                      |
| `dtu/heap/size`                           | R     | Total heap size                                      | Bytes                      |
| `dtu/heap/minfree`                        | R     | Lowest level of free heap since boot                 | Bytes                      |
| `dtu/heap/maxalloc`                       | R     | Largest block of heap that can be allocated at once  | Bytes                      |

## Inverter total topics

Enabled inverter means, that only inverters with "Poll inverter data" enabled are considered.

| Topic                                     | R / W | Description                                          | Value / Unit               |
| ----------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `ac/power`                                | R     | Sum of AC active power of all enabled inverters      | W                          |
| `ac/yieldtotal`                           | R     | Sum of energy converted to AC since reset watt hours of all enabled inverters | Kilo watt hours (kWh) |
| `ac/yieldday`                             | R     | Sum of energy converted to AC per day in watt hours of all enabled inverters | Watt hours (Wh)
| `ac/is_valid`                             | R     | Indicator whether all enabled inverters were reachable | 0 or 1                   |
| `dc/power`                                | R     | Sum of DC power of all enabled inverters             | Watt (W)                   |
| `dc/irradiation`                          | R     | Produced power of all enabled inverter stripes with defined irradiation settings divided by sum of all enabled inverters irradiation | % |
| `dc/is_valid`                             | R     | Indicator whether all enabled inverters were reachable | 0 or 1                   |

## Inverter specific topics

serial will be replaced with the serial number of the inverter.

| Topic                                     | R / W | Description                                          | Value / Unit               |
| ----------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `[serial]/name`                           | R     | Name of the inverter as configured in web GUI        |                            |
| `[serial]/device/bootloaderversion`       | R     | Bootloader version of the inverter                   |                            |
| `[serial]/device/fwbuildversion`          | R     | Firmware version of the inverter                     |                            |
| `[serial]/device/fwbuilddatetime`         | R     | Build date / time of inverter firmware               |                            |
| `[serial]/device/hwpartnumber`            | R     | Hardware part number of the inverter                 |                            |
| `[serial]/device/hwversion`               | R     | Hardware version of the inverter                     |                            |
| `[serial]/radio/tx_request`               | R     | Amount of sent packet requests                       |                            |
| `[serial]/radio/tx_re_request`            | R     | Amount of sent fragment resend requests              |                            |
| `[serial]/radio/rx_success`               | R     | Amount of successfully received packets              |                            |
| `[serial]/radio/rx_fail_nothing`          | R     | Amount of failed packets, nothing was received       |                            |
| `[serial]/radio/rx_fail_partial`          | R     | Amount of failed packets, some fragments were missing |                            |
| `[serial]/radio/rx_fail_corrupt`          | R     | Amount of failed packets, payload corrupt            |                            |
| `[serial]/radio/rssi`                     | R     | RSSI of last received packet (Inverters with NRF24 module only support 2 states. > -64dBm and < -64dBm. In the first case -30 dBm is shown, in the second one -80 dBm) | dBm                        |
| `[serial]/status/reachable`               | R     | Indicates whether the inverter is reachable          | 0 or 1                     |
| `[serial]/status/producing`               | R     | Indicates whether the inverter is producing AC power | 0 or 1                     |
| `[serial]/status/last_update`             | R     | Unix timestamp of last inverter statistics update    | seconds since JAN 01 1970 (UTC) |

### AC channel / global specific topics

| Topic                                     | R / W | Description                                          | Value / Unit               |
| ----------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `[serial]/0/current`                      | R     | AC current in ampere                                 | Ampere (A)                 |
| `[serial]/0/efficiency`                   | R     | Ratio AC Power over DC Power in percent              | %                          |
| `[serial]/0/frequency`                    | R     | AC frequency in hertz                                | Hertz (Hz)                 |
| `[serial]/0/power`                        | R     | AC active power in watts                             | Watt (W)                   |
| `[serial]/0/powerdc`                      | R     | DC power in watts                                    | Watt (W)                   |
| `[serial]/0/powerfactor`                  | R     | Power factor in percent                              | %                          |
| `[serial]/0/reactivepower`                | R     | AC reactive power in VAr                             | VAr                        |
| `[serial]/0/temperature`                  | R     | Temperature of inverter in degree celsius            | Degree Celsius (°C)        |
| `[serial]/0/voltage`                      | R     | AC voltage in volt                                   | Volt (V)                   |
| `[serial]/0/yieldday`                     | R     | Energy converted to AC per day in watt hours         | Watt hours (Wh)            |
| `[serial]/0/yieldtotal`                   | R     | Energy converted to AC since reset watt hours        | Kilo watt hours (kWh)      |

### DC input channel topics

[1-4] represents the different inputs. The amount depends on the inverter model.

| Topic                                     | R / W | Description                                          | Value / Unit               |
| ----------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `[serial]/[1-4]/current`                  | R     | DC current of specific input in ampere               | Ampere (A)                 |
| `[serial]/[1-4]/name`                     | R     | Name of the DC input channel as configured in web GUI|                            |
| `[serial]/[1-4]/irradiation`              | R     | Ratio DC Power over set maximum power (in web GUI)   | %                          |
| `[serial]/[1-4]/power`                    | R     | DC power of specific input in watt                   | Watt (W)                   |
| `[serial]/[1-4]/voltage`                  | R     | DC voltage of specific input in volt                 | Volt (V)                   |
| `[serial]/[1-4]/yieldday`                 | R     | Energy converted to AC per day on specific input     | Watt hours (Wh)            |
| `[serial]/[1-4]/yieldtotal`               | R     | Energy converted to AC since reset on specific input | Kilo watt hours (kWh)      |

### Inverter limit specific topics

cmd topics are used to set values. Status topics are updated from values set in the inverter.

| Topic                                       | R / W | Description                                          | Value / Unit               |
| ------------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `[serial]/status/limit_relative`            | R     | Current applied production limit of the inverter     | % of total possible output |
| `[serial]/status/limit_absolute`            | R     | Current applied production limit of the inverter     | Watt (W)                   |
| `[serial]/cmd/limit_persistent_relative`    | W     | Set the inverter limit as a percentage of total production capability. The  value will survive the night without power. The updated value will show up in the web GUI and limit_relative topic immediately. | %                          |
| `[serial]/cmd/limit_persistent_absolute`    | W     | Set the inverter limit as an absolute value. The value will survive the night without power. The updated value will set immediately within the inverter but show up in the web GUI and limit_absolute topic after around 4 minutes. If you are using an already known inverter (known Hardware ID), the updated value will show up within a few seconds. | Watt (W)                   |
| `[serial]/cmd/limit_nonpersistent_relative` | W     | Set the inverter limit as a percentage of total production capability. The  value will reset to the last persistent value at night without power. The updated value will show up in the web GUI and limit_relative topic immediately. The value must be published non-retained, otherwise it will be ignored! | %                          |
| `[serial]/cmd/limit_nonpersistent_absolute` | W     | Set the inverter limit as an absolute value. The value will reset to the last persistent value at night without power. The updated value will set immediately within the inverter but show up in the web GUI and limit_absolute topic after around 4 minutes. If you are using a already known inverter (known Hardware ID), the updated value will show up within a few seconds. The value must be published non-retained, otherwise it will be ignored! | Watt (W)                   |
| `[serial]/cmd/power`                        | W      | Turn the inverter on (1) or off (0)                 | 0 or 1                     |
| `[serial]/cmd/reset_rf_stats`               | W      | Reset the radio statistics                          | 1                          |
| `[serial]/cmd/restart`                      | W      | Restarts the inverters (also resets YieldDay)       | 1                          |

## Victron MPPT topics

### General

| Topic                                   | R / W | Description                                          | Value / Unit               |
| --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `victron/[serial]/PID`                  | R     | Product description                                  | text                       |
| `victron/[serial]/SER`                  | R     | Serial number                                        | text                       |
| `victron/[serial]/FW`                   | R     | Firmware number                                      | int                        |
| `victron/[serial]/LOAD`                 | R     | Load output state                                    | ON /  OFF                  |
| `victron/[serial]/CS`                   | R     | State of operation                                   | text, e.g., "Bulk"         |
| `victron/[serial]/ERR`                  | R     | Error code                                           | text, e.g., "No error"     |
| `victron/[serial]/OR`                   | R     | Off reason                                           | text, e.g., "Not off"      |
| `victron/[serial]/MPPT`                 | R     | Tracker operation mode                               | text, e.g., "MPP Tracker active" |
| `victron/[serial]/HSDS`                 | R     | Day sequence number (0...364)                        | int in days                |

### Battery output

| Topic                                   | R / W | Description                                          | Value / Unit               |
| --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `victron/[serial]/V`                    | R     | Voltage                                              | Volt (V)                   |
| `victron/[serial]/I`                    | R     | Current                                              | Ampere (A)                 |

### Solar input

| Topic                                   | R / W | Description                                          | Value / Unit               |
| --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `victron/[serial]/VPV`                  | R     | Voltage                                              | Volt (V)                   |
| `victron/[serial]/PPV`                  | R     | Power                                                | Watt (W)                   |
| `victron/[serial]/H19`                  | R     | Yield total (user resettable counter)                | Kilo watt hours (kWh)      |
| `victron/[serial]/H20`                  | R     | Yield today                                          | Kilo watt hours (kWh)      |
| `victron/[serial]/H21`                  | R     | Maximum power today                                  | Watt (W)                   |
| `victron/[serial]/H22`                  | R     | Yield yesterday                                      | Kilo watt hours (kWh)      |
| `victron/[serial]/H23`                  | R     | Maximum power yesterday                              | Watt (W)                   |

## (Pylontech) Battery topics

!!!warning "Incomplete"
    In particular, topics specific to the JK BMS and Victron SmartShunt are not
    yet documented.

| Topic                                         | R / W | Description                                          | Value / Unit               |
| --------------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `battery/settings/chargeVoltage`              | R     | Voltage                                              | Volt (V)                   |
| `battery/settings/chargeCurrentLimitation`    | R     | BMS requested max. charge current                    | Ampere (A)                 |
| `battery/settings/dischargeCurrentLimitation` | R     | BMS requested max. discharge current                 | Ampere (A)                 |
| `battery/stateOfCharge`                       | R     | State of Health                                      | %                          |
| `battery/stateOfHealth`                       | R     | State of Charge                                      | %                          |
| `battery/dataAge`                             | R     | How old the data is                                  | Seconds                    |
| `battery/voltage`                             | R     | Actual voltage                                       | Volt (V)                   |
| `battery/current`                             | R     | Actual current                                       | Ampere (A)                 |
| `battery/temperature`                         | R     | Actual temperature                                   | °C                         |
| `battery/alarm/overCurrentDischarge`          | R     | Alarm: High discharge current                        | 0 / 1                      |
| `battery/alarm/underTemperature`              | R     | Alarm: Low temperature                               | 0 / 1                      |
| `battery/alarm/overTemperature`               | R     | Alarm: High temperature                              | 0 / 1                      |
| `battery/alarm/underVoltage`                  | R     | Alarm: Low voltage                                   | 0 / 1                      |
| `battery/alarm/overVoltage`                   | R     | Alarm: High voltage                                  | 0 / 1                      |
| `battery/alarm/bmsInternal`                   | R     | Alarm: BMS internal                                  | 0 / 1                      |
| `battery/alarm/overCurrentCharge`             | R     |                                                      |                            |
| `battery/warning/highCurrentDischarge`        | R     | Warning: High discharge current                      | 0 / 1                      |
| `battery/warning/lowTemperature`              | R     | Warning: Low temperature                             | 0 / 1                      |
| `battery/warning/highTemperature`             | R     | Warning: High temperature                            | 0 / 1                      |
| `battery/warning/lowVoltage`                  | R     | Warning: Low voltage                                 | 0 / 1                      |
| `battery/warning/highVoltage`                 | R     | Warning: High voltage                                | 0 / 1                      |
| `battery/warning/bmsInternal`                 | R     | Warning: BMS internal                                | 0 / 1                      |
| `battery/manufacturer`                        | R     | Manufacturer                                         | String                     |
| `battery/charging/chargeEnabled`              | R     | Charge enabled flag                                  | 0 / 1                      |
| `battery/charging/dischargeEnabled`           | R     | Discharge enabled flag                               | 0 / 1                      |
| `battery/charging/chargeImmediately`          | R     | Charge immediately flag                              | 0 / 1                      |

## Huawei AC charger topics

| Topic                                   | R / W | Description                                          | Value / Unit               |
| --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `huawei/cmd/limit_online_voltage`       | W     | Online voltage (i.e. CAN bus connected)              | Volt (V)                   |
| `huawei/cmd/limit_online_current`       | W     | Online current (i.e. CAN bus connected)              | Ampere (A)                 |
| `huawei/cmd/limit_offline_voltage`      | W     | Offline voltage (i.e. CAN bus not connected)         | Volt (V)                   |
| `huawei/cmd/limit_offline_current`      | W     | Offline current (i.e. CAN bus not connected)         | Ampere (A)                 |
| `huawei/cmd/mode`                       | W     | Controls GPIO output pin to switch slot detect       | 0 (off) / 1 (on) / 2 (set automatically depending on online_current value) / 3 (set automatically based on Power Meter reading ) |
| `huawei/mode`                           | R     | Currently set charging mode                          | see above                  |
| `huawei/data_age`                       | R     | How old the data is                                  | Seconds                    |
| `huawei/input_voltage`                  | R     | Input voltage                                        | Volt (V)                   |
| `huawei/input_current`                  | R     | Input current                                        | Ampere (A)                 |
| `huawei/input_power`                    | R     | Input power                                          | Watt (W)                   |
| `huawei/output_voltage`                 | R     | Output voltage                                       | Volt (V)                   |
| `huawei/output_current`                 | R     | Output current                                       | Ampere (A)                 |
| `huawei/max_output_current`             | R     | Maximum output current (set using the online limit)  | Ampere (A)                 |
| `huawei/output_power`                   | R     | Output power                                         | Watt (W)                   |
| `huawei/input_temp`                     | R     | Input air temperature                                | °C                         |
| `huawei/output_temp`                    | R     | Output air temperature                               | °C                         |
| `huawei/efficiency`                     | R     | Efficiency                                           | Percentage                 |

## Power Limiter topics

### General

| Topic                                         | R / W | Description                                       | Value     |
| --------------------------------------------- | ----- | ------------------------------------------------- |---------- |
| `powerlimiter/status/upper_power_limit`       | R     | get currently set maximum power limit of inverter | Power [W] |
| `powerlimiter/cmd/upper_power_limit`          | W     | set maximum power limit of inverter               | Power [W] |
| `powerlimiter/status/target_power_consumption`| R     | get currently set target grid consumption         | Power [W] |
| `powerlimiter/cmd/target_power_consumption`   | W     | set target grid consumption                       | Power [W] |

### Battery Thresholds

If the inverter is solar-powered, none of the thresholds are published and
publishing to the respective `cmd` topic has no effect.

Note that, depending on your settings, some of the thresholds might have no
effect. Refer to the [DPL
documentation](https://github.com/hoylabs/OpenDTU-OnBattery/wiki/Dynamic-Power-Limiter)
to understand the thresholds.

| Topic                                                                | Limitation |
| -------------------------------------------------------------------- | ---------- |
| `powerlimiter/status/threshold/voltage/start`                        | |
| `powerlimiter/status/threshold/voltage/stop`                         | |
| `powerlimiter/status/threshold/voltage/full_solar_passthrough_start` | Not published if VE.Direct disabled |
| `powerlimiter/status/threshold/voltage/full_solar_passthrough_stop`  | Not published if VE.Direct disabled |
| `powerlimiter/status/threshold/soc/start`                            | Not published if no battery interface configured or SoC is set to be ignored |
| `powerlimiter/status/threshold/soc/stop`                             | Not published if no battery interface configured or SoC is set to be ignored |
| `powerlimiter/status/threshold/soc/full_solar_passthrough`           | Not published if no battery interface configured or SoC is set to be ignored or VE.Direct disabled |

All thresholds have respective `cmd` topics (replace `status` with `cmd`),
which allow to override the threshold. The overrides are persistent, i.e., new
values are written to the persistent configuration file.

Example: Use topic `powerlimiter/cmd/threshold/voltage/start` to override the
battery discharge cycle start voltage threshold.

### Mode
| Topic                                   | R / W | Description                                          | Value                      |
| --------------------------------------- | ----- | ---------------------------------------------------- | -------------------------- |
| `powerlimiter/cmd/mode`                 | W     | Power Limiter operation mode                         | see below                  |
| `powerlimiter/status/mode`              | R     | Get Power Limiter operation mode                     | see below                  |

Setting any a mode through MQTT has *no* effect if the Power Limiter is
disabled by configuration in the web application. The Power Limiter will stay
off in that case.

When using the web application to change DPL settings, the DPL mode will be
reset to *normal operation*.

Three modes are implemented:

* **0** - Normal operation: The Power Limiter works as configured through the
  web application.
* **1** - Fully disable with inverter shut down: The inverter is shut down and
  afterwards the Power Limiter stops operating, as if it was disabled in the
  web application. Note that this means that the inverter can start producing
  power if the web application or an MQTT topic is used to control it.
* **2** - Unconditional Full Solar-Passthrough: The power limits are set such
  that all available solar power is fed into the home, irrespective of grid
  consumption. Essentially, the inverters mimic the behavior of traditional,
  non-smart inverters. Depending on your configuration, the inverters' limits
  are set to the following:
    1. **Battery-powered inverters**: The inverters' limits are set to match
       the solar power output (VE.Direct interface), adjusted for efficiency,
       such that no energy from the battery is consumed. Note that if VE.Direct
       is disabled or the data is outdated, the inverters are shut down
       instead. This mode can be particularly useful in scenarios where solar
       power is better stored elsewhere, such as in an electric car.
    2. **Solar-powered inverters**: The inverters' limits are set to the
       respective upper limit configured in the DPL settings (starting from
       release 2024.05.03).

### Timeout Counter

!!!note "Availability"
    Starting from release 2024.05.03.

The DPL counts the amount of times an attempt to configure an inverter to a
particular state times out. This counter is used to decide to sent an inverter
restart command, hoping to "revive" the inverter. If the counter keeps
increasing even after multiple restart commands have been issued, the ESP
restarts as a last resort. The thresholds to perform these actions are hard
coded to 10 and 20 timeouts, respectively.

Topic `powerlimiter/status/inverter_update_timeouts` can be monitored to be
alerted by these timeouts.

# Introduction

The dynamic power limiter (DPL) is a software feature allowing automatic
inverter power limit adjustments. Depending on the setup, the DPL will take the
power meter (i.e. currently consumed power), the available solar power, and the
battery charge state into account. Selected inverters are governed such that
the currently consumed power (as provided by the power meter) reaches a
user-defined value (usually zero). In particular, this allows to implement a
zero-export setup.

!!!note "Systems without battery"
    The DPL also works in systems that do not have a battery, i.e., where
    selected inverters are powered by solar panels directly.
    [Configure](configuration/dpl.md) the DPL accordingly. Battery-related
    features and settings do not apply for these setups.

!!!note "Systems without a power meter"
    Setups without access to a power meter reading are supported. However, this
    implies that the selected inverters will never produce more than the
    configured base load.

Refer to the documentation on [DPL parameters](configuration/dpl.md) to learn
how to configure the DPL.

## Terminology and Basics

!!!warning "Best Effort"
    The DPL can be slow (>10 seconds) to calculate and send new power limits.
    With tuning, the limit updates can be as frequent as one or two seconds.
    There will always be a delay. The DPL **cannot** be expected to react in
    "real-time" similar to a commercial inline battery/inverter combination.

### Inverters

It is expected that the DPL is in exclusive control over the inverters selected
to be governed by the DPL. No other software and no human interaction shall
trigger limit updates to be sent to the selected inverters.

### Battery Cycles

!!!note "Reboot State"
    After a reboot the battery is assumed to be in a charge cycle unless the
    SoC or voltage is found to be above the respective start threshold.

#### Charge Cycle

A battery charge cycle is started when the battery SoC or voltage falls below
the respective stop threshold. The charge cycle completes when the battery SoC
or voltage reaches the respective start threshold.

#### Discharge Cycle

The battery is or was charged to or beyond the start threshold. The discharge
cycle ends when the battery SoC or voltage reaches the respective stop
threshold.

### Voltage Measurements

If the DPL is configured such that voltage thresholds (rather than SoC
thresholds) are effective, or when the battery interface fails to provide
recent SoC values, the battery's voltage reading is taken from any of the
following sources, in the given order:

1. [Battery interface](battery_interface.md), i.e., the BMS
2. [Victron charge controller](../hardware/victron_charge_controllers.md)
   battery output terminal
3. Selected inverter input (configurable)

If available, the battery interface voltage reading is preferred, as it is
closest to the actual battery and therefore to be expected to be the most
accurate. Without a battery interface and if available, the Victron charge
controller output voltage is preferred, as it also is often closer to the
battery and more accurate. As a fallback, the inverter's input voltage
reading is assumed as the battery voltage.

### (Full) Solar-Passthrough

If the currently available solar power is known, the DPL can make additional
decisions on how to calculate the inverter limits. See the documentation on
[(Full) Solar-Passthrough](solar_passthrough.md).

**Full** Solar-Passthrough is implemented separately from the strategy outlined
below: All solar-powered inverters are set to 100% output power and the
battery-powered inverters are then configured to match the reported solar
output power.

### Strategy

1. The DPL calculates the desired total inverter output based on the power
   meter reading, the inverters' current output (if
   [configured](configuration/dpl.md#power-meter-reading-includes-inverter-output)),
   and the [target grid
   consumption](configuration/dpl.md#target-grid-consumption) value. If no
   power meter reading is available, the configured [base
   load](configuration/dpl.md#base-load) is used.
1. The calculated desired total inverter output is confined by the configured
   [maximum total output](configuration/dpl.md#maximum-total-output).
1. Solar-powered inverters, if any, are set up with limits such that the desired
   total inverter output is matched as closely as possible.
1. The expected output of all solar-powered inverters after their new update
   is applied is subtracted from the desired total inverter output.
1. An allowance of DC power is calculated that the battery-powered inverters
   may consume. The following is a possibly incomplete list of typical
   scenarios:
    * The battery charge may be below the [stop
      threshold](configuration/dpl.md#stop-threshold-for-battery-discharging)
      and no solar power is available, so the allowance is zero.
    * Even if the battery is empty, there might be solar power and if
      [Solar-Passthrough](solar_passthrough.md) is enabled, the allowance is
      equal to the solar output (adjusted for cable losses).
    * The battery may be charged beyond the [start
      threshold](configuration/dpl.md#start-threshold-for-battery-discharging),
      so the allowance is effectively infinite.
    * Even if the battery is fully charged, the battery discharge limit can
      reduce the allowance accordingly.
1. The remaining desired total inverter output is confined by the battery
   allowance.
1. Battery-powered inverters, if any, are set up with limits such that the
   remaining desired total inverter output is matched as closely as possible.

Choosing new limits for the inverters is based on the difference between the
current inverters' output of a particular group (solar-powered and
battery-powered inverters) and the desired output for that group. The DPL
adjusts the output of individual inverters to match the desired output. It
adjusts multiple inverters if the change cannot be covered by a single
inverter.

The DPL currently prefers inverters to be producing and only shuts down an
inverter when necessary to achieve a particular reduction.

Eligible inverters of a group are sorted in descending order of the amount they
can add to a particular increase or reduction of output power. This reduces the
number of inverters that need to receive an update to implement the change in
output power and hence increases the DPL's responsiveness.

# (Full) Solar-Passthrough

!!!note
    This feature only applies to systems with battery-powered inverters and is
    currently additionally limited to systems using a connected Victron charge
    controller reporting the solar output.

## Solar-Passthrough

Solar-Passthrough is a feature that, if enabled, makes the [DPL](dpl.md)
calculate the inverter limits such that the available solar power is first used
to cover the needs of the household. This effectively prevents the energy from
being stored in the battery just so it can be discharged later. If there is any
surplus solar power left, it will be used to charge the battery.

Refer to the [DPL documentation](dpl.md) to understand what is considered a
battery charge and discharge cycle.

### Solar-Passthrough is OFF

* The inverters are disabled during a battery charge cycle.
* All solar power will be used to charge the battery.
* The inverters are enabled as needed during a battery discharge cycle.
    * The inverters will use as much power as is needed to compensate the
      household consumption.
    * Depending on the current consumption and the available solar power, the
      battery is drained.

### Solar-Passthrough is ON

As with Solar-Passthrough OFF, in general the inverters are disabled during a
battery charge cycle, and they are enabled as needed during a battery discharge
cycle.

With Solar-Passthrough ON, there is an important difference while the battery
is in a charge cycle:

* If there is solar power available (Victron charge controller is producing
  power), it will be used to cover the household consumption.
    * The inverters are thus enabled as needed and producing power even though
      the battery is in a charge cycle.
    * The inverters' total power limit is **constrained to the solar power**.
      Therefore, discharging of the battery is avoided.
* Surplus solar power (available when solar output is greater than the
  household consumption) still charges the battery.

## Full Solar-Passthrough

There is an additional battery SoC/voltage threshold activating **Full**
Solar-Passthrough.

While the battery reached or is above the **Full** Solar-Passthrough threshold,
the inverters' limits are set to match the total solar power output (regardless
of the power meter value). This threshold is usually configured such that Full
Solar-Passthrough activates when the battery is considered fully charged.
Surplus solar power is then fed into the grid.

!!!note "Limitation"
    If the solar output power is greater than the inverters can feed or are
    allowed to feed into the grid, the charge controller will eventually switch
    to absorption and then to float mode. This reduces the charge controller's
    power output, and the inverters follow the new output power, and the
    production of AC power ceases.

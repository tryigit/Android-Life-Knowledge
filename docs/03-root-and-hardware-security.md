# Root Access, Sysfs, and Hardware Security

For power users and developers with root access, Android exposes the raw interface to the kernel's power supply subsystem. However, a common misconception is that having root access allows one to simply "edit" the battery health. This document explains why the hardware architecture makes such manipulation nearly impossible.

## The Kernel Interface (Sysfs)

On a rooted device, you can navigate to the power supply class to see raw driver values.

**Path:** `/sys/class/power_supply/battery/` (or `bms`, `main_battery` depending on OEM)

Common files of interest:

*   `cycle_count`: The number of charge cycles.
*   `charge_full`: The full charge capacity (FCC) in µAh.
*   `charge_full_design`: The design capacity in µAh.
*   `health`: The string status (e.g., "Good", "Dead").
*   `capacity`: The current battery percentage (0-100).

> [!WARNING]
> Writing to these files (e.g., `echo 100 > capacity`) is temporary. The kernel driver constantly polls the hardware (PMIC/Fuel Gauge) and will overwrite your value within milliseconds or on the next hardware interrupt.

## Hardware-Level Security: The Fuel Gauge IC

Modern smartphones use sophisticated Fuel Gauge Integrated Circuits (ICs) from manufacturers like Texas Instruments (TI) or Maxim Integrated. These are not simple counters; they are complex microcontrollers dedicated to battery management.

### Why You Can't "Spoof" Cycle Counts

1.  **Non-Volatile Memory (NVM):** The cycle count and learnt capacity (FCC) are stored in the Fuel Gauge's internal NVM (EEPROM or Flash). This memory is often One-Time Programmable (OTP) or protected by a security key to prevent tampering.
2.  **Coulomb Counting Algorithms:** The chip measures current flowing in and out of the battery with high precision. It integrates this over time to calculate capacity. It does not rely on the OS telling it what the capacity is; it tells the OS.
3.  **Impedance Tracking:** Advanced chips measure the internal resistance of the battery cells. As a battery ages, resistance increases. Even if you reset the cycle count, the high resistance measured by the chip would cause it to recalculate and report a lower State of Health (SoH) almost immediately.

### The Limits of Root

Root access gives you control over the **Operating System**, not the **Firmware** of peripheral components.

*   **OS Level (Root):** Can read/write files in `/sys/`, change UI, modify system props.
*   **Hardware Level (Fuel Gauge):** Operates on its own firmware. The OS driver merely acts as a translator.

To genuinely reset a battery's health stats, you would need to:
1.  Physically replace the battery cell (which has a new, uncalibrated controller or fresh chemistry).
2.  Use specialized hardware tools (I2C/SMBus programmers) to interface directly with the Fuel Gauge chip and reset its registers (if not password protected).

## Manual Calibration (Fuel Gauge Learning)

If the reported battery percentage drifts from reality (e.g., the device shuts down at 15%), you can force the Fuel Gauge to relearn the battery's absolute limits (0% and 100%). This is not a "reset" of health, but a synchronization of the reported capacity with the physical voltage.

1.  **Discharge to Cut-off:** Use the phone until it reaches 0% and powers off automatically.
2.  **Verify Empty State:** Try to turn the phone on again. If it boots and immediately shuts down, the battery has truly reached its low-voltage cut-off point.
3.  **Charge While Off:** Connect the device to an original, high-quality charger while it remains powered off.
4.  **Continuous Charge:** Charge without interruption until the indicator shows 100%.
5.  **Trickle Charge (Saturation):** Do not unplug immediately. Leave the device connected for an additional 1-2 hours. This ensures the "trickle charge" phase completes and allows the Fuel Gauge to register the precise saturation voltage (Term Taper Current).

**Conclusion:** The battery health data seen in Android 14+ APIs is trustworthy because it is rooted in physical hardware measurements that are resilient to software-level manipulation.

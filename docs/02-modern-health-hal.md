# Modern Health HAL & Android 14+ Standards

With Android 14 (API Level 34), Google introduced a fundamental shift in how the operating system interacts with battery hardware. The reliance on user-space estimations and generic kernel drivers has been replaced by a standardized Hardware Abstraction Layer (HAL) specifically for battery health.

## The Paradigm Shift: From Estimation to Interrogation

In previous versions, the OS "guessed" health based on charge cycles and current capacity. The new approach is to "interrogate" the hardware. Modern Power Management Integrated Circuits (PMICs) and Fuel Gauge ICs maintain their own internal registers for health data. Android now simply asks for this data.

### Key Components

1.  **Health HAL (AIDL):** The interface definition language that standardizes communication between the Android Framework and the vendor's hardware implementation.
2.  **BatteryManager APIs:** New methods added to `BatteryManager` that expose the HAL data to applications.
3.  **Fuel Gauge IC:** The physical chip responsible for monitoring the cell.

## Android 15 Implementation

Android 15 mandates stricter adherence to these HAL definitions for Google Mobile Services (GMS) certification. This means OEMs must implement the drivers to correctly report:

*   **Cycle Count:** The total number of charge/discharge cycles as recorded by the fuel gauge's non-volatile memory.
*   **State of Health (SoH):** A percentage value calculated by the fuel gauge's proprietary algorithm (e.g., Coulomb Counting + Impedance Tracking).
*   **Manufacturing Date:** The date the battery cell was produced, read from the battery's EEPROM if available.

### Code Example: Accessing Modern Health Data

The following snippet demonstrates how to access these new properties using the `BatteryManager` API in Android 14+:

```kotlin
import android.os.BatteryManager

fun getBatteryHealth(context: Context) {
    val batteryManager = context.getSystemService(Context.BATTERY_SERVICE) as BatteryManager

    // Requires API Level 34+
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.UPSIDE_DOWN_CAKE) {
        val cycleCount = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CHARGING_CYCLE_COUNT)
        val manufacturingDate = batteryManager.getLongProperty(BatteryManager.BATTERY_PROPERTY_MANUFACTURING_DATE)
        val stateOfHealth = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_STATE_OF_HEALTH)

        Log.d("HealthHAL", "Cycles: $cycleCount, SoH: $stateOfHealth%")
    }
}
```

## Why This Matters

This architecture eliminates the "guesswork". The values you see are not calculated by the OS on the fly; they are retrieved directly from the hardware.

> [!NOTE]
> If a device reports `0` or `-1` for these values, it indicates that the OEM's kernel driver has not implemented the specific HAL method to map the fuel gauge register to the Android Framework, or the hardware lacks the capability.

For a deeper dive into how this data is secured and why it cannot be easily spoofed, refer to the [Root & Hardware Security](./03-root-and-hardware-security.md) documentation.

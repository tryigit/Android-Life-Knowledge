# Legacy Methods and The PowerProfile Hack

Before the standardization of battery health APIs in Android 14, developers and users relied on a collection of "hacks" and estimations to guess the state of the battery. This document details the most common method, its implementation, and why it was fundamentally flawed.

## The PowerProfile "Hack"

For years, the most cited method for calculating battery health involved accessing the internal `PowerProfile` class. This class is designed by Android to estimate power consumption (in mAh) for various hardware components (CPU, WiFi, Screen) to attribute battery usage to specific apps. It was **never** intended to report battery health.

### How it worked

The logic relied on a simple ratio:

1.  **Retrieve Design Capacity:** Read `getBatteryCapacity()` from `com.android.internal.os.PowerProfile`. This value is statically defined in `power_profile.xml` by the device manufacturer (OEM).
2.  **Retrieve Current Charge Counter:** Read the `CHARGE_COUNTER` property from the `BatteryManager` API (or via sysfs `charge_counter`), which gives the current charge in microampere-hours (µAh).
3.  **The Formula:**

```java
// Simplified pseudo-code of the legacy hack
double designCapacity = powerProfile.getBatteryCapacity(); // e.g., 4500 mAh
double currentCharge = batteryManager.getLongProperty(BatteryManager.BATTERY_PROPERTY_CHARGE_COUNTER) / 1000.0;
double batteryLevel = batteryManager.getIntProperty(BatteryManager.BATTERY_PROPERTY_CAPACITY) / 100.0;

// Attempt to reverse-calculate full capacity
double estimatedFullCapacity = currentCharge / batteryLevel;

double healthPercentage = (estimatedFullCapacity / designCapacity) * 100;
```

### Why this method died

This approach suffered from critical reliability issues that made it effectively useless for accurate diagnostics:

1.  **Static XML Values:** The `designCapacity` is hardcoded in an XML file (`frameworks/base/core/res/res/xml/power_profile.xml`). OEMs often copy-paste this file between devices or enter incorrect values (e.g., listing 4500mAh for a 4800mAh typical cell).
2.  **Snapshot Inaccuracy:** The `charge_counter` fluctuates significantly based on temperature, load, and the Battery Management System's (BMS) current estimation state. A single snapshot calculation introduces massive variance.
3.  **Kernel "MaxLearned" Variance:** Some apps tried to read `charge_full` or `charge_full_design` from `/sys/class/power_supply/battery/`. While better, these values are kernel driver estimates that reset on reboot or vary wildy depending on the specific charging controller driver (Qualcomm vs MediaTek vs Samsung LSI).

## The End of an Era

With the introduction of **Android 14**, Google acknowledged the need for a standardized, hardware-backed API. The reliance on `power_profile.xml` for health estimation is now considered an anti-pattern. Modern analysis requires direct communication with the fuel gauge hardware, which is detailed in the [Modern Health HAL](./02-modern-health-hal.md) documentation.

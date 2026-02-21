# Android Life Knowledge

**The definitive technical resource for Android hardware health, system metrics, and lifecycle evolution.**

---

This repository documents the architectural shift in how the Android operating system monitors, reports, and secures hardware health metrics—specifically focusing on battery subsystems. It serves as a bridge between legacy estimation methods and modern hardware abstraction layers.

## Documentation Index

Explore the detailed technical breakdowns below:

### 1. [Legacy Methods & The PowerProfile Hack](./docs/01-legacy-methods-and-hacks.md)
> *Why the old `getBatteryCapacity()` method was never accurate.*
>
> Learn about the historical reliance on `power_profile.xml`, the flawed math behind user-space estimation apps, and why these methods are now considered obsolete anti-patterns.

### 2. [Modern Health HAL (Android 14+)](./docs/02-modern-health-hal.md)
> *The new standard: Asking the hardware directly.*
>
> A deep dive into the Android 14/15 Health HAL (AIDL), direct PMIC communication, and how the OS retrieves trusted data like Cycle Count and Production Date.

### 3. [Root, Sysfs & Hardware Security](./docs/03-root-and-hardware-security.md)
> *The limits of software control.*
>
> An analysis of the `/sys/class/power_supply` interface and the hardware-level security mechanisms (Fuel Gauge ICs, ROM/OTP memory) that prevent data spoofing even on rooted devices.

---

## Vision

As Android matures, the gap between "software estimation" and "hardware reality" is closing. This project aims to:
*   Debunk myths surrounding battery calibration and health "resets".
*   Provide accurate, engineering-level explanations of system behaviors.
*   Guide developers toward using the correct, modern APIs (`BatteryManager` properties) instead of deprecated hacks.

## Contributing

Corrections and technical additions are welcome. Please ensure all contributions are backed by AOSP source code references or hardware datasheets.

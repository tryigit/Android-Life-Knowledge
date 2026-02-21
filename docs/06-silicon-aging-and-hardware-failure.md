# Silicon Aging & Hardware Failure

While [Software Lifecycle](./05-software-lifecycle-and-updates.md) dictates how long a device remains secure and feature-rich, the **Hardware Lifecycle** determines how long the physical device will actually turn on and function correctly.

Unlike software, which does not degrade, physical silicon and the materials surrounding it are subject to wear and tear at the atomic level.

## 1. Electromigration: The Silent Killer

Electromigration is the gradual movement of the ions in a conductor due to the momentum transfer between conducting electrons and diffusing metal atoms.

*   **The Physics:** As electricity flows through the microscopic copper or aluminum traces inside a CPU, the "wind" of electrons can physically knock atoms out of place.
*   **The Result:** Over time, this creates **voids** (gaps that break the connection) or **hillocks** (piles of atoms that cause short circuits).
*   **Acceleration:** High temperatures and high voltages (often used in "performance modes" or overclocking) exponentially increase the rate of electromigration.

## 2. The "Slow Down" Myth vs. Instability

A common myth is that processors "get slower" over time like a car engine losing horsepower. **This is false.** A transistor works at its set frequency, or it doesn't.

However, as a chip ages due to electromigration and gate oxide breakdown:
1.  **Instability:** The chip requires more voltage to maintain stability at the same frequency.
2.  **crashes:** If the voltage isn't increased, the phone may freeze or reboot randomly.
3.  **Throttling:** To prevent crashes, the system might be forced to run at lower frequencies, creating the *perception* of slowness.

## 3. Thermal Cycling & Solder Fatigue

Often, it is not the silicon itself that fails, but the connection between the chip and the motherboard.

*   **BGA (Ball Grid Array):** Modern mobile chips are soldered onto the board using hundreds of tiny solder balls.
*   **Expansion & Contraction:** Every time your phone heats up (gaming, fast charging) and cools down, the chip and the motherboard expand and contract at different rates.
*   **Micro-Cracks:** Over thousands of cycles, this stress causes micro-cracks in the solder joints. Eventually, a critical pin disconnects, leading to "Bootloops" or "Sudden Death".
    *   *Historical Example:* The LG G4 and Nexus 5X bootloop issues were largely attributed to solder joint failure caused by thermal stress on the Snapdragon 808/810 chips.

## 4. Flash Memory Wear (UFS/eMMC)

As detailed in [UFS Storage Health](./04-storage-ufs-health.md), the storage chip has a finite number of write cycles. When the storage degrades:
*   **Read/Write Errors:** The system hangs while waiting for data.
*   **Data Corruption:** Apps crash or files disappear.
*   **ReadOnly Mode:** The chip locks itself to prevent further data loss, making the phone unusable.

## Summary: How to Extend Hardware Life

1.  **Avoid Heat:** Heat is the primary accelerator of all these failure modes.
2.  **Avoid constant high voltage:** Keeping the battery at 100% (high voltage state) stresses the power management circuits.
3.  **Don't chase benchmarks:** Repeatedly running stress tests significantly ages the silicon and solder joints.

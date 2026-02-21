# Chip Lifecycle & The "Update" Illusion

A common misconception in the Android ecosystem is that a "System Update" renews the entire software stack. In reality, for most devices, an Android version upgrade (e.g., Android 13 to 14) is merely a facelift for the upper application layers, while the foundation—the kernel and hardware drivers—remains frozen in time.

## The "Fake" Update Problem

When you buy a phone with Android 14, it ships with a specific Linux Kernel version (e.g., 5.15 or 6.1) and a set of proprietary drivers (GPU, Modem, Wi-Fi) provided by the chipset manufacturer.

Years later, when that phone receives Android 16, it is overwhelmingly likely that:
1.  **The Kernel is unchanged:** It is still running the original 5.15 kernel, just with security patches backported.
2.  **Drivers are old:** The GPU driver is likely the same version that shipped on day one.

This creates a "Frankenstein" system: a modern user interface running on top of an aging, potentially vulnerable hardware interface.

## The Vendor Support Hierarchy

The primary bottleneck for true long-term support (LTS) is the chipset vendor (SoC manufacturer). Once they stop updating the Board Support Package (BSP) for a specific chip, the phone manufacturer (OEM) is effectively stranded.

### 1. Qualcomm (The "Standard")
Historically, Qualcomm has provided support for approximately 3-4 years of Android updates. After this period, they stop validating new Android kernels on their older chips.
*   **Support Tier:** Limited.
*   **Impact:** Once Qualcomm drops a chip (e.g., Snapdragon 888), no phone using that chip can easily receive a *true* kernel upgrade. Custom ROM developers struggle to port newer Android versions because the proprietary binaries (blobs) are incompatible with newer kernels.

### 2. MediaTek (The "Middle Ground")
MediaTek has historically lagged behind Qualcomm in open-source contribution, making their chips notorious in the custom ROM community. However, in recent years, their official support window has improved to match the industry standard.
*   **Support Tier:** Intermediate.
*   **Impact:** Better than in the past, but still reliant on the vendor's roadmap. Updates often arrive later than their Qualcomm counterparts.

### 3. OEM Custom Silicon (Google Tensor, Samsung Exynos)
This is where the paradigm shifts. Companies like Google (Tensor) and Samsung (Exynos) control both the silicon design and the operating system integration.
*   **Support Tier:** Full / Extended (7+ Years).
*   **Impact:** Because Google owns the Tensor chip, they can decide to write new drivers for it 6 years later. This allows the Pixel 8 series, for example, to promise 7 years of OS updates that include *actual* kernel version bumps, keeping the device secure and performant at a hardware level.

## Development Deep Dive: Why is it so hard?

Why can't an OEM just write their own drivers?

### 1. Proprietary Blobs
Hardware drivers for GPUs (Adreno/Mali), Modems, and Image Signal Processors (ISPs) are closed-source trade secrets. They are distributed as pre-compiled binaries ("blobs") linked against a specific kernel version. If you update the kernel, the blob breaks. Without the source code, you cannot recompile it.

### 2. Certification Cost
Google requires devices to pass the **VTS (Vendor Test Suite)** and **CTS (Compatibility Test Suite)** to ship with Play Services. Updating a kernel requires re-certifying the entire hardware stack, a process that costs millions of dollars in engineering hours and testing fees per device model.

### 3. Planned Obsolescence vs. Engineering Reality
While planned obsolescence is a factor, the sheer complexity of maintaining a BSP for 5+ years is technically immense. It requires a dedicated team to constantly backport patches to an ancient kernel (e.g., Linux 4.14) or refactor closed-source drivers to work with a new kernel (e.g., Linux 6.1). For most manufacturers, the cost-benefit analysis favors releasing a new phone instead.

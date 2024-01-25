# Battery Healt
The Android kernel is currently reading the battery health information in this file.
```
/sys/class/powersupply/battery/charge_full
```
```
/sys/class/powersupply/battery/cycle_count
```
> [!WARNING]
> It is possible to edit this file on old Android devices. However, if you exaggerate and enter an incorrect value, your phone may indicate the battery incorrectly and shut down prematurely.

# Storage Life
The Android kernel is currently reading the battery health information in this folder.
```
/sys/devices/platform/soc/1d84000.ufshc/health_descriptor/
```
> [!IMPORTANT]
> There is no known benefit or harm in editing this file. Ufs health information is given in the image below.

![Storage Healt](https://raw.githubusercontent.com/tryigit/AndroidHealth/main/IMG_20240126_022748_336.jpg)

> [!TIP]
> Software hardware lifespan data does not represent the actual lifespan. It's just a software profile. Restrictions can be added to prevent the phone from suddenly shutting down and other things.

# Battery Life
The Android kernel is currently reading the battery health information in this file.

### Battery Value
```
/sys/class/powersupply/battery/charge_full
```
### Charging Cycle
```
/sys/class/powersupply/battery/cycle_count
```
### Qualcomm Battery Life (Found)
These files has never been tested and it is not known exactly what it does. For example, it may affect cpu/gpu performance 💀
```
/sys/class/qcom-battery/soh
```
```
/sys/class/qcom-battery/fg1_soh
```
> [!WARNING]
> It is possible to edit this file on old Android devices. However, if you exaggerate and enter an incorrect value, your phone may indicate the battery incorrectly and shut down prematurely.

# Storage Life
The Android kernel is currently reading the battery health information in this folder.
```
/sys/devices/platform/soc/1d84000.ufshc/health_descriptor/
```
> [!IMPORTANT]
> There is no known benefit or harm in editing this file. However, editing is not recommended.

Ufs health information is given in the image below.

![Storage Healt](https://raw.githubusercontent.com/tryigit/AndroidHealth/main/IMG_20240126_022748_336.jpg)

> [!TIP]
> Software hardware lifespan data does not represent the actual lifespan. It's just a software profile. Restrictions can be added to prevent the phone from suddenly shutting down and other things.

## Telegram
https://t.me/chinacloudgroup

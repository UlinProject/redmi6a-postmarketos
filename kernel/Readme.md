
**Directory:** `/home/username/.local/var/pmbootstrap/cache_git/pmaports/device/archived/linux-xiaomi-cactus/`

## Patch list
|  name  |  info  |
| ----------------------------------------- | ----------------------------- | 
| 0001-Port-build-scripts-to-Python3.patch  | Port build scripts to Python3 |
| 0002-fix-modern-gcc-assembler.patch       | arm: fix asm section attributes for modern toolchains |
| 0003-kill-mtk-wdt-hardware.patch          | drivers: watchdog: mtk_wdt: stub out watchdog to prevent unexpected reboots |
| 0004-kill-mtk-pbm-spam.patch              |  |
| 0005-disaux-logs.patch                    | drivers: misc: mediatek: pmic: mute verbose auxadc raw read logging |
| 0006-dispmucharget-logs.patch             | drivers: misc: mediatek: pmic: mt6370: silence chatty charger register dump logs |
| 0007-dispmucharget-logs.patch             | drivers: misc: mediatek: mt6370: aggressively stub out driver logging macros |
| 0008-dispmucharget-logs.patch             | drivers: misc: mediatek: pmic: mute pr_notice logs across multiple PMIC drivers |
| 0009-disfocaltouch-logs.patch             | drivers: input: touchscreen: focaltech: silence verbose FTS_INFO logging |
| 0010-distickbroadcast-logs.patch          | timers: mtk: disable AEE dump in tick broadcast |
| 0012-addbacklight.patch                   | drivers: misc: mediatek: leds: expose lcd-backlight via standard Linux backlight class |
| 0013-mtkkpd.patch                         | drivers: input: keyboard: mtk_kpd: expose additional key bits for X11/evdev compatibility |
| 0014-battery.patch                        | drivers: power: supply: mtk: implement linear SoC fallback with smoothing filter |
| 0015-disableusbboost.patch                | drivers: misc: mediatek: usb_boost: disable CPU frequency lock on USB connection |
| 0016-dissleptdispwarning.patch            | drivers: misc: mediatek: video: mute sleep-state DISPWARN logs |
| 0017-disdlpt.patch                        | drivers: misc: mediatek: pmic: mt6357: disable DLPT power throttling thread |

## Will there be any more patches?

At this stage, I’ve gotten everything I wanted out of this old device. It runs surprisingly well on the adjusted legacy kernel. Any future development or changes for this device are planned to be focused entirely on the **mainline kernel** branch, should I decide to return to this project.

## License
This project is licensed under the **GNU General Public License v2.0** - see the [LICENSE](../LICENSE) file for details.

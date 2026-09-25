<div id="header" align="center">

  <b>[redmi6a-postmarketos]</b>
  
  (Description and patches for launching postmarketOS..)
  </br></br>
<div id="badges">
  <a href="https://github.com/denisandroid">
    <img src="https://github.com/UlinProject/img/blob/main/short_32/uproject.png?raw=true" alt="uproject"/>
  </a>
</div>
</div>

## Description
This is a simple weekend project: running the current version of postmarketOS on a long-obsolete smartphone. Don't expect a turn-key solution that can serve as your primary daily driver.

## Disclaimer
| :boom: Please Read Carefully |
|:------------------|
| :warning: **Xiaomi** and **Redmi** are registered trademarks of Xiaomi Inc. The author has no affiliation with Xiaomi and does not provide hardware advice. For official support, please refer to Xiaomi’s website. |
| :warning: **Bootloader unlocking is not covered in this guide.** All instructions provided here assume that your device already has an unlocked bootloader. |
| :warning: Anything you do based on the information provided here is **entirely at your own risk**. The author is not responsible for any damage to your device (bricking, bootloops, hardware failure, etc.). |
| :warning: All information is based on personal research and experimentation; it **may contain errors** or outdated steps. |
| :warning: This material is provided strictly for **informational and educational purposes** only. |

## Specifications

| name | value | status |
| ---- | ----- | ------ |
| board / codename | xiaomi-cactus |  |
| SoC | MediaTek Helio A22 (MT6762M / MT6761) |  |
| kernel | 4.9.117 (armv7) | **Working.** The kernel and OS only boot in `armv7` mode. Running `aarch64` is not recommended until mainline kernel support is achieved. |
| cpu | Quad-core 2.0 GHz Cortex-A53 (12nm) | **Working.** Governors: `schedplus`, `powersave`, `conservative`, and `ondemand`. *Note: `schedutil` performs poorly.* |
| gpu | PowerVR Rogue GE8320 | **WIP.** GPU firmware can be loaded, but nothing beyond that. No active attempts to enable full hardware acceleration yet; waiting for mainline kernel support. |
| mem | 2 GB LPDDR3 | **Working** out of the box. |
| storage | 16 GB / 32 GB eMMC | **Working.** Both internal eMMC and external MicroSD cards are fully functional. |
| display | 5.45" IPS LCD 1440×720 | **Working.** Backlight brightness control patch applied; works flawlessly. |
| touchscreen | FocalTech (fts_ts) | **Working** out of the box. |
| sound | Mono speaker, 3.5mm jack | **Unstable.** Unclear behavior; certain modes may trigger a kernel panic. It is highly recommended to disable or avoid using audio for now. |
| usb | Micro-USB 2.0 (with OTG support) | **Working.** Only OTG and legacy RNDIS (USB networking) have been tested and verified. |
| connectivity | Wi-Fi, Bluetooth, GPS | **Untested.** No attempts have been made to initialize or test wireless modules yet. |
| sensors | Accelerometer, Light & Proximity | **Untested.** Not worked on or initialized. |
| camera | Front & Rear | **Untested.** Not worked on or initialized. |
| battery | BN37 (Max 4.4V) | **Partial.** A primitive linear capacity calculation patch is applied (not suitable for daily driver use). If you prefer to use the proprietary MTK downstream kernel blob, do not apply this patch. |
| leds | Front notification LED, Flashlight | **Working.** Both LEDs are fully operational. |
| buttons | Volume Up, Volume Down, Power | **Working.** Key patch added. |

## Software

Running a modern Linux software stack on this hardware has its nuances:
* **Toolkits:** `Qt` applications run fully and stably out of the box. Conversely, `GTK` applications fail to launch due to an outdated kernel and a broken `bwrap` (bubblewrap) sandbox mechanism, though applying custom patches slightly improves the situation.
* **Browsers:** Both `Chromium` and `Firefox` run stably.
* **Display Server & Window Managers:** `X11` works flawlessly, and `Openbox` is highly recommended as a lightweight starting point. No attempts have been made to run `Wayland` environments.
* **Performance:** Overall system responsiveness and performance are exactly what you would expect from a low-end SoC of this generation.

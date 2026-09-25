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
| leds | Front notification LED, Flashlight | **Working.** Both LEDs are fully operational. (/sys/class/leds/flashlight/brightness - flashlight; /sys/class/leds/blue/brightness - LED on the screen)|
| vibration | — | **Working.** However, no custom patches were made to integrate it into standard Linux subsystem frameworks. To trigger vibration, you must manually set the duration first and then write `1` to `activate`. (/sys/class/leds/vibrator) |
| buttons | Volume Up, Volume Down, Power | **Working.** Key patch added. |

## Software

Running a modern Linux software stack on this hardware has its nuances:
* **Init System:** Only `OpenRC` is currently supported and working. `systemd` is completely non-functional at this stage.
* **Toolkits:** 
  * `Qt` applications run fully and stably out of the box, with excellent touchscreen responsiveness.
  * `GTK` applications fail to launch cleanly due to an outdated kernel and a broken `bwrap` (bubblewrap) sandbox mechanism. Even with custom patches applied, touchscreen behavior remains problematic. For example, in `Xfce`, tapping the main application menu does not trigger an action no matter how many times you click it, whereas panel widgets like the clock/date menu respond perfectly.
* **Browsers:** Both `Chromium` and `Firefox` run stably.
* **Display Server & Window Managers:** `X11` works flawlessly, and `Openbox` is highly recommended as a lightweight starting point. No attempts have been made to run `Wayland` environments.
* **Performance:** Overall system responsiveness and performance are exactly what you would expect from a low-end SoC of this generation.

## Patches

A collection of workarounds and fixes to resolve various issues. Keep in mind that most major problems and limitations stem directly from the outdated kernel.

<details> 
  <summary><b># Time & Date (chronyd)</b></summary>
  
  **File:** `/etc/conf.d/chronyd`
  
  ```ini
  command_args="-F 0"
  ```

  Without this, chronyd won't even start.
</details>

<details> 
  <summary><b># GTK & Icons Fix (bwrap bypass)</b></summary>

  Because sandbox isolation via `bwrap` fails completely on this outdated kernel, launching most modern GTK applications is normally impossible. This patch acts as a workaround by substituting `bwrap` with a wrapper script to bypass isolation entirely. 
  
  > ⚠️ **Note:** This is not a proper solution. Be aware that system updates may overwrite this file and restore the original `bwrap` binary.

  ### Step 1: Remove or backup the original binary
  ```bash
  sudo rm /usr/bin/bwrap
  ```
  ### Step 2: Create the wrapper script
  **File:** `/usr/bin/bwrap`
  
  ```sh
  #!/bin/sh

if echo "$@" | grep -q "glycin-svg"; then
    DBUS_FD=$(echo "$@" | grep -o -- '--dbus-fd [0-9]*' | awk '{print $2}')
    
    export container=bwrap
    export UNDER_BWRAP=1

    if [ -x "/usr/libexec/glycin-loaders/2+/glycin-svg" ]; then
        exec "/usr/libexec/glycin-loaders/2+/glycin-svg" --dbus-fd "$DBUS_FD"
    elif [ -x "/usr/libexec/glycin-loaders/glycin-svg" ]; then
        exec "/usr/libexec/glycin-loaders/glycin-svg" --dbus-fd "$DBUS_FD"
    elif [ -x "/home/alarm/.cache/glycin/usr/libexec/glycin-loaders/2+/glycin-svg" ]; then
        exec "/home/alarm/.cache/glycin/usr/libexec/glycin-loaders/2+/glycin-svg" --dbus-fd "$DBUS_FD"
    fi
fi

while [ $# -gt 0 ]; do
    case "$1" in
        --) shift; exec "$@";;
        -*) shift;;
        *) exec "$@";;
    esac
done
  ```

  ```bash
  chmod +x /usr/bin/bwrap
  ```
</details>


<details> 
  <summary><b># RNDIS Disconnects & Slow SSH Login (USB Ethernet)</b></summary>

  This issue is caused by `NetworkManager`, which is not fully debugged for this device. Disconnecting and reconnecting the USB cable breaks the RNDIS connection, and SSH logins experience significant delays. To resolve this, you can configure NetworkManager to ignore the `rndis0` interface and let `unudhcpd` handle it instead.
  
  ### Step 1: Make rndis0 unmanaged in NetworkManager
  **File:** `/etc/NetworkManager/conf.d/99-unmanaged-devices.conf`
  ```ini
  [keyfile]
  unmanaged-devices=interface-name:rndis0
  ```

  ### Step 2: Symlink and enable the unudhcpd service
  Run the following commands to create a dedicated service instance for `rndis0` and add it to the default runlevel:
  ```bash
  sudo ln -s /etc/init.d/unudhcpd /etc/init.d/unudhcpd.rndis0
  sudo rc-update add unudhcpd.rndis0 default
  sudo rc-service unudhcpd.rndis0 start
  ```
</details>

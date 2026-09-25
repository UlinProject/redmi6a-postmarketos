## Build Instructions

You must already have an unlocked smartphone running the latest official version of Android.

### Step 1: Clean Your Local Environment
Before starting a fresh build, it is highly recommended to purge old chroot mounts and clear stale build caches to prevent conflicts.

```bash
# Force unmount any active chroot containers
pmbootstrap shutdown

# Safely purge temporary chroots and build rootfs files
pmbootstrap zap
```

### Step 2: Initialize pmbootstrap
Run the configuration wizard to initialize the repository. 

```sh
pmbootstrap init
```

During the interactive setup, make sure to select the following configuration variables:
```
[23:39:35] Location of the 'work' path. Multiple chroots (native, device arch, device rootfs) will be created in there.
[23:39:35] Work path [/home/alarm/.local/var/pmbootstrap]: 
[23:39:44] Location of the 'pmaports' path, containing package definitions.
[23:39:44] pmaports path [/home/alarm/.local/var/pmbootstrap/cache_git/pmaports]: 
[23:39:49] Choose the postmarketOS release channel.
[23:39:49] Available (14):
[23:39:49] * edge: Rolling release / Most devices / Occasional breakage: https://postmarketos.org/edge
[23:39:49] * v26.06: Latest release / Recommended for best stability
[23:39:49] * v25.12: Old release (unsupported)
[23:39:49] Channel [edge]: edge
[23:39:54] NOTE: pmaports is on main branch, copying git hooks.
[23:39:54] Choose your target device vendor (either an existing one, or a new one for porting).
[23:39:54] Available vendors (107): acer, alcatel, amazon, amediatech, amlogic, apple, ark, arrow, asus, ayaneo, ayn, bananapi, barnesnoble, beelink, blackberry, bq, clockworkpi, cubietech, cutiepi, dell, dongshanpi, epson, essential, fairphone, finepower, fly, fxtec, generic, gigaset, goclever, google, gp, hisense, hp, htc, huawei, inet, infocus, jolla, khadas, klipad, kobo, lark, leeco, lenovo, lg, librecomputer, linksys, lynx, mangopi, medion, meizu, microsoft, mnt, mobvoi, motorola, nextbit, nobby, nokia, nothing, nvidia, odroid, oneplus, oppo, ouya, pine64, planet, pocketbook, postmarketos, powkiddy, purism, qcom, qemu, qualcomm, radxa, raspberry, realme, rockchip, samsung, semc, sharp, shift, sipeed, solidrun, sony, sourceparts, sqfmi, starway, surftab, t2m, thundercomm, tokio, tolino, trekstor, vernee, vivo, volla, wd, wexler, wiko, wileyfox, xiaomi, xunlong, yu, zhihe, zte, zuk
[23:39:54] Vendor [xiaomi]: xiaomi
[23:39:55] Devices are categorised as follows, from best to worst:
* Main: ports where mostly everything works.
* Community: often mostly usable, but may lack important functionality.
* Testing: anything from "just boots in some sense" to almost fully functioning ports.
* Downstream: ports that use a downstream kernel — very limited functionality. Not recommended.

Available devices by codename (22): begonia (testing), beryllium (community), cepheus (testing), clover (testing), dipper (downstream), elish (testing), equuleus (testing), gauguin (testing), ido (downstream), jasmine_sprout (testing), lancelot (testing), lavender (testing), miatoll (testing), mocha (testing), monet (downstream), nabu (testing), pipa (testing), pyxis (testing), scorpio (testing), tulip (testing), vayu (testing), whyred (testing)
[23:39:55] Device codename [cactus]: cactus
[23:39:56] WARNING: xiaomi-cactus is archived: Kernel does not build anymore
[23:39:56] Continue? (y/n) [n]: y
[23:40:01] Username [alarm]: alarm
[23:40:02] Available providers for postmarketos-base-ui-audio-backend (2):
[23:40:02] * pulseaudio: Use pulseaudio as the audio backend. (default)
[23:40:02] * pipewire: Use pipewire as the audio backend. (but may not work with all devices)
[23:40:02] Provider [default]: 
[23:40:05] Available providers for postmarketos-base-ui-wifi (2):
[23:40:05] * wpa_supplicant: Use wpa_supplicant as the WiFi backend. (default)
[23:40:05] * iwd: Use iwd as the WiFi backend (but may not work with all devices)
[23:40:05] Provider [default]: 
[23:40:06] Available providers for postmarketos-usb-moded-default-profile (2):
[23:40:06] * developer: Make 'developer mode' the default usb-moded profile (always enables usb networking) (default)
[23:40:06] * charging: Make 'charging mode' the default usb-moded profile (usb networking must be manually enabled)
[23:40:06] Provider [default]: 
[23:40:10] Available user interfaces (15): 
[23:40:10] * none: Bare minimum OS image for testing and manual customization. The "console" UI should be selected if a graphical UI is not desired.
[23:40:10] * buffyboard: Plain framebuffer console with modern touchscreen keyboard support
[23:40:10] * console: Console environment, with no graphical/touch UI
[23:40:10] * fbkeyboard: Plain framebuffer console with touchscreen keyboard support
[23:40:10] * gnome: (Wayland) Gnome Shell
[23:40:10] * gnome-mobile: (Wayland) Gnome Shell patched to adapt better to phones (Experimental)
[23:40:10] * i3wm: (X11) Tiling WM (keyboard required)
[23:40:10] * lxqt: (X11) Lightweight Qt Desktop Environment (stylus recommended)
[23:40:10] * mate: (X11) MATE Desktop Environment, fork of GNOME2 (stylus recommended)
[23:40:10] * openbox: (X11) A highly configurable and lightweight X11 window manager (keyboard required)
[23:40:10] * os-installer: UI for installing postmarketOS
[23:40:10] * plasma-desktop: (Wayland) KDE Desktop Environment (works well with tablets)
[23:40:10] * sxmo-de-dwm: Simple Mobile: Mobile environment based on SXMO and running on dwm
[23:40:10] * sxmo-de-i3: Simple Mobile: Mobile environment based on SXMO and running on i3
[23:40:10] * windowmaker: (X11) Window manager inspired by the NeXTSTEP user interface (stylus recommended)
[23:40:10] * xfce4: (X11) Lightweight desktop (stylus recommended)
[23:40:10] NOTE: 10 UIs are hidden because "deviceinfo_drm" is not set (see https://postmarketos.org/deviceinfo).
[23:40:10] User interface [openbox]: openbox
[23:40:13] WARNING: Kernel version 4.9.117 is lower than systemd's minimal requirement (5.4). Choosing systemd may result in non-bootable system. Get more information for systemd requirements at https://github.com/systemd/systemd/blob/main/README
[23:40:13] Based on your UI selection, 'default' will result in choosing openrc.
[23:40:13] Which service manager should be used? (default/openrc/systemd) [openrc]: openrc
[23:40:21] Additional options: extra free space: 0 MB, boot partition size: 512 MB, parallel jobs: 4, ccache per arch: 5G, sudo timer: False, mirror: http://mirror.postmarketos.org/postmarketos/
[23:40:21] Change them? (y/n) [n]: n
[23:40:25] Additional packages that will be installed to rootfs. Specify them in a comma separated list (e.g.: vim,file) or "none"
[23:40:25] Extra packages [none]: none
[23:40:27] Your host timezone: Europe/Minsk
[23:40:27] Use this timezone instead of GMT? (y/n) [y]: y
[23:40:31] Choose your preferred locale, like e.g. en_US. Only UTF-8 is supported, it gets appended automatically. Use tab-completion if needed.
[23:40:31] Locale [ru_RU]: ru_RU
[23:40:32] Device hostname (short form, e.g. 'foo') [xiaomi-cactus]: 
[23:40:33] SSH public keys found (6):
[23:40:33] * 
[23:40:33] See https://postmarketos.org/ssh-key-glob for more information.
[23:40:33] Would you like to copy these public keys to the device? (y/n) [n]: n
[23:40:38] After pmaports are changed, the binary packages may be outdated. If you want to install postmarketOS without changes, reply 'n' for a faster installation.
[23:40:38] Build outdated packages during 'pmbootstrap install'? (y/n) [y]: y
[23:40:42] Zap existing chroots to apply configuration? (y/n) [y]: y
[sudo] 
[23:40:49] Unregister qemu binfmt (arm)
[23:40:49] DONE!
```

### Step 3: Inject Custom Kernel Patches

The custom patches included in this repository are required to compile the legacy `4.9.117` Android kernel with modern compiler toolchains. 

Run the following command from the root of this cloned repository to copy all patch files directly into the `pmbootstrap` git ports cache:

```bash
cp ./kernel/* ~/.local/var/pmbootstrap/cache_git/pmaports/device/archived/linux-xiaomi-cactus/
```

### Step 4: Compile the Kernel

Once the patches are safely placed in the target directory, trigger the compiler to build the modified kernel package:

```bash
pmbootstrap build linux-xiaomi-cactus --force
```
*The `--force` flag ensures that `pmbootstrap` notices the newly added manual patches and rebuilds the package from scratch.*

### Step 5: Install

```bash
pmbootstrap install
```

### Step 6: flash

```bash
pmbootstrap export
cd /tmp/postmarketos-export/
fastboot flash boot xiaomi-cactus-boot.img
fastboot flash userdata xiaomi-cactus-rootfs.img
fastboot reboot
```

<b>Make.md errors are possible; the information will be updated.</b>

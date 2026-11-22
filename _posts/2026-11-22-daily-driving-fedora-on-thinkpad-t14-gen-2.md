---
title: "Daily Driving Fedora on a ThinkPad T14 Gen 2"
layout: post
tags:
  - fedora
  - linux
  - thinkpad
  - kde
description: A practical guide to installing Fedora KDE on an Intel ThinkPad T14 Gen 2 and tuning it for daily use.
---

The ThinkPad T14 Gen 2 is a capable Linux laptop, but a smooth daily setup is about more than getting Fedora installed. Desktop choice, codecs, power management, hibernation, and graphics settings all affect how the machine feels away from a charger.

This guide focuses on the **Intel** T14 Gen 2. Lenovo sold multiple configurations, so check the exact CPU, display, and wireless hardware in your machine before following model-specific advice.

## Installation: Fedora KDE and Wayland

I recommend Fedora's [KDE Plasma Desktop](https://fedoraproject.org/kde/) edition. KDE Plasma is actively developed and gives you a customizable desktop with a strong Wayland experience. Fedora also offers other actively maintained desktops, so this is a preference rather than a claim that KDE is the only good option.

Download the current Fedora KDE image, back up your files, and install it using the Fedora installer. During setup:

- Connect to Wi-Fi and install updates when offered.
- Keep the default UEFI installation unless you have a reason to change it.
- If you use disk encryption, save the recovery information somewhere safe.
- Check that the installer is using the correct disk before confirming any partition changes.

After the first boot, update the system and restart:

```bash
sudo dnf upgrade --refresh
```

Fedora KDE normally starts a Wayland session by default. Confirm the session type in **System Settings → About this System** (the exact page name can vary by Plasma version). Before relying on the laptop, test Wi-Fi, Bluetooth, audio, brightness keys, the touchpad, external displays, suspend, and resume.

## Multimedia and third-party software

Fedora prioritizes software that meets its licensing and distribution policies. If you need additional codecs or redistributable software, RPM Fusion is a commonly used third-party repository. Read its [repository setup instructions](https://rpmfusion.org/Configuration) and use the commands for your Fedora release:

```bash
sudo dnf install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

For common multimedia support, RPM Fusion documents switching to its full FFmpeg build and installing additional GStreamer codecs:

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf install @multimedia --setopt="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
```

Only install what you need, and check the [current multimedia instructions](https://rpmfusion.org/Howto/Multimedia) if a command changes for a newer Fedora release. RPM Fusion is an independent project, not part of Fedora.

## Intel graphics: no proprietary display driver needed

You generally do **not** need to install a proprietary Intel graphics driver. The graphics stack for modern Intel integrated GPUs is provided by the Linux kernel and Mesa, and Fedora updates those components with the system. Keep Fedora updated rather than adding an old vendor driver.

There is a useful distinction between the graphics driver and media codecs. RPM Fusion's multimedia guide lists `intel-media-driver` for recent Intel hardware for hardware-accelerated video codecs:

```bash
sudo dnf install intel-media-driver
```

This is an optional media component, not a replacement for the kernel/Mesa graphics stack. Install it if your video applications need it, then verify hardware decoding in the application you care about.

## Battery life and charging

Start with Plasma's **Power Management** settings: lower the display brightness on battery, set a sensible screen-off timeout, and choose the power profile that fits your workload. Check the battery's reported capacity and health in Plasma or with `upower -e` followed by `upower -i <battery-device>`.

Avoid stacking multiple power-management tools without a reason. Fedora's default power-profile service and a separately configured tool such as TLP can conflict or make behavior hard to understand. Change one thing at a time, then compare battery drain during the same kind of use.

If you want to limit charging to preserve battery longevity, first check whether your firmware and current Plasma version expose a charge threshold for this model. Support can vary with firmware and laptop configuration; do not assume every ThinkPad exposes the same controls.

## Suspend, then hibernate after an hour

This is a good way to reduce battery drain when a laptop is left asleep for a long time, but hibernation needs to work first. Fedora installations commonly use compressed RAM swap (zram); zram alone cannot preserve a hibernation image after the laptop powers off. You need correctly configured **disk-backed swap** and the matching resume setup. With disk encryption, resume must also be configured to unlock and access that swap. Follow the [Fedora hibernation guidance](https://forge.fedoraproject.org/workstation/wg-docs/src/branch/master/notes/hibernationstatus.md) for your installation rather than copying generic swap commands.

First test that a normal hibernate and resume works reliably, with your files saved. Then create a systemd drop-in to set the delay:

```bash
sudo mkdir -p /etc/systemd/sleep.conf.d
sudo tee /etc/systemd/sleep.conf.d/60-suspend-then-hibernate.conf >/dev/null <<'EOF'
[Sleep]
AllowSuspendThenHibernate=yes
HibernateDelaySec=1h
EOF
```

The delay applies to **suspend-then-hibernate**, not to ordinary suspend. Test the mode from a terminal with:

```bash
systemctl suspend-then-hibernate
```

If your Plasma version offers **Suspend then Hibernate** as a lid-close or power-button action, select it in Power Management. Otherwise, the KDE action may still be plain suspend; consult the current [systemd sleep configuration documentation](https://www.freedesktop.org/software/systemd/man/latest/systemd-sleep.conf.html) and your desktop's power-management settings before changing lid behavior. Verify that the machine resumes normally before trusting it with unsaved work.

## Display scaling

Fractional scaling in KDE Wayland has improved, but some applications—especially older XWayland applications—can still look blurry or scale inconsistently. My conservative recommendation is to try **100% or 200% scaling** first. On a high-resolution panel, 200% can be a better starting point than a fractional value; choose what is comfortable for your eyes and applications.

## Watch for unusual resource use

Pay attention to battery drain, fan activity, and CPU/GPU use during normal work. A browser using the GPU is not automatically a problem: compositing and video decoding are expected to use it. If Firefox seems unusually busy, check `about:support` for graphics details and test video playback with hardware acceleration enabled. Also confirm that the formats your sites and media apps use are supported by the installed codecs. Missing codec support can make video decoding fall back to the CPU.

For a closer look at Intel GPU activity, install Fedora's `intel-gpu-tools` package and run `intel_gpu_top` while reproducing the workload. Compare before and after changing one setting, rather than turning off hardware acceleration based on a single reading. If power use becomes abnormal after an update, check the desktop's system monitor and recent system logs before changing several settings at once.

## A few habits that help

- Keep Fedora and firmware up to date; check whether Lenovo firmware updates are offered through [LVFS](https://fwupd.org/) and review them before installing.
- Test suspend and resume after major kernel or firmware updates.
- Keep a backup before changing partitions, swap, or encryption settings.
- When troubleshooting, change one setting at a time and repeat the same test.

Fedora KDE on the T14 Gen 2 can make a responsive daily laptop. The biggest wins are usually a current system, working multimedia support, measured power settings, and a tested hibernation setup—not installing a proprietary Intel graphics driver.

## References

- [Fedora KDE Plasma Desktop](https://fedoraproject.org/kde/)
- [RPM Fusion repository setup](https://rpmfusion.org/Configuration)
- [RPM Fusion multimedia instructions](https://rpmfusion.org/Howto/Multimedia)
- [Fedora hibernation guidance](https://forge.fedoraproject.org/workstation/wg-docs/src/branch/master/notes/hibernationstatus.md)
- [systemd sleep configuration](https://www.freedesktop.org/software/systemd/man/latest/systemd-sleep.conf.html)

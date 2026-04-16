# aibattery
GTK3 replacement for Power Monitor

* Tray icon that reflects battery level (using themed icons included as part of the XDG icon spec so they work with any theme)
* Tooltip showing 82% — ~2h 14m remaining or Charging (45 min to full)
* Right-click menu with Refresh and Quit

#### Setup

Setup assumes Debian. Aimed towards GTK3, Meson, C language, GCC.

```
sudo apt install meson ninja-build clang \
    libgtk-3-dev libayatana-appindicator3-dev
```

#### Battery Data Source

On Linux, battery info lives in `/sys/class/power_supply/`

This is the layout of my Thinkpad T480 which includes one user changable external battery as well as an internal battery plus.

```
$ ls -l /sys/class/power_supply/
total 0
lrwxrwxrwx 1 root root 0 Apr 15 14:10 AC -> ../../devices/pci0000:00/0000:00:1f.0/PNP0C09:00/ACPI0003:00/power_supply/AC
lrwxrwxrwx 1 root root 0 Apr 15 14:10 BAT0 -> ../../devices/LNXSYSTM:00/LNXSYBUS:00/PNP0A08:00/device:19/PNP0C09:00/PNP0C0A:00/power_supply/BAT0
lrwxrwxrwx 1 root root 0 Apr 15 14:10 BAT1 -> ../../devices/LNXSYSTM:00/LNXSYBUS:00/PNP0A08:00/device:19/PNP0C09:00/PNP0C0A:01/power_supply/BAT1
lrwxrwxrwx 1 root root 0 Apr 15 14:10 ucsi-source-psy-USBC000:001 -> ../../devices/platform/USBC000:00/power_supply/ucsi-source-psy-USBC000:001
lrwxrwxrwx 1 root root 0 Apr 15 14:10 ucsi-source-psy-USBC000:002 -> ../../devices/platform/USBC000:00/power_supply/ucsi-source-psy-USBC000:002
```

#### Project Structure

We seperate the battery logic from UI which makes the battery module independently testable.

```
battery-monitor/
├── meson.build
├── meson_options.txt
├── src/
│   ├── main.c
│   ├── battery.c / battery.h     # sysfs reading, time estimation
│   └── tray.c / tray.h           # appindicator + GTK menu
└── data/
    └── icons/                    # optional custom icons (we default to the XDG system themed icons)
```


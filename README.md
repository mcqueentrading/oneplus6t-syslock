# Syslock for OnePlus 6T

This is not the generic upstream Syslock repository.

This repository is the OnePlus 6T / `oneplus-fajita` phone-image fork of
Syslock. It exists specifically for the Arch Linux OnePlus 6T Hyprland phone
image, so a new reader should treat it as the phone-targeted build rather than a
neutral mirror of upstream Syslock.

For the original general-purpose Wayland lock screen, use upstream Syslock:
<https://github.com/System64fumo/syslock>

## What This Fork Is For

Syslock is a simple Wayland lock screen written with `gtkmm4` and
`gtk4-layer-shell`. This fork keeps the upstream design, but carries defaults
and build options for the OnePlus 6T phone image.

In the OnePlus 6T Arch image, Syslock is the touch-first lock screen used by the
Hyprland phone session. It is meant to pair with the phone power-key behavior:
the power key should lock or blank the screen instead of powering the device off.

## Phone-Specific Defaults

The current fork defaults are aimed at the OnePlus 6T display and phone usage:

- primary monitor set to `DSI-1`
- keypad enabled for touch unlock
- password length set to `6`
- runtime config enabled
- tap-to-wake feature compiled in
- event hooks available for lock, unlock, and tap handling

The default config lives in:

```text
config.conf
```

Important defaults:

```text
[main]
start-unlocked=false
keypad=true
password-length=6
main-monitor=DSI-1

[tap-to-wake]
device-path=/dev/input/by-path/SET-ME-UP
verbose=false
timeout=500
```

The tap-to-wake device path is intentionally a placeholder until the phone image
or user install selects the correct input device. Do not ship a random desktop
input path as a OnePlus 6T default.

Installed system copies are placed under:

```text
/usr/local/share/sys64/lock/config.conf
/usr/local/share/sys64/lock/style.css
```

User overrides can be placed under:

```text
~/.config/sys64/lock/config.conf
~/.config/sys64/lock/style.css
```

## Build

Required build/runtime pieces include:

```text
gtkmm-4.0
gtk4-layer-shell
pam
libevdev
pkgconf
make
gcc or clang
```

Build locally with:

```sh
make
```

Install with:

```sh
sudo make install
```

The Makefile installs:

```text
/usr/local/bin/syslock
/usr/local/lib/libsyslock.so
/usr/local/share/sys64/lock/config.conf
/usr/local/share/sys64/lock/style.css
```

## Runtime Usage

The OnePlus 6T image normally starts and controls Syslock through phone UI
helpers, but it can also be launched directly:

```sh
syslock
```

Useful arguments:

```text
-s    Start unlocked
-k    Enable the keypad
-l    Set password length for automatic unlocks
-m    Set primary monitor
-d    Enable debug mode
-v    Print version information
```

Example phone-targeted launch:

```sh
syslock -k -l 6 -m DSI-1
```

## Signals

You can send a signal to show the lock window again:

```sh
pkill -10 syslock
```

## Session Lock

Syslock uses the Wayland session lock protocol:

```text
ext-session-lock-v1
```

Special thanks to `gtk4-layer-shell` for making the layer-shell integration
straightforward.

## Security Notes

This fork is part of an experimental Linux phone image. Treat it as phone-image
infrastructure, not as a security audit claim. Test PAM behavior, keypad
unlocking, power-key handling, and suspend/wake behavior on the target device
before shipping a release image.

## Relationship To Upstream

This fork exists so the OnePlus 6T phone image can carry known-good mobile
defaults without forcing those choices onto upstream Syslock users. Upstream is
still the right place for general-purpose Syslock development.

When syncing from upstream, keep the phone-specific config, keypad, tap-to-wake,
and monitor-default changes visible in review so the public OnePlus 6T image
does not silently regress.

## License

This fork keeps the upstream license. See `LICENSE`.

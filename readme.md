# ZMK Corne Keyboard Layout

This this my personal [zmk](https://github.com/zmkfirmware/zmk) config for my
[corne keyboard](https://github.com/foostan/crkbd), the firmware is built to
work with the following devices.

- Keyboard: Corne 6 column
- Controller: nice!nano v2 + nice!view (for both left and right keyboard)
- Dongle: Seed Xiao nRF52840

> This is a dongle setup with zmk studio support. Left and Right keyboard both
> acts as peripheral and seed xiao as the main controller. This increases the
> battery life of the left board compared to when it is used as both main and
> left peripheral.

## The Keyboard

![typeractive_kb](https://github.com/DarrenVictoriano/zmk-config/blob/master/images/kb.jpeg)

> Nice!view shield is courtesy of
> [M165437's nice-view-gem](https://github.com/M165437/nice-view-gem).

## Keymaps

These are the keymaps and layers defined in this config. The keymaps were
generated using
[Nick Coutsos's Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).

![keymaps](https://github.com/DarrenVictoriano/zmk-config/blob/master/images/corne.svg)

**Macros**

- ⚡️ : hyper key (`ctrl` + `shift` + `alt` + `cmd`)
- ⚙️ : system settings (`cmd` + `shift` + `J`)
- 📷 : `printscreen` (linux) / `F13` (macos)
- 🖥️ : TTY for linux (`ctrl` + `cmd` + `F2`)
- `⌘⌥⎵` : homerow / hints
- `⌃⎵` : tmux leader key

> The image is generated using
> [Cem Aksoylar's Keymap-drawer](https://github.com/caksoylar/keymap-drawer)

## Installing Firmware

Whether you built it yourself or downloaded my prebuilt firmware, it should
contain the following files:

For the Dongle (XIAO):

- xiao_dongle.uf2
- xiao_reset_settings.uf2

For Keyboard (Corne):

- nano_corne_left.uf2
- nano_corne_right.uf2
- nano_reset_settings.uf2

> The `*_reset_settings.uf2` file is used to clear persistent settings like
> default layers, BLE pairings, and other saved data that may remain after
> repeatedly flashing new firmware.

### Steps

1. Plug in your XIAO dongle to your computer.
2. Double-press the small button on the XIAO to enter bootloader mode.
3. Copy `xiao_reset_settings.uf2` to the XIAO's root folder. (Optional, but
   recommended to ensure a clean slate.)
4. You’ll see an error after flashing—this is normal. Unplug and replug the
   XIAO.
5. Double-press the button again to re-enter bootloader mode.
6. Copy `xiao_dongle.uf2` to the root folder of the XIAO.
7. Wait for the error, then unplug the XIAO dongle. Done flashing the dongle for
   now.
8. Plug in your left Corne keyboard (it doesn't need to be turned on).
9. Double-press the side button to enter bootloader mode.
10. Copy `nano_reset_settings.uf2` to the keyboard's root folder.
11. After the error appears, unplug and replug the keyboard.
12. Double-press the button again to go back into bootloader mode.
13. Copy `nano_corne_left.uf2` to the device. (Make sure to use the correct left
    firmware!)
14. Unplug after the error, then repeat steps 8–13 for your right Corne using
    `nano_corne_right.uf2.`
15. Plug your XIAO dongle back in to your computer.
16. Turn on both sides of your keyboard. Enjoy!

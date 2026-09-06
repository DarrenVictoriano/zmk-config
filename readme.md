# ZMK Corne Keyboard Layout

This this my personal [zmk](https://github.com/zmkfirmware/zmk) config for my
[corne keyboard](https://github.com/foostan/crkbd), the firmware is built to
work with the following devices.

- Keyboard: Corne 6 column
- Controller: nice!nano v2 + nice!view (for both left and right keyboard)
- Dongle: Seeed XIAO nRF52840 or Raytac MDBT50Q-CX-40 Nordic nRF52840

> This is a dongle setup with zmk studio support. Left and Right keyboard both
> act as peripherals and the selected dongle is the main controller. This
> increases the battery life of the left board compared to when it is used as
> both main and left peripheral.

## The Keyboard

![typeractive_kb](https://github.com/DarrenVictoriano/zmk-config/blob/master/images/kb.jpeg)

> Nice!view shield is courtesy of
> [M165437's nice-view-gem](https://github.com/M165437/nice-view-gem).

## Keymaps

These are the keymaps and layers defined in this config. The keymaps were
generated using
[Nick Coutsos's Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).

![keymaps](https://github.com/DarrenVictoriano/zmk-config/blob/master/images/corne.svg)

**Keymap Legend**

- `⚡` : Hyper (`Ctrl` + `Shift` + `Alt` + `Cmd`)
- `▾` : Meh (`Ctrl` + `Shift` + `Alt`)
- Enter/Ctrl key: tap for Enter, hold for Right Ctrl (balanced, 250 ms tapping
  term)
- `⌘⌥↵` : `Cmd` + `Option` + `Enter`
- `⌘⌥⎵` : Homerow
- `⌘⇧` : `Cmd` + `Shift`
- `⌃⎵` : tmux leader key
- `⌃⌥` : `Ctrl` + `Option`
- `✦⎋` : `Ctrl` + `Shift` + `Esc`
- Camera icon: Print Screen (Linux) / `F13` (macOS)
- Mouse wheel, left-click, and right-click icons: middle, left, and right mouse
  buttons
- Adjust-layer circle arrows: mouse movement
- Adjust-layer pan arrows: scrolling

> The image is generated using
> [Cem Aksoylar's Keymap-drawer](https://github.com/caksoylar/keymap-drawer)

## Installing Firmware

Whether you built it yourself or downloaded my prebuilt firmware, it should
contain the following files:

For the XIAO dongle:

- xiao_dongle.uf2
- xiao_reset_settings.uf2

For the Raytac dongle:

- raytac_dongle.hex
- raytac_reset_settings.hex

For Keyboard (Corne):

- nano_corne_left.uf2
- nano_corne_right.uf2
- nano_reset_settings.uf2

> The `*_reset_settings` files clear persistent settings like default layers,
> BLE pairings, and other saved data that may remain after repeatedly flashing
> new firmware.

### Dongle warning

Flashing both dongles does not make them interchangeable.

The left and right firmware files contain no predefined dongle identity. During
first startup, each half bonds to the specific central dongle it discovers. That
hardware address and bond are saved in persistent settings.

If you initially power all four devices together:

```text
XIAO powered + Raytac powered + left + right
```

Pairing can be unpredictable. One half could even bond with a different dongle
than the other.

Use one of these arrangements:

```text
XIAO powered + Raytac unplugged + both halves
```

or:

```text
Raytac powered + XIAO unplugged + both halves
```

After pairing, only that selected dongle will work with those halves. Plugging
in the other dongle will not take over.

To switch dongles later, you must:

1. Flash settings-reset firmware to both halves and the desired dongle.
2. Flash their normal firmware again.
3. Keep the other dongle unplugged while they pair.

Building both dongle images in one repository only means you have firmware
available for either hardware model. It does not add multi-dongle or failover
support. For two complete keyboards, however, you can use XIAO with one set of
halves and Raytac with the other set.

### XIAO Dongle

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

### Raytac Dongle

The Raytac MDBT50Q-CX-40 uses its factory Nordic USB DFU bootloader instead of a
UF2 drive. Do not erase or replace this bootloader. Install
[Nordic nRF Util](https://www.nordicsemi.com/Products/Development-tools/nRF-Util)
and its nRF5 SDK tools command before flashing:

```sh
nrfutil install nrf5sdk-tools
```

Package the settings-reset firmware:

```sh
nrfutil nrf5sdk-tools pkg generate \
  --hw-version 52 \
  --sd-req=0x00 \
  --application raytac_reset_settings.hex \
  --application-version 1 \
  raytac_reset_settings.zip
```

With the Raytac unplugged, hold its side button inward toward the USB connector,
plug it in, and keep holding the button for about one second. Release it when
the LED starts blinking or fading, then flash the package:

```sh
nrfutil nrf5sdk-tools dfu usb-serial \
  -pkg raytac_reset_settings.zip \
  -p /dev/ttyACM0
```

Package the normal firmware:

```sh
nrfutil nrf5sdk-tools pkg generate \
  --hw-version 52 \
  --sd-req=0x00 \
  --application raytac_dongle.hex \
  --application-version 1 \
  raytac_dongle.zip
```

Put the Raytac into DFU mode again and flash the normal firmware:

```sh
nrfutil nrf5sdk-tools dfu usb-serial \
  -pkg raytac_dongle.zip \
  -p /dev/ttyACM0
```

The serial port may differ from `/dev/ttyACM0`. The Nordic nRF Connect for
Desktop Programmer can also load the HEX files using its **Open DFU Bootloader**
option.

# ZMK Studio / Sleep Fixes

This snapshot is patched for the attached TOTEM dongle configuration.

## Firmware changes

- Pins ZMK to v0.3.0 for a stable Zephyr/ZMK baseline.
- Enables ZMK Studio locking on the dongle.
- Adds `&studio_unlock` to the far-right key on the Button layer.
- Disables the XIAO BLE board's default CDC-ACM console on the dongle, leaving the Studio RPC CDC-ACM endpoint as the only USB serial device.
- Uses the upstream mock kscan approach for the dongle and removes the dongle's physical-layout kscan links.
- Keeps the dongle awake over USB while leaving normal deep sleep enabled on both battery-powered halves.
- Leaves the existing matrix kscan `wakeup-source` setting in place for the halves.

## Studio unlock

The Button layer is held with the SLASH key on the Base layer.

Press: **hold SLASH, tap P, release SLASH**

After a reconnect, the Studio lock normally needs to be unlocked again.

## First flash

Because this is a dongle configuration change, reset settings on all three devices before flashing the patched firmware. This clears old BLE bonds, so pair the keyboard again afterward.

1. Flash `settings_reset` to the dongle.
2. Flash `settings_reset` to the left half.
3. Flash `settings_reset` to the right half.
4. Flash the patched dongle firmware.
5. Flash the patched left firmware.
6. Flash the patched right firmware.

Then connect the dongle by USB and open ZMK Studio. The dongle should expose one Studio serial port.

## Windows access denied

If Windows still reports `access is denied` on the remaining COM port, close any serial terminal/monitor or flashing utility that has the port open, unplug/replug the dongle, and reopen ZMK Studio.

This archive contains configuration changes only. The firmware itself should be rebuilt by the repository's normal GitHub Actions workflow.

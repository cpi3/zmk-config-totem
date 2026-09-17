# ZMK Studio / Totem dongle fixes

## Dongle USB / COM ports

The Seeed XIAO BLE board exposes a normal CDC-ACM UART at `&usb_cdc_acm_uart`.
ZMK Studio's `studio-rpc-usb-uart` snippet adds a second CDC-ACM UART for the
Studio RPC protocol. The dongle disables the normal board CDC node and keeps
the Studio RPC node, so Windows should enumerate one Studio serial port.

The dongle overlay is now part of the local Totem shield definition, so it is
applied after `totem.dtsi` defines `kscan0`. This avoids the previous
`undefined node label 'kscan0'` devicetree failure.

## Dongle kscan

The dongle has no keys, so it disables the physical Totem `kscan0` and selects
`zmk,kscan = &mock_kscan`. The physical layout no longer hard-codes a kscan,
so it falls back to the chosen mock scanner as recommended by ZMK's dongle
guidance.

## Sleep

Deep sleep is disabled on both split halves. ZMK issue #2904 documents a
split-central hang when a peripheral goes to sleep, with disabling peripheral
sleep reported as the working workaround. The USB dongle is also configured
without deep sleep because it is continuously powered.

The halves still enter normal ZMK idle state, but they stay BLE-connected so
key presses do not depend on deep-sleep wake/reconnect behavior.

## Studio unlock

`&studio_unlock` is on the far-right key of the Button layer. On Base that
physical key is the `/` key (`&lt 7 SLASH`).

Press:

1. Hold `/`
2. While holding `/`, press `P`
3. Release `/`

That invokes `&studio_unlock`.

## ZMK version

The manifest is pinned to ZMK `v0.3.0`, whose release notes include the Studio+USB
build fix and which keeps this configuration on the known-stable Zephyr 3.5 era.

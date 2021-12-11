# XPS9700_HACK

My main goal for this "guide" is to provide a smooth installation experience with no indefinite steps that'll be hard to follow, which is why I've documented everything to a pretty high extent. You *can* skip through and probably be just fine, but I'd still advise you to take your time, this is not an easy task after all...

This repo has been built from the ground up, by doing a lot of research and running several experiments, which means that it's not a mutant made from many different machines, as others sadly are. There are no questionable files or settings and everything has been well thought out, optimizing for efficiency and quick boot times. If you want to contribute, please stick to this mentality.

## Table of Contents

TODO

## Hardware

As far as I know, all XPS 9700 4K machines will ship with the exact same hardware, other than RAM and SSD. RAM is pretty much irrelevant in our case, but if you got an *SK Hynix* as your SSD, it needs to go. Actually, I've kept mine, and installed a compatible drive in the second available M.2 slot. This is efficient, but comes with one small caveat: The Hynix slot **needs** to be "disabled" with a custom SSDT in order for macOS not to loose it's mind on boot. More on that later!

(insert image here)

After adding custom bought parts, this is what I'm running on (check against yours, or your mileage may vary):

| Part | Description | Working on macOS | Comment |
| ---- | ----------- | ---------------- | -------- |
| **Working** |
| Intel Core i7-10875H | 8C/16T CometLake Processor, UHD-630 | yes, fully | / |
| Display | Dell XPS 17 SHP14D6 4K 16:10 | yes, fully | / |
| Kingston Fury DDR4-2933 2x16GB | 32GB of CL17-19-19 | yes, fully | 2933MHz is max |
| WD Black SN750 1TB | M.2 Gen3 SSD, Slot B | yes, fully | / |
| Killer AX1650s | WiFi 6 & BT 5 Card | yes, pretty good | soldered in |
| Touchpad | DELL098F:0004F3:311C, I2C | yes, fully | all gestures operational |
| Keyboard | Standard PS2 | yes, fully | function keys are mapped |
| Touch-Digitizer | ELAN2097:0004F3:2A15, I2C | yes, fully | all native gestures operational |
| Webcam | Micromedia 0c45:6d14, 0.9MP | yes, fully | / |
| Battery | DELL 01RR3YM Li-poly 95.1Wh | yes, fully | / |
| **Untested** |
| Gigabit Ethernet | Realtek RTL8153 | not yet tested | / |
| SD-Card reader | Realtek RTS5260 | not yet tested | / |
| Thunderbolt 3 | Intel JHL7540, JHL6540 | not yet tested | / |
| Sensors | Intel 8086:06fc:1028:098f 400 Series Sensor Hub | not yet tested | / |
| **Not Working** |
| Speakers & Jack | Codec currently unknown | no | / |
| SK Hynix PC611 1TB | M.2 Gen3 SSD, Slot A | no, never will | deactivated using SSDT |
| Nvidia RTX 2060 Max-Q | 6GB DDR6 VRAM | no, never will | deactivated using SSDT |
| Fingerprint reader | Shenzhen Goodix 27c6:533c | no, never will | / |

## BIOS

I am on the currently (date: 11th Dec, 2021) latest available BIOS, which I installed using windows updates. It then flashed the firmware onto my device on a reboot. The exact version is: 1.11.1 11/18/2021. In order to have no complications, make sure you change these BIOS settings:

| Menu | Setting | State |
| ---- | ------- | ----- |
| Boot Configuration | Enable Secure Boot | Off |
| Integrated Devices | Thunderbolt Security Level | No Security |
| Storage | Sata Operation | AHCI |
| Power | Enable Lid Switch | On |
| Security | Intel Software Guard Extensions | Software Control |

## Deactivating unsupported NVMe

If you also want to use an unsupported NVMe drive in the second slot, you need to disable it for macOS. When looking at the opened up machine having the battery at the bottom (and thus the labels on it readable), the *left* slot is at `\_SB.PCI0.RP21.PXSX` and the *right* slot is at `\_SB.PCI0.RP17.PXSX`. You can always check your paths in Windows' Device Manger, at Storage controllers > NVM Express Controller > Details > BIOS device name. Then, just adapt the paths in `SSDT-NVMe-SLT.aml` under `ACPI`, and enable it in `config.plist`.
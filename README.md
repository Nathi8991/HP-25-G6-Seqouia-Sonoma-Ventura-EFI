# HP Laptop 250 G6 — OpenCore EFI (Ventura · Sonoma · Sequoia · Tahoe)

> **Quick note:** This EFI is built and tested on the **HP Laptop 250 G6**. It might be useful as a starting point for other Kaby Lake laptops, but I can't tell you if it'll work on different hardware or not.

>**SYSTEM WILL REFUSE TO BOOT**: Please read the whole guide to actually get a functional EFI that will work properly on this hardware

---

So this is my OpenCore EFI for running macOS Ventura, Sonoma, Sequoia and Tahoe on the HP 250 G6. Getting it to a daily-driver state took a while, especially the Wi-Fi situation, which is its own thing entirely. I Hope this saves someone a few hours of struggling.

If you're also trying to get the **Intel Wireless-AC 7260** working on a modern macOS version, the setup notes below should be pretty useful even if your hardware isn't identical.

---

## System Specifications

| Component | Details |
|---|---|
| **Model** | HP Laptop 250 G6 |
| **CPU** | Intel Core i5-7200U (Kaby Lake, 7th Gen) |
| **RAM** | 12GB DDR4 (came with 8GB, supports up to 16GB) |
| **Graphics** | Intel HD Graphics 620 |
| **Wi-Fi / Bluetooth** | Intel Wireless-AC 7260 |
| **Bootloader** | OpenCore |

---

## What Works

- [x] Full UEFI boot, NVMe/SATA storage works fine
- [x] Intel HD 620 graphics acceleration (spoofed device-id)
- [x] Intel Wi-Fi and Bluetooth (needs post-install patching, covered below)
- [x] iCloud, iMessage, FaceTime — all working once SMBIOS is set up properly
- [x] macOS Versions — all above Ventura!
- [x] Full macOS Tahoe Support oncoming!

---

## Before You Boot — Set Up Your SMBIOS First

Please do this before anything else. The `config.plist` in this repo has been wiped of all serial numbers and platform identifiers — **it will not boot out of the box**, and that's intentional. I'm not going to share my serials, and you shouldn't be using someone else's anyway to begin with XD.

Please follow the following steps to properly to successfully boot into the recovery system:

1. Open `EFI/OC/config.plist` in **ProperTree**.
2. Go to `PlatformInfo → Generic`.
3. Use **GenSMBIOS** to generate values for a **MacBookPro15,1** profile (For anything before sequoia), and **MacBookPro16,1** for Tahoe.
4. Replace the `REPLACE_WITH_YOUR_...` placeholders with your generated **MLB**, **SystemSerialNumber**, and **SystemUUID**.
5. Set the `ROM` field to `Data` type, or that of a similar looking number.
6. Save the file.
   
iServices WILL FAIL if not done correctly

---

## Post-Install: Wi-Fi and Bluetooth

Sequoia dropped native support for older Intel wireless chips, so the wifi card will literally block wifi from ever being turned on until you patch it.

**The Following is the the steps in a simpler version based on the guide from 5T33Z0. His guide is more comprehensive, and I will also post another README.md file specifically for this wifi issue**

**Root Patch with OCLP-Mod**

1. Boot into the desktop with this EFI (Wi-Fi won't work yet, use Ethernet).
2. Download and open a custom **OCLP-Mod** by laobamac — it's a community fork of OpenCore Legacy Patcher maintained by him to fix Intel Wifi Card issues for Seqouia and Tahoe
3. Hit **Post-Install Root Patch** and let it run. It injects the legacy kexts back into the system volume.
4. Reboot.

Wi-Fi and Bluetooth should show up normally in System Settings.

**Speed Issues**

If your connection keeps dropping or throttling down to almost nothing during heavy activity, this is a known limitation with these legacy drivers for Sequoia and above. The AC 7260 doesn't work natively. An alternative like **Heliport** may give you better results, but completely removes native functionality, so iServices would struggle without it (Like Find My, weather, and that requiring native functionality. Other stuff like iMessage and Facetime will work regardless).

A few other things worth knowing:

- **Disable Bluetooth during initial iCloud sync.** The AC 7260 shares its antenna between Wi-Fi and BT, and when both are hammering it at once the card starts to slow down. Turn BT off and let iCloud finish its first sync, then turn it back on.
- **Stick to 2.4GHz if you can.**
- **Use Ethernet if it's an option.** The onboard NIC works natively and is genuinely the fastest you'll get on this machine.
---

## Legal

No Apple software or macOS images are included here. Grab a legitimate copy of macOS directly from Apple, or from dortania's OpenCore Tutorial Page. This repo is purely a personal project shared for reference.

---

## From Here Onwards

I am currently looking into the EFI for Big sur, monterey, and even possibly catalina if it is what you desire! Stay tuned y'all!

---

## Credits

Big thanks to the people who actually built the tools that make this possible:

- [**Acidanthera**](https://github.com/acidanthera) — OpenCore, Lilu, VirtualSMC, WhateverGreen. The foundation
- [**OCLP4HACKINTOSH (Seqouia and Tahoe Wifi Fix)**](https://github.com/5T33Z0/OCLP4Hackintosh/blob/main/Enable_Features/AirportItllwm_Sequoia.md) — Dedicated guide for Airportitlwm.kext fix on Seqouia and above
- [**OpenIntelWireless**](https://github.com/OpenIntelWireless/itlwm) — `AirportItlwm`, keeping Intel Wi-Fi cards alive.
- [**OpenIntelWireless**](https://github.com/OpenIntelWireless/HeliPort) — HeliPort, when you need a Wi-Fi menu bar that actually works (NOT NATIVE).
- [**OCLP-Mod Contributors**](https://github.com/laobamac/OCLP-Mod) — For keeping the patch framework going for Intel Wifi Card hardware.

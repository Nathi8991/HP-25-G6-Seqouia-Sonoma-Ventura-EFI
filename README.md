# HP-25-G6-Seqouia-Sonoma-Ventura-EFI
EFI folders for the HP Laptop 250 G6, as well as other Kaby Lake (7th Gen Intel CPU's) EFI
<hr> 
<b>THIS REPOSITORY IS DIRECTED TOWARDS THE HP LAPTOP 250 G6, I HAVE NO CERTAIN IDEA WHETHER THIS WILL WORK WITH OTHER LAPTOP MANUFACTURERS</b>  

This repository serves as an idea for booting into Sequoia, Sonoma, and Ventura on this specific hardware profile, as well as a reference framework for anyone trying to stabilize an **Intel Wireless-AC 7260** card on downgraded legacy wireless drivers.

## System Specifications
- **Laptop Model:** HP Laptop 250 G6
- **CPU:** Intel Core i5-7200U (Kaby Lake)
- **RAM:** 12GB DDR4
- **Graphics:** Intel HD Graphics 620
- **Wi-Fi/Bluetooth:** Intel Wireless-AC 7260
- **Bootloader:** OpenCore

---

## What Works
- [x] Full UEFI Booting & Native Storage Management
- [x] Intel HD 620 Graphics Acceleration (Spoofed)
- [x] Intel Wi-Fi & Bluetooth (By Specialized root-patch configurations)
- [x] iCloud Syncing, iMessage, and FaceTime Authentication

---

## Post-Install Setup for Intel Wi-Fi & iServices

To reduce your chances of having sysyem instability, connection drops, and service failures, you **must** implement the following post-install configuration:

### 1. Re-apply Post-Install Root Patches
Because macOS Sequoia dropped native support for older legacy wireless chipsets, you must use the specialized community modification tool:
1. Boot into the desktop using this EFI.
2. Open **OCLP-Mod** (the custom Intel-supported fork of OpenCore Legacy Patcher).
3. Select **Post-Install Root Patch** and let the system inject the necessary kernel extensions into the system volume.
4. Reboot your machine.

### 2. Fix Speed Drops & Connection Jitters
To stabilize the translated network drivers and stop the card from choking down to kilobytes during heavy indexing:
- Open your configuration and ensure the **`-novht`** flag is appended to your `boot-args` string under `NVRAM -> Add -> 7C436110-...`. This disables High-Throughput profiles, locking the card into a highly stable Wireless-N state. I have already done this for you guys, but a double-check is always recommended.
- Unfortunately, Native WiFi For heavy transfers (like initial iCloud indexing), temporarily disable Bluetooth to prevent antenna packet collisions, and isolate your router's 5GHz band if possible. Use Ethernet, since that port has the fastest speeds you could get on this machine!

---

## Crucial Requirement Before Booting (SMBIOS Data)

Because of privacy and safety, the `config.plist` in this repository has been fully clean of any serial or any trace of previous use. **It will not boot out of the box.** 

Before copying this EFI to your drive:
1. Open `EFI/OC/config.plist` using ProperTree.
2. Navigate to `PlatformInfo -> Generic`.
3. Use **GenSMBIOS** to generate unique system values for a **MacBookPro14,1** profile.
4. Replace the placeholders (`REPLACE_WITH_YOUR_...`) with your unique **MLB**, **SystemSerialNumber**, and **SystemUUID** values. 
5. Set the `ROM` type to `Data` and populate it with your laptop's true physical MAC address (or 12 zeros to reset it).
6. Save the file.

---

## Legal Notice
This repository does **NOT** contain proprietary Apple software binaries or installer images. Users must fetch an official copy of macOS Sequoia directly via Apple's software distribution network.

## Credits & Acknowledgments
A massive thanks to the brilliant developers across the Hackintosh landscape whose open-source tools made this possible:
- [Acidanthera](https://github.com/acidanthera) - For the OpenCore bootloader and foundational system kexts (`Lilu`, `VirtualSMC`, `WhateverGreen`).
- [OpenIntelWireless](https://github.com/OpenIntelWireless/itlwm) - For the development of `AirportItlwm`, keeping legacy wireless hardware alive.
- Again, [OpenIntelWireless](https://github.com/OpenIntelWireless/Heliport) - For the Heliport Option, enabling WiFi (Non-Native)
- [**OCLP-Mod Contributors**](https://github.com/laobamac/OCLP-Mod/releases). - For creating the critical patch frameworks required to bypass modern macOS networking lockdowns.

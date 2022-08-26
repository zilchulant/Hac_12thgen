# Hackintosh with i5-12400F and Gigabyte B660m-DS3H

This is my Creation of a bootable EFI with OpenCore on Intel Alder Lake with a Gigabyte B660m-DS3H Mainboard and an Intel i5-12400F. I did not install MacOS from an USB-Stick as I already had a SSD with MacOS installed. But it was possible to boot the MacOS Installer Stick without any issues.

- [Hackintosh with i5-12400F and Gigabyte B660m-DS3H](#hackintosh-with-i5-12400f-and-gigabyte-b660m-ds3h)
- [1. Specs](#1-specs)
- [2. BIOS Settings](#2-bios-settings)
- [3. Checkliste](#3-checkliste)
- [3. Problems](#3-problems)
- [4. OpenCore](#4-opencore)
  - [4.1 SMBIOS](#41-smbios)
  - [4.2 Necessary Kexts](#42-necessary-kexts)
  - [4.3 SSDTs](#43-ssdts)
  - [4.4 ACPI Hotpatches](#44-acpi-hotpatches)
  - [4.5 USB](#45-usb)
  - [4.6 CPUFriend](#46-cpufriend)


<br>

# 1. Specs

|Part|Brand|Model|Notes|
|:--|:--|:--|:--|
|Motherboard|Gigabyte|B660m-DS3H DDR4|BIOS Version R5|
|CPU|Intel|i5-12400F||
|GPU|Sapphire|AMD Radeon RX Vega 56||
|RAM|G.Skill|Aegis 2x 8GB DDR4 2666MHz||
|Wifi|Broadcom|BCM943602cs|- MacOS: Works out of box <br> - Windows: Needs drivers <br> - Linux: Works out of box|
|Bluetooth|Broadcom|BCM943602cs|- MacOS: Works out of box <br> - Windows: Needs drivers <br> - Linux: Works out of box|
|Drive|Western Digital|WD SN750 1TB||
|Power Supply|BeQuiet|Straight Power 11 650W 80+ Gold||
|CPU Cooler|Intel|Stock|for now!

---

<br>

# 2. BIOS Settings

---

<br>

# 3. Checkliste
- :white_check_mark: CPU Stepping
- :white_check_mark: CPU Sleep / Wakeup
- :white_check_mark: GPU Acceleration
- :white_check_mark: GPU Connection DP
- :white_check_mark: GPU Connection HDMI
- :white_check_mark: ALC1220 Audio Out
- :white_check_mark: ALC1220 Audio In
- :white_check_mark: USB Ports
- :white_check_mark: SSD NVMe
- :white_check_mark: All Sensors
- :white_check_mark: NVRAM (Native)

---

<br>

# 3. Problems

So far no problems.

---

<br>

# 4. OpenCore

## 4.1 SMBIOS

I chose MacPro7,1 as SMBIOS, you might consider iMacPro1,1 as well. Head over to Dortania for more information.

<br>

## 4.2 Necessary Kexts

I wrote an explenation if my choise differed from Dortania.

|Kext|Reason|
|:--|:--|
|Lilu||
|LiluFriend||
|AppleALC||
|CPUFriend||
|CPUFriendDataProvider||
|LucyRTL8125Ethernet||
|NVMeFix||
|RestrictEvents||
|VirtualSMC||
|SMCProcessor||
|SMCSuperIO||
|USBToolBox||
|USBMap|Self generated USB Table|
|VegaTab56|Self generated PowerTable for my GPU|
|Whatevergreen||

<br>

## 4.3 SSDTs

My list of SSDTs is the bare minimum to get MacOS working.

|SSDT|Reason|
|:--|:--|
|SSDT-AWAC|RTC-AWAC Clock issue is present in DSDT|
|SSDT-EC||
|SSDT-HPET|created with SSDTTime|
|SSDT-Plug-ALT|There are different versions, this one works|
|SSDT-RHUB||
|SSDT-USBX||

<br>

## 4.4 ACPI Hotpatches

|Patch|Reason|Find|OemTableId|Replace|TableSignature|
|:--|:--|:--|:--|:--|:--|
|Change ADBG to XDBG||43031419 41444247 |47535741 7070|43031419 58444247|53534454|
|HPET _CRS to XCRS Rename|created with SSDTTime|255F4352 53|00000000|25584352 53|00000000|

<br>

## 4.5 USB

You probably should create your own USB Map which fits your needs. This USBMap is created using the USBToolBox on Windows.

|Number|Port USB 2.0|Port USB 3.0|Port Type|Status|Comment|
|:--|:--|:--|:--|:--|:--|
|1|6|-|USB 2 - 0|active||
|2|5|-|USB 2 - 0|disabled||
|3+14|1|17|USB - C (Switch) - 9|active||
|5|2|18|USB 3 - 3|active||
|6|3|19|USB 3 - 3|active||
|7|4|20|USB 3 - 3|active||
|F_USB1|7|-|USB 2 - Internal|active|Bluetooth Card!|
|F_USB2|7|-||disabled||
|FU32-a|10|25|USB 3 - 3|active|Front USB|
|FU32-b|9|26|USB 3 - 3|active|Front USB|
|?|11|-|USB2 - 255|active|Internal "ITE Device Operating at USB1.1"|

## 4.6 CPUFriend

**Warning** this Kext is especially created for the i5-12400F with CPU Vectors to fit the iMacPro7,1 SMBIOS!
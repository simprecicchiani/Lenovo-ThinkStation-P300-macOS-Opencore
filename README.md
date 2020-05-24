# ThinkStation P300 macOS Catalina (OpenCore bootloader)

<img src="/Images/p300.png" alt="ThinkStation P300" height="500">

## Introduction

### General knowledge & credits

* [Why OpenCore](https://dortania.github.io/OpenCore-Desktop-Guide/#advantages-of-opencore)

- To install macOS follow the guides provided by [Dortania](https://dortania.github.io)

- Lots of SSDT patches from [OC-little](https://translate.google.it/translate?sl=zh-CN&tl=en&u=https%3A%2F%2Fgithub.com%2Fdaliansky%2FOC-little)

- Useful tools by [CorpNewt](https://github.com/corpnewt)

- [Acidanthera](https://github.com/acidanthera) that make this possible


### My Hardware

* Model: ThinkStation P300 Tower - Type 30AH
* Processor: Intel Core i5-4690 (4C, 2.5 / 3.9GHz, 6MB)
* Graphics: Integrated Intel HD 4600
* Memory: 2x4GB DDR3L 1600MHz
* Sound Card: Realtek ALC662
* Storage: 500GB HDD
* WLAN + Bluetooth: Fenvi FV-HB1200


## What if I don't have this exact model?


This EFI will suit any P300 regardless of CPU model[^1] / RAM amount / Storage drive (HDD or SSD[^2]).

[^1]: custom power management SSDT needed

[^2]: Some NVMe drives may not work OOTB with MacOS, do your own researches

## Recommended changes

### USB ports map

USBMap.kext is used to map needed ports. If you need a different configuration follow [USBMap guide](https://github.com/corpnewt/USBMap)

### CPU Power Management
If you happen to have a different CPU model **remove CPUFriend.kext and replace SSDT-CPUD with plain SSDT-PLUG**, power management is natively supported by OpenCore anyway. If you want to take a step forward and create a custom profile, follow [CPUFriendFriend guide](https://github.com/corpnewt/CPUFriendFriend)

## Bios settings (to do)

* `Security` > `Security Chip` > **Disable**
* `Security` > `Virtualization` > `Intel Virtualization Technology` > **Enable**
* `Security` > `Virtualization` > `Intel VT-d Feature` > **Enable**
* `Security` > `Anti-Theft` > `Computrace` > `Current Setting` > **Disable**
* `Security` > `Secure Boot` > **Disable**
* `Security` > `Intel SGX` > **Disable**
* `Startup` > `UEFI/Legacy Boot` > **UEFI Only**
* `Startup` > `CSM Support` > **No**
* `Startup` > `Boot Mode` > **Quick**

## What's working ✔️

- [x] CPU Power Management

- [x] Intel HD 4600 Graphics `incuding graphics acceleration`

- [x] USB ports `with custom kext or SSDT`

- [x] Sleep / Wake / Shutdown / Reboot

- [x] Intel Gigabit Ethernet

- [x] **Wifi, Bluetooth, Airdrop, Handoff, Continuity, Sidecar wireless**

- [x] iMessage, FaceTime, App Store, iTunes Store `Generate your own SMBIOS`

- [x] DRM support `iTunes Movies, Apple TV+, Amazon Prime, Netflix and others`

- [x] Headphones and microphone jack

- [x] SIP and FileVault 2 can be enabled

- [x] DisplayPorts `with digital audio passthrough`

- [x] SD Card Reader


## What's not working ⚠️

Nothing to mention so far

## Update tracker 🔄

- [x] safe to install macOS Catalina‌ 10.15.4 supplemental update


| Item | Version |
| :--- | ---: |
| MacOS | 10.15.4 |
| OpenCore | 0.5.8 |
| Lilu | 1.4.4 |
| VirtualSMC | 1.1.3 |
| WhateverGreen | 1.3.9 |
| AppleALC | 1.4.9 |
| IntelMausi | 1.0.2 |


## If you found my work useful please consider a PayPal donation

<a href="https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Y5BE5HYACDERG&source=url" target="_blank"><img src="/Images/buymeacoffee.png" alt="Buy Me A Coffee" width="300" ></a>

## Thanks to

All the hackintosh community, especially the guys on GitHub.

===
### How much this desktop costs

* ThinkStation P300 configured as above: €89 (+€15 shipping)
<img src="/Images/purchase.png" alt="ThinkStation P300" height="200">

* Pcie WLAN: €26
<img src="/Images/fenvi.png" alt="ThinkStation P300" height="200">

_**total €130**, making it probably the cheapest iMac ever_


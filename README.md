# Lenovo ThinkStation P300 running macOS (OpenCore bootloader)

<img align="right" src="/Images/p300.png" alt="Lenovo ThinkStation P300 macOS" width="300">

[![macOS](https://img.shields.io/badge/macOS-Catalina_10.15.4-blue.svg)](https://support.apple.com/en-us/HT210642)
[![OpenCore](https://img.shields.io/badge/OpenCore-0.5.8-green)](https://github.com/acidanthera/OpenCorePkg)
[![MODEL](https://img.shields.io/badge/Model-30AH000RUS-lightgrey)](https://psref.lenovo.com/Product/ThinkStation/ThinkStation_P300_Tower)
[![LICENSE](https://img.shields.io/badge/license-MIT-purple)](/LICENSE)

**DISCLAIMER:**
Read the entire README before you start. I am not responsible for any damages you may cause.

Should you find an error, or improve anything, be it in the config itself or in the my documentation, please consider opening an issue or a pull request to contribute.

Lastly, if you found my work useful please consider a PayPal donation, it would mean a lot to me.

[![donate](https://img.shields.io/badge/-buy%20me%20a%20coffee-orange)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Y5BE5HYACDERG&source=url)

## Introduction

<details>  
<summary><strong>General knowledge & credits</strong></summary>

- [Why OpenCore](https://dortania.github.io/OpenCore-Install-Guide/why-oc.html)

- [Dortania's website](https://dortania.github.io)

- [SSDT patches from OC-little](https://translate.google.it/translate?sl=zh-CN&tl=en&u=https%3A%2F%2Fgithub.com%2Fdaliansky%2FOC-little)

- Useful tools by [@CorpNewt](https://github.com/corpnewt)

- [Acidanthera's OpenCore and kexts development](https://github.com/acidanthera)

</details>

<details>  
<summary><strong>My Hardware</strong></summary>

| Product             | P300 Tower                         |
|:--------------------|:-----------------------------------|
| Model               | 30AH000RUS                         |
| Region              | US                                 |
| Machine Type        | 30AH                               |
| Processor           | Core i5-4690 4C/3.5GHz/6MB/1600MHz |
| Graphics            | Integrated Intel HD 4600           |
| Memory              | 2x4GB 1600MHz non-ECC              |
| Discrete graphics   | Open                               |
| WLAN + Bluetooth    | Fenvi FV-HB1200                    |
| Sound Card          | Realtek ALC662                     |
| Internal disk drive | 1x1TB 3.5" SATA6Gbs 7.2K           |
| Optical             | DVD±RW                             |
| Media Reader        | Yes                                |
| Power supply        | 280W 85%                           |

</details>

<details>  
<summary><strong>Hardware compatibility</strong></summary>

This EFI will suit any P300 regardless of CPU model<sup>[1](#CPU)</sup> / RAM amount / Storage drive (SATA or NVMe<sup>[2](#NVMe)</sup>).

<a name="CPU">1</a>: Follow CPU Power Management guide

<a name="NVMe">2</a>: Some NVMe drives may not work OOTB with MacOS, [NVMeFix](https://github.com/acidanthera/NVMeFix) could resolve some issues.

</details>

## Installation

<details>  
<summary><strong>How to install macOS</strong></summary>

- Download [EFI folder](/EFI/)
- Follow [Dortania's guide](https://dortania.github.io/OpenCore-Install-Guide/installation/installation-process.html)

</details>

<details>  
<summary><strong>BIOS Settings</strong></summary>

* `Security` > `Security Chip` > **Disable**
* `Security` > `Virtualization` > `Intel Virtualization Technology` > **Enable**
* `Security` > `Virtualization` > `Intel VT-d Feature` > **Enable**
* `Security` > `Anti-Theft` > `Computrace` > `Current Setting` > **Disable**
* `Security` > `Secure Boot` > **Disable**
* `Security` > `Intel SGX` > **Disable**
* `Startup` > `UEFI/Legacy Boot` > **UEFI Only**
* `Startup` > `CSM Support` > **No**
* `Startup` > `Boot Mode` > **Quick**

</details>

## Post-install

<details>  
<summary><strong>USB ports mapping</strong></summary>

USBMap.kext is used to map needed ports. If you need a different configuration follow [USBMap guide](https://github.com/corpnewt/USBMap)

</details>

<details>  
<summary><strong>Custom CPU Power Management</strong></summary>

If you happen to have a different CPU model **remove CPUFriend.kext and replace SSDT-CPUD with plain SSDT-PLUG**, power management is natively supported by OpenCore anyway. If you want to take a step forward and create a custom profile, follow these steps:

- Run the following command in Terminal:

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/stevezhengshiqi/one-key-cpufriend/master/one-key-cpufriend.sh)"
```

- Copy `CPUFriend.kext` and `CPUFriendDataProvider.kext` from desktop to `/OC/Kexts/`.

- Open `/OC/config.plist` and add the following code:

```xml
<dict>
    <key>BundlePath</key>
    <string>CPUFriend.kext</string>
    <key>Comment</key>
    <string>Power management data injector</string>
    <key>Enabled</key>
    <true/>
    <key>ExecutablePath</key>
    <string>Contents/MacOS/CPUFriend</string>
    <key>MaxKernel</key>
    <string></string>
    <key>MinKernel</key>
    <string></string>
    <key>PlistPath</key>
    <string>Contents/Info.plist</string>
</dict>
<dict>
    <key>BundlePath</key>
    <string>CPUFriendDataProvider.kext</string>
    <key>Comment</key>
    <string>Power management data</string>
    <key>Enabled</key>
    <true/>
    <key>ExecutablePath</key>
    <string></string>
    <key>MaxKernel</key>
    <string></string>
    <key>MinKernel</key>
    <string></string>
    <key>PlistPath</key>
    <string>Contents/Info.plist</string>
</dict>
```

</details>

<details>  
<summary><strong>Enable Apple Services</strong></summary>

- Do the following one line at a time in Terminal:

```bash
$ git clone https://github.com/corpnewt/GenSMBIOS
$ cd GenSMBIOS
$ chmod +x GenSMBIOS.command
```

- Run with either `./GenSMBIOS.command` or by double-clicking *GenSMBIOS.command*

- Type `iMac15,1 10`

- Add the last results to `PlatformInfo > Generic > MLB, SystemSerialNumber and SystemUUID`

</details>

<details>  
<summary><strong>Enable HiDPI</strong></summary>

- Disable SIP (just for this process, you can enable it once finished)

- Run this script in Terminal

```bash
bash -c "$(curl -fsSL https://raw.githubusercontent.com/xzhih/one-key-hidpi/master/hidpi.sh)"
```

</details>

## Other tweaks

<details>  
<summary><strong>Monitor temperatures and power consumption</strong></summary>

- Download and install [HWMonitor](https://github.com/kzlekk/HWSensors/releases)
- Open the app and check `launch on login` option

</details> 

<details>  
<summary><strong>Faster macOS dock animation</strong></summary>

- Run these lines in terminal:

```bash
$ defaults write com.apple.dock autohide-delay -float 0
$ defaults write com.apple.dock autohide-time-modifier -float 0.5
$ killall Dock
```
</details>

<details>  
<summary><strong>Mac Bootloader GUI and Boot Chime</strong></summary>

- Follow the appropriate [Guide](https://dortania.github.io/OpenCore-Post-Install/cosmetic/gui.html#setting-up-opencore-s-gui).

</details>

## Status
<details>  
<summary><strong>What's working ✅</strong></summary>

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

</details>

<details>  
<summary><strong>What's not working ⚠️</strong></summary>


Nothing to mention so far

</details>

<details>  
<summary><strong>Update tracker 🔄</strong></summary>

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

</details>

<details>  
<summary><strong>Changelog</strong></summary>

- 20200824:  
New README for improved readability

</details>

## Thanks to

The hackintosh community from GitHub, [InsanelyMac](https://www.insanelymac.com/forum/) and [r/hackintosh](https://www.reddit.com/r/hackintosh/).

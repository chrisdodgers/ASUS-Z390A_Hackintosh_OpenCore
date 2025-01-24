# ASUS Z390-A with a RX 5700XT - macOS Sequoia 15.x Hackintosh (OpenCore)

[![Z390-A](https://img.shields.io/badge/ASUS_Z390A-green)](https://www.asus.com/us/motherboards-components/motherboards/prime/prime-z390-a/)
[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.3-blue)](https://github.com/acidanthera/OpenCorePkg/releases/latest)
![MacOS](https://img.shields.io/badge/macOS-15.x-purple.svg)
[![Release](https://img.shields.io/badge/Download-Latest-success.svg)](https://github.com/chrisdodgers/ASUS-Z390A_Hackintosh_OpenCore/releases)<br>![SequoiaLogo](https://github.com/chrisdodgers/ASUS-Z390A_Hackintosh_OpenCore/blob/main/Photos/Sequoia-Logo.jpeg)


## About
This is an OpenCore EFI folder for running macOS Sequoia (15.x) on an ASUS Z390-A system with a RX 5000/6000 series GPU. This may be used as a helpful reference on you creating your own EFI using the Dortania guide.

## What Works?

- Wifi/Bluetooth
- Gigabit Ethernet
- Handoff/Continuity features and Universal Control
- Perfect Sleep/Hibernation (Including PowerNap)
- Hardware DRM Support
- Full dGPU Graphics Acceleration with multiple 1440p+ monitors
- CPU Power Management
- Rear Audio Outputs
- USB 3.2, 3.1, 3.0, 2.0 (including USB-C ports)

### What doesn't work?
- Case headphone output *(This could probably be fixed using a different layout-id for audio, but I use a dedicated USB audio interface so I never did investigate further on this.)*



## Specs

| Component           | Details                                       |
| ------------------: | :-------------------------------------------- |
| Motherboard         | ASUS Prime Z390-A |
| Chipset             | Intel Z390 |
| ASUS BIOS Version   | 2004 [(Customized with an Apple logo instead of the ASUS logo)](https://github.com/chrisdodgers/ASUS-PRIME-Z390A_Custom_AppleLogo_BIOS/tree/main) |
| Processor           | Intel Core i9-9900K |
| Memory              | 32GB Corsair DDR4 3000Mhz |
| Storage             | 2TB Western Digital SN770 NVMe |
| Integrated Graphics | Intel UHD Graphics 630 |
| Dedicated Graphics  | MSI RX 5700XT (AMD) |
| Audio               | Realtek ALC1220 (layout-id:`11`)|
| Ethernet            | Intel i219-V Gigabit Ethernet|
| WiFi and Bluetooth  | Fenvi T919 (BCM94360CD)|

### macOS-incompatible Components
- **NVIDIA GPUs are NOT SUPPORTED!** (Make sure you have a compatible AMD dGPU by checking out the [supported GPU list](https://dortania.github.io/GPU-Buyers-Guide/modern-gpus/amd-gpu.html)). 

## EFI Folder Content (OpenCore)

<details>
<summary><strong>Click to reveal</strong></summary>

```
EFI
├── BOOT
│   └── BOOTx64.efi
└── OC
    ├── ACPI
    │   ├── SSDT-EC.aml
    │   ├── SSDT-PLUG.aml
    │   ├── SSDT-PMC.aml
    │   ├── SSDT-RTCAWAC.aml
    │   ├── SSDT-USBX.aml
    ├── Drivers
    │   ├── HfsPlus.efi
    │   ├── OpenCanopy.efi
    │   ├── OpenRuntime.efi
    │   └── ResetNvramEntry.efi
    ├── Kexts 
    │   ├── AMFIPass.kext (Required for macOS 14+ for Wifi Root-Patching)
    │   ├── AppleALC.kext
    │   ├── IntelMausi.kext
    │   ├── IO80211FamilyLegacy.kext (Required for macOS 14+ for Wifi)
    │   ├── IOSkywalkFamily.kext (Required for macOS 14+ for Wifi)
    │   ├── Lilu.kext
    │   ├── RestrictEvents.kext
    │   ├── RTCMemoryFixup,kext
    │   ├── SMCProcessor.kext
    │   ├── SMCRadeonSensors.kext (Used only if using a compatible AMD dGPU)
    │   ├── SMCSuperIO.kext 
    │   ├── USBToolBox.kext
    │   ├── UTBMap.kext
    │   ├── VirtualSMC.kext
    │   ├── WhateverGreen.kext
    ├── OpenCore.efi
    ├── Resources (NOTE: shows sub-folders only, no files)
    |   ├── Audio
    │   ├── Font
    │   └── Image
    │   │    └── Acidanthera
    │   │        └── GoldenGateExt-Icons
    │   │    
    │   └── Label    
    ├── Tools  
    │   ├── ControlMsrE2.efi
    │   ├── OpenShell.efi   
    │       
    └── config.plist
```
</details>

# Deployment
Please read the Dortania guide and guide notes here very carefully and thoroughly! I recommend only using my EFI folder as a point of reference and NOT for actual use. Instead, follow the Dortania guide to create your own EFI and only use my EFI as a reference. My configuration even if identical specs may differ from your configuration! 


## Pre-Requisites:
### BIOS Configuration:
<details>
<summary><strong>Click to reveal</strong></summary>

`Advanced -> CPU Configuration -> Software Guard Extensions (SGX)` = *Disabled*

`Advanced -> CPU Configuration -> Intel (VMX) Virtualization Technology` = *Enabled*

`Advanced -> CPU Configuration -> Hyper-Threading` = *Enabled*

`Advanced -> CPU Configuration -> CPU-Power Management Control -> CFG Lock` = *Disabled*

`Advanced -> System Agent (SA) Configuration -> VT-d` = *Enabled*

`Advanced -> System Agent (SA) Configuration -> Above 4G Decoding` = *Enabled*

`Advanced -> System Agent (SA) Configuration -> Graphics Configuration -> Primary Display` = *Auto*

`Advanced -> System Agent (SA) Configuration -> Graphics Configuration -> CPU Multi-Monitor` = *Disabled*

`Advanced ->  SATA Mode Selection` = *AHCI*

`Advanced -> USB Configuration -> Legacy USB Support` = *Disabled*

`Advanced -> USB Configuration -> XHCI Hand-off` = *Enabled*

`Boot -> CSM (Compatibility Support Module)` = *Disabled*

`Boot -> Secure Boot -> OS Type` = *Other OS*



</details>

### USB Mapping (IMPORTANT!):
- USB mapping is very critical in macOS to have proper sleep and to have your USB ports/devices work properly. Unfortunately in macOS, there is a XHCI port limit of 15, so you will need to disable some of your USB ports. *(There is a way to bypass this limit, but it comes with some negative cons and I will not be covering nor recommending bypassing the 15 port limit).*
- **You should create your own USB Map!** 
-  **Download [USBToolBox](https://github.com/USBToolBox/tool) to create your own USB Map.** [Here is an easy to follow guide](https://www.reddit.com/r/hackintosh/comments/ta1ef4/guide_easy_usb_mapping_with_usbtoolbox_on_windows/) on how to use USBToolBox. 

> [!NOTE]
> My USB Map may be different than what your USB Map should be. There is a chance of my USB Map not working well on your configuration. Once again, please follow the above guide to create your own USB Map as it is very easy. 
> 
> If you still choose to use my USB Map which I don't recommend:
>  
> - The bottom 2 USB 3.0 ports are disabled
> - The top right USB 2.0 port is disabled (closest to the PS/2 port)


### Generate your own SSDTs using SSDTTime (Optional, but highly recommended!):
- The SSDT files found in my EFI folder (`EFI -> OC -> ACPI`) I generated using [SSDTTime](https://github.com/corpnewt/SSDTTime). Mine were generated for my exact configuration. Your configuration may differ slightly from mine, so it is HIGHLY RECOMMENDED to generate your own SSDT files as it is very easy to do.
   1. Run SSDTTime.bat in Windows as an Administrator
   2. Select option `P`(Dump DSDT)
   3. Select the following options to generate: `2`(FakeEC), `4`(USBX), `5`(PluginType), `6`(PMC), `7`(RTCAWAC).
   4. Inside the SSDTTime folder, there should now be a "Results" folder within there. You should see the following files: `SSDT-EC.aml SSDT-PLUG.aml SSDT-PMC.aml SSDT-RTCAWAC.aml SSDT-USBX.aml`. Copy those files to replace the exisiting SSDT files found in `EFI -> OC -> ACPI`.

## Preparing the `config.plist`:
Download and extract the EFI Folder found in the Releases section of this repo.

- **NOTE: ONLY use [ProperTree Editor](https://github.com/corpnewt/ProperTree) for making any edits to config.plist.** OCAT and other configurator tools are known for breaking configurations! You have been warned!


1. **DeviceProperties**: This guide assumes you have an AMD dGPU to drive your display. The framebuffer already specified in my EFI is `AAPL,ig-platform-id 0300913E` which is a headless framebuffer used for this scenario. If you are not using an AMD dGPU to drive your displays, you will need to specify another framebuffer! 

> [!WARNING]
> 
> You will need to follow [the framebuffer patching portion of the Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/config.plist/coffee-lake.html#deviceproperties) if you are using an iGPU to drive your display(s)! My guide will not cover this!
>


2. **Kernel**:
   - Under `Kernel -> Quirks`, `AppleCpuPmCfgLock` and `AppleXcpmCfgLock` is set to `False` which is ideal. **In your BIOS settings, CFG-Lock MUST be set to Disabled for this configuration to work!**. 
   
>[!WARNING]
>*If for some reason you do not have a BIOS option to set your CFG-Lock to Disabled, you can temporarily change the value above to `True`. It is highly advised to instead [find a method for your motherboard to disable CFG-Lock!](https://dortania.github.io/OpenCore-Post-Install/misc/msr-lock.html*
>

> [!IMPORTANT]
> Your system may not boot if this kernel section is not configured properly!
> 

3. **Wifi and Bluetooth**: This guide assumes you have a Fenvi T919 Wifi/Bluetooth card which is a BCM94360CD. Bluetooth/Wifi is required for Handoff/Continuity features to work! Intel Wifi/Bluetooth cards most likely will break many Handoff/Continuity features. If you plan on using an Intel card, you will need to follow [this portion of the Dortania guide](https://dortania.github.io/OpenCore-Install-Guide/ktext.html#wifi-and-bluetooth) as it will not be covered in my guide/repo. *(If you do not care about wifi/bluetooth and handoff/continuity features and do not want it - this can be fully ignored.)*   

4. **Additional Kexts**:
   - I did not include [VoodooPS2.kext](https://github.com/acidanthera/VoodooPS2/releases) which is only needed if you plan on using a PS/2 Keyboard or Mouse. (You most will not likely need this.)
   - My NVMe drive does not require [NVMeFix.kext](https://github.com/acidanthera/NVMeFix), so I did not include it in my EFI. Your NVMe drive may require this! [*Check the compatibility of your NVMe drive.*](https://dortania.github.io/Anti-Hackintosh-Buyers-Guide/Storage.html)

> [!NOTE]   
>If you add these optional kexts into the `EFI -> OC -> Kexts`, you will need to do an OC Snapshot in ProperTree. You can do this by selecting `"File" -> "OC Snapshot"` in the menu and then select the OC folder in the EFI. An OC Snapshot automatically enables these kexts and automatically defines them in the `Kernel -> Add` section. 
>

5. **Misc**(optional):
   - Only if you are having issues booting and want detailed logging, set `Misc -> Debug -> AppleDebug = True` and `Misc -> Debug -> ApplePanic = True`. Also set `Misc -> Debug -> Target = 67`.
   - If you want to hide the boot picker, set `Misc -> Boot -> ShowPicker = False`. Or, you can shorten the boot picker by lowering the value (in seconds) by adjusting `Misc -> Boot -> Timeout`.

6. **NVRAM**:
   - `rtc-blacklist` has been set to `5859`. This was configured as macOS kept writing to RTC memory regions which caused the system to boot into safe-mode after a reboot/shutdown. This value should work for most Z390 systems. However if this value does not work for you, you can follow [the RTC fix portion of the Dortania guide](https://dortania.github.io/OpenCore-Post-Install/misc/rtc.html#finding-our-bad-rtc-region)
   - `revblock` has `pci` enabled which prevents us from seeing a PCI warning on each boot. This is specific to using MacPro7,1 as our SMBIOS. 
   - `boot-args`:  
      - `agdpmod=pikera` is added here as I am using an AMD RX 5700XT. This boot-arg is ONLY required for RX 5000 and RX 6000 series GPUs, so remove this if you are not using a GPU that requires this! 
      - `-v debug=0x100 keepsyms=1`*(Optional - if you need additional debugging enabled.*)
   - `csr-active-config`: The current specified value is `03080000`**(Partial SIP)**. This is required for root-patching wifi in macOS 14+ using [OCLP](https://github.com/dortania/OpenCore-Legacy-Patcher/releases). If you are running macOS 13 or older, or do not plan on using/patching wifi on macOS 14+ - you can fully enable SIP by setting the value to `00000000`**(Full SIP)**.


7. **PlatformInfo**:
   - Download the latest version of [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) and run the .bat file if on windows, or the .command file if on macOS. 
   - Select option (1) to install/update MacSerial.
   - Select option (2) and drag and drop your config.plist file onto the open terminal window.
   - Select option (3) and enter `MacPro7,1`
   - If completed successfully, you should see what you just generated within `PlatformInfo -> Generic` (`MLB`, `ROM`, `SystemSerialNumber`, and `SystemUUID` should be populated).
> [!IMPORTANT]
> Your system may not boot and or iServices may not work if this section is not configured properly!
>    
   

## Installation Guide:
This portion of the guide I will not provide support for. This is well covered in the [Dortania Guide]((https://dortania.github.io/OpenCore-Install-Guide/installer-guide/))

1. **Create a bootable USB macOS Sequoia installer.**

2. **Copy the EFI folder to the bootable USB**
   - Run [MountEFI](https://github.com/corpnewt/MountEFI) to mount the EFI partition of your bootable USB drive. Once the EFI partition is mounted, you can copy my EFI folder there.

3. **Boot into OpenCore and reset NVRAM**
   - Once you see the OpenCore BootPicker, press the Space Bar which will show some hidden tools. Select "ResetNvramEntry.efi and it should reboot your system. 

4. **Boot into the macOS Sequoia Installer**
   - Select "Install macOS Sequoia" in the OpenCore BootPicker

5. **Install macOS Sequoia**
   - Follow the macOS installation instructions on your screen. Once the installer has completed, you should be able to follow the welcome/setup screen instructions to create an account and then get to the desktop. 

> [!WARNING]
>  Do NOT sign in with your Apple ID at the setup screen! Sign in with your Apple ID once we have root-patched wifi. *This is so we can ensure Handoff/Continuity features work properly. Since wifi at this point in the guide is broken, there is a chance signing in before fixing wifi could cause some slight issues with Handoff/Continuity down the line.* 
>

   
## Post-Installation Guide:
### Boot macOS Without a USB Drive:
- Run MountEFI and select your installation USB and the drive you installed macOS on.
- Copy your EFI folder from your USB EFI partition to your internal drives EFI partition

> [!NOTE]
> You may see a folder that already exists in your internal drives EFI folder called `APPLE`. This can be either ignored or deleted as it serves a purpose for real Macs and not Hackintosh 
> 

  
### Fixing Wifi (Fenvi T919):
Since macOS 14+ dropped native support for BCM94360CD, we have to use Opencore Legacy Patcher (OCLP) to apply root-patches to restore wifi functionality. **NOTE: If you plan on using FileVault and want to patch wifi, make sure to follow this porition FIRST before turning on FileVault (optional)!**

1. **[Download and install the latest version of OCLP](https://github.com/dortania/OpenCore-Legacy-Patcher/releases)**
2. **Apply Root-Patches**
   - Once you launch OCLP, you should see an option that says *"Post-Install Root Patch"*. Select that and it should automatically detect that it needs to apply networking patches. 
3. **Reboot and wifi should now properly work!** 
   - *(If you followed these steps and wifi still isn't working - try resetting your NVRAM like earlier in the installation guide)*
4. (Optional - Now you can sign into your Apple ID in System Settings.)   
4. (Optional - You can now move on to the FileVault portion of this guide.)

### Enabling FileVault (Optional)
I discovered when I tried to enable FileVault, I would get an "Invalid Password" error no matter how I tried to save my encryption keys. This is due to us being booted with Partial SIP instead of Full SIP. Partial SIP is required for OCLP root-patches to work. However, the only way I was able to setup FileVault was enabling Full SIP. The good news is FileVault will work just fine once we set SIP from Full to Partial after setting up FileVault!

1. **Enable Full SIP**
   - Go to `NVRAM -> csr-active-config` in your config.plist and set the value to `00000000`
   - Reboot system. (I would recommend like earlier in the installation guide to reset your NVRAM.)
2. **Enable FileVault in System Settings**
   - At this point before enabling FileVault, in System Settings you could go ahead and sign-in with your Apple ID. Doing so before turning on FileVault will allow you the option to save your encryption key to iCloud.
   - Enable FileVault and be patient while FileVault encrypts your volume.
3. **Enable Full SIP**
   - Now that FileVault has completed its setup, go ahead and go back to Partial SIP by setting the `NVRAM -> csr-active-config` value back to `03080000`.
   - Reboot system. (Once again, I would recommend like earlier to reset your NVRAM.)   
   

### Fix AGDC (Only needed for AMD dGPUs using certain 1440p or 4k displays):
This is only required for some situations with AMD dGPUs. This was required for my configuration with my RX 5700xt and dual 1440p monitors. This is already configured in the EFI you downloaded. If you do not need this fix, you can simply remove it from DeviceProperties.

1. Download [gfxutil](https://github.com/acidanthera/gfxutil/releases)
2. Run the following command: `/path/to/gfxutil -f GFX0` *(Replace /path/to/gfxutil with the actual path to gfxutil, or open a Terminal window and drag and drop gfxutil on that window and press "Enter".)*
3. Inside the config.plist, go to `DeviceProperties -> Add` and you will see `PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)`. Within that dictionary you will see `CFG,CFG_USE_AGDC` with a data value of `00`.
4. Compare what gfxutil found for you to the PciRoot path found in the step above. If the path gfxutil shows is different, replace `PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)/Pci(0x0,0x0)` with the path gfxutil found.

### Reduce Boot Time:
In `UEFI -> Drivers`, disable `ConnectDrivers`. This reduces the timeout between the ASUS logo and the BootPicker by a few seconds

> [!WARNING]
>
> - Before installing macOS from a USB flash drive, `ConnectDrivers` needs to be re-enabled, otherwise you won't see it in the BootPicker.
> - With `ConnectDrivers` disabled, the bootchime cannot be played back since `AudioDXE.efi` is not loaded.

## CPU Benchmark:
![Screenshot](https://github.com/chrisdodgers/ASUS-Z390A_Hackintosh_OpenCore/blob/main/Photos/Sequoia-Benchmark.png)</br>

## Additional Photos:
![Proof](https://github.com/chrisdodgers/ASUS-Z390A_Hackintosh_OpenCore/blob/main/Photos/Sequoia-Proof.png)</br>

## Credits and Thanks:
- Apple for macOS
- Acidanthera for [OpenCore Bootloader](https://github.com/acidanthera/OpenCorePkg) and countless Kexts
- Dortania for [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide) and [OpenCore Legacy Pacher](https://dortania.github.io/OpenCore-Legacy-Patcher/)
- [Corpnewt](https://github.com/corpnewt) for SSDTTime, GenSMBIOS, MountEFI, and ProperTree
- jsassu20 for [MacDown](https://macdown.uranusjr.com/) Markdown Editor
- [5T33Z0](https://github.com/5T33Z0) for a great MD template used for making this guide, and for creating great guides and write-ups in the community.

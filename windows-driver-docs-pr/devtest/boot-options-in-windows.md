---
title: Overview of Boot Options in Windows
description: Describes Windows boot loader architecture, firmware-independent boot configuration, and boot option editing tool.
ms.assetid: 1cc5b1cc-8d0e-4b4e-93fe-272772a3e458
keywords:
- boot options WDK , Windows
- editing boot options
- multiboot systems WDK boot options
- legacy boot entries WDK
- Boot Configuration Data WDK
- BCD WDK
- BCDEdit tool
- boot options WDK , editing
- ntldr tool
- Windows Boot Manager WDK
- Bootmgr tool
- system-specific boot loaders WDK
- boot loaders WDK
- firmware-independent boot options WDK
ms.author: windowsdriverdev
ms.date: 06/29/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---

# Overview of Boot Options in Windows

The Windows boot loader architecture includes a firmware-independent boot configuration and storage system called *Boot Configuration Data* (BCD) and a boot option editing tool, BCDEdit (BCDEdit.exe). During development, you can use BCDEdit to configure boot options for debugging, testing, and troubleshooting your driver on computers running Windows 10, Windows 8, Windows Server 2012, Windows 7, and Windows Server 2008.

**Important**  Administrative privileges are required to use BCDEdit to modify BCD. Changing some boot entry options using BCDEdit could render your computer inoperable. As an alternative, use the System Configuration utility (MSConfig.exe) to change boot settings.

## Boot Loading Architecture

Windows includes boot loader components that are designed to load Windows quickly and securely. The previous Windows NT boot loader, *ntldr*, is replaced by three components:

-   Windows Boot Manager (Bootmgr.exe)

-   Windows operating system loader (Winload.exe)

-   Windows resume loader (Winresume.exe)

In this configuration, the Windows Boot Manager is generic and unaware of the specific requirements for each operating system while the system-specific boot loaders are optimized for the system that they load.

When a computer with multiple boot entries includes at least one entry for Windows, the Windows Boot Manager, which resides in the root directory, starts the system and interacts with the user. It displays the boot menu, loads the selected system-specific boot loader, and passes the boot parameters to the boot loader.

The boot loaders reside in the root directory of each Windows partition. Once selected, the boot loaders take over the boot process and load the operating system in accordance with the selected boot parameters.

## <span id="boot_configuration_data"></span><span id="BOOT_CONFIGURATION_DATA"></span>Boot Configuration Data

Windows boot options are stored in the Boot Configuration Data (BCD) store on BIOS-based and EFI-based computers.

BCD replaces the traditional Boot.ini text file in BIOS-based systems. Storing boot parameters in a text file, however simple, was considered to be too vulnerable to malicious attacks to justify its use. On EFI-based computers, where boot options are stored in NVRAM, you use the same BCD methods to edit boot options as you would use on a BIOS-based computer, instead of accessing NVRAM directly using Windows APIs or specialized tools.

BCD provides a common, firmware-independent boot option interface for all computers running Windows 10, Windows 8, Windows Server 2012, Windows 7, and Windows Server 2008. It is more secure than previous boot option storage configurations, because it permits secure lockdown of the BCD store and lets Administrators assign rights for managing boot options. BCD is available at run time and during all phases of setup. You can even call BCD during power state transitions and use it to define the boot process for resuming after hibernation.

You can manage BCD remotely and manage BCD when the system boots from media other than the media on which the BCD store resides. This feature is extremely important for debugging and troubleshooting, especially when a BCD store must be restored while running Startup Repair from a CD, from USB-based storage media, or even remotely.

BCD is easy to use. The BCD store, with its familiar object-and-element architecture, uses GUIDs to precisely identify boot-related applications.

BCD includes its own set of boot options. Most of the Windows boot options that were used before BCD, such as **/debug**, **/maxmem**, and **/pae**, have been preserved; however, in some cases, the names of the options might have changed to better suite their function. For more information about these boot options, see [BCD Boot Options Reference](https://msdn.microsoft.com/library/windows/hardware/ff542205).

## <span id="multiboot_scenarios"></span><span id="MULTIBOOT_SCENARIOS"></span>Multiboot Scenarios

If multiple Windows operating systems are installed on the computer, the Windows Boot Manager works with the booting components for older ("legacy") versions of Windows to interact with the user and start the selected operating system.

When a multiboot computer is started, the following scenario occurs:

-   The Windows Boot Manager displays a menu with the boot entries for Windows and a **Legacy** option.

-   If you select a boot entry for a specific version of Windows, the Windows Boot Manager loads the system-specific boot loader for that operating system and passes the parameters for that boot entry to the system-specific boot loader. The system-specific boot loader loads the operating system in accordance with the boot parameters.

-   If you select **Legacy**, the Windows Boot Manager starts Ntldr, the boot manager for NT-based Windows operating systems prior to Windows Vista. From this point forward, the boot process proceeds as it did prior to Windows Vista.

    If the computer includes multiple installations of pre-Windows Vista Windows, Ntldr displays a boot menu consisting of the entries for these operating systems. This boot menu is generated from the entries in the Boot.ini file on BIOS-based systems and the boot entries stored in EFI-NVRAM on EFI-based systems. When you select a boot entry, Ntldr loads the operating system in accordance with the boot parameters.

## Editing Boot Options

To edit boot options in Windows, use BCDEdit (BCDEdit.exe), a tool included in Windows. You cannot use Bootcfg or NvrBoot to edit boot options in Windows, although you can continue to use them to edit boot options on legacy versions of Windows.

To use BCDEdit, you must be a member of the Administrators group on the computer.

You can also use the System Configuration utility (MSConfig.exe) to change boot settings.

To change boot options programmatically in Windows, use the Windows Management Instrument (WMI) interface to boot options. This BCD WMI interface is the best method to programmatically change the boot options. For information about the BCD WMI interface, see [Boot Configuration Data WMI Provider](https://msdn.microsoft.com/library/windows/desktop/bb986746(v=vs.85).aspx) in the Windows SDK documentation.

## Related topics

- [BCD Boot Options Reference](https://msdn.microsoft.com/library/windows/hardware/ff542205)
- [Editing Boot Options](editing-boot-options.md)
- [Using Boot Parameters](using-boot-parameters.md)
- [Boot Configuration Data](http://go.microsoft.com/fwlink/p/?linkid=74322)


 

 







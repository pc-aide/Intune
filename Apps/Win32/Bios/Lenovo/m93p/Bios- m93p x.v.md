# Bios-ThinkStation m93p x.v

---

## Acronym
1. ddl - download direct link

---

## URL
1. [ds035753-flash-bios](https://support.lenovo.com/us/en/downloads/ds035753-flash-bios-update-thinkcentre-e93-m73p-m83-m93-m93p-thinkstation-e32-p300)
2. [ddl-fbjye0usa.exe](https://download.lenovo.com/pccbbs/thinkcentre_bios/fbjye0usa.exe)

---

## Group
1. Name : Intune_Deploy_M93p
2. Type : Dynamic device
3. Query : `(device.deviceModel -eq "10AAA16JAU")`

---

## Flash Porgram options
````txt
*****************************************************************************
*                          3. Flash Program Options                         *
*                                                                           *
*     wflash2.exe [option1] [option2] ... [optionX]                         *
*                                                                           *
*     [OPTIONS]                                                             *
*     /h               Show help messages.                                  *
*     /rsmb            Preserve all SMBIOS structures.                      *
*     /clr             Clear BIOS settings.                                 *
*     /ign             Ingore BIOS version check.                           *
*     /sn:nnnnnnn      Update system serial number (up to 20 characters).   *
*     /csn:nnnnnnn     Update chassis serial number (up to 20 characters).  *
*     /mtm:nnnnnnn     Update machine type and model number (up to 25       *
*                      characters).                                         *
*     /tag:nnnnnnn     Update system asset tag (up to 25 characters).       *
*     /uuid            The flash utility will generate an Universally       *
*                      Unique Identifier (UUID), replacing the one that     *
*                      is currently in the system.                          *
*     /logo:<filename> Change logo. The max supported size of logo file     *
*                      is displayed on the screen during the compressing.   *
*     /cpu             Update Intel CPU microcode.                          *
*     /reboot          Automatic reboot after all requests done.            *
*     /pass:nnnnnnn    Input current system password.                       *
*     /quiet           Operating without physical presence.                 *
*                                                                           *
*     The following example shows how to update system asset tag number     *
*     to "1234567" use command line:                                        *
*       wflash2.exe /tag:1234567                                            *
*                                                                           *
*     The following example shows how to update bios and update system      *
*     asset tag number by one command:                                      *
*       wflash2.exe bios.rom /tag:1234567                                   *
*                                                                           *
*     The following example shows how to change the power-on logo.          *
*       wflash2.exe /logo:myfav.bmp                                         *
*                                                                           *
*     Note: A flash update image using these program options should be      *
*           tested carefully before widespread usage.                       *
*                                                                           *
*****************************************************************************
````

---

## before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : FBKTA5AUS
Manufacturer      : LENOVO
Name              : FBKTA5AUS
SerialNumber      : PC03PK9M
Version           : LENOVO - 1A50
````

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
1. Name : Intune_Desktop_M93p_Lenovo
2. Type : Dynamic device
3. Query : `(device.deviceModel -eq "10AAA16JAU")`

---

## Flash Porgram options
1. wflash2.exe call afuwinx64.exe
````txt
 +---------------------------------------------------------------------------+
|                 AMI Firmware Update Utility  v5.09.02.1384.07.B608.LV     |
|      Copyright (C)2017 American Megatrends Inc. All Rights Reserved.      |
+---------------------------------------------------------------------------+
| Usage: AFUWINx64.EXE <ROM File Name> [Option 1] [Option 2]...             |
|           or                                                              |
|        AFUWINx64.EXE <Input or Output File Name> <Command>                |
|           or                                                              |
|        AFUWINx64.EXE <Command>                                            |
| ------------------------------------------------------------------------- |
| Commands:                                                                 |
|         /O - Save current ROM image to file                               |
|         /U - Display ROM File's ROMID                                     |
|         /S - Refer to Options: /S                                         |
|         /D - Verification test of given ROM File without flashing BIOS.   |
|         /A - Refer to Options: /A                                         |
|       /OAD - Refer to Options: /OAD                                       |
| /CLNEVNLOG - Refer to Options: /CLNEVNLOG                                 |
| Options:                                                                  |
|      /CMD: - Send special command to BIOS. /CMD:{xxx}                     |
|       /DPC - Don't Check Aptio 4 and Aptio 5 platform.                    |
|     /MEUL: - Program ME Entire Firmware Block, which supports             |
|              Production.BIN and PreProduction.BIN files.                  |
|         /Q - Silent execution                                             |
|         /X - Don't Check ROM ID                                           |
|       /CAF - Compare ROM file's data with Systems is different or         |
|              not, if not then cancel related update.                      |
|         /S - Display current system's ROMID                               |
|       /JBC - Don't Check AC adapter and battery                           |
|  /HOLEOUT: - Save specific ROM Hole according to RomHole GUID.            |
|              NewRomHole1.BIN /HOLEOUT:GUID                                |
|        /SP - Preserve Setup setting.                                      |
|         /R - Preserve ALL SMBIOS structure during programming             |
|        /Rn - Preserve SMBIOS type N during programming(n=0-255)           |
|         /B - Program Boot Block                                           |
|         /P - Program Main BIOS                                            |
|         /N - Program NVRAM                                                |
|         /K - Program all non-critical blocks.                             |
|        /Kn - Program n'th non-critical block(n=0-15).                     |
|     /HOLE: - Update specific ROM Hole according to RomHole GUID.          |
|              NewRomHole1.BIN /HOLE:GUID                                   |
<Press any key to continue>
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

---

## install.ps1
1. extract Flash bios folder

[<img src="https://i.imgur.com/PsHqQzY.png">](https://i.imgur.com/PsHqQzY.png)


2. install.ps1
````ps1
# ver : 27-11-2022

$ErrorActionPreference = "stop"
$Log_file = "MAJ_Bios_Lenovo.xt"

# /rsmb --Preserve all SMBIOS structure
try{
    Suspend-BitLocker c: -RebootCount 1
    start wflash2.exe -args "IMAGEFB.ROM /rsmb /quiet /reboot" `
        -RedirectStandardOutput $env:ProgramData\$Log_file -wait -winDowStyle Hidden
}catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

---

## intuneWinAppUtil
````ps1
.\IntuneWinAppUtil.exe -v
1.8.4.0

# -c --source_folder
# -s --source_setup_file
# -o --output_folder
# -q --quiet
start IntuneWinAppUtil.exe -args "-c $env:temp\intuneWin32 -s $env:temp\intuneWin32\install.ps1 -o $env:temp\intuneWin32 -q"
````

[<img src="https://i.imgur.com/nhwtrJe.png">](https://i.imgur.com/nhwtrJe.png)

---

## Intune
1. NewApps\win32
2. Name : Bios-ThinkStation m93p
3. Desc.
````md
# Bios-ThinkStation m93p
````
4. Publisher : Lenovo
5. Logo
6. Install command / Uninstall command `powershell -executionpolicy bypass -file Install.ps1`
7. Install behavior : system
8. Device restart behavior : App Install may force a device restart
9. Operating system architecture : x64
10. Minimum operating system : 20h04
11. Detection rules\Rule type : Registry
12. Key path : HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS
13. Value name : BIOSVersion
14. Detection method : String comparison
15. Operator : Equals
16. Value : `FBKTE0A`
17. Associated with a 32-bit app on 64-bit clients : no
18. Assignments\Add group : Intune_Desktop_M93p_Lenovo

---

## companyPortal
1. run\companyPortal:

2. force sync from the client or Intune 

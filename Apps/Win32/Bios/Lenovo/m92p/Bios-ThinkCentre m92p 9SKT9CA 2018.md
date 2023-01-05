# Bios-ThinkCentre m92p 2018

---

## URL
1. [download-page](https://support.lenovo.com/ca/en/downloads/ds029265-flash-bios-update-thinkcentre-edge-92-thinkcentre-m82-m92-and-m92p-thinkstation-e31)
2. [ddl-installShield-9sjy9cusa.exe](https://download.lenovo.com/pccbbs/thinkcentre_bios/9sjy9cusa.exe)

---

## Before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : 9SKT69AUS
Manufacturer      : LENOVO
Name              : 9SKT69AUS
SerialNumber      : MJVHTXA
Version           : LENOVO - 1450
````

---

## NewGroup
1. Name : Intune_Postes_M92p
2. Type: Dynamic Group
3. Query : `(device.deviceModel -contains "3237CN2")`

---

## Extract
1. extract 9SJY9CUSA :

[<img src="https://i.imgur.com/fq9U19K.png">](https://i.imgur.com/fq9U19K.png)

---

## wFlash2.exe
````txt
Lenovo Firmware Update Utility 4.3.2
(C) Copyright 1984-2012, Lenovo Group. All Rights Reserved.

Usage1: Operating in default mode (only for DOS system)
   wflash2.exe <BIOS file name>
Usage2: Update BIOS and other data.
   wflash2.exe <BIOS file name> [arg1] [arg2]...
Usage3: Update other data without updating BIOS.
   wflash2.exe [arg1] [arg2]...

Arguments:
   /rsmb              Preserve all SMBIOS structures
   /clr               Clear BIOS settings
   /ign               Ignore BIOS version check
   /mtm:nnnnnnn       Update system Machine Type and Model
   /sn:nnnnnnn        Update system Serial Number
   /csn:nnnnnnn       Update Chassis Serial Number
   /tag:nnnnnnn       Update Asset Tag
   /uuid              Generate and update system UUID
   /cpu               Update Intel CPU microcode
   /logo:<file name>  Change logo
   /quiet             Operating without physical presence
   /reboot            Automaticly reboot after all requests done
   /pass:nnnnnnn      Input current system password
````

---

## install.ps1
````ps1
# ver: 03-12-2022
 
$Log_file = 'MAJ_Bios_lenovo.txt'

try{
     # /rsmb --Preserve all SMBIOS structures
     # /reboot --Automaticly reboot after all requests done # not work popUp as the same m93p
    Suspend-Bitlocker c: -RebootCount 1
    start .\wflash2.exe -args 'IMAGE9S.CAP /rsmb /quiet' `
        -RedirectStandardOutput $env:ProgramData\$Log_file -Wait -WindowStyle Hidden
    Restart-Computer -force
}
catch{
    $_ | out-file $env:ProgramData\$Log_file
}
`````

[<img src="https://i.imgur.com/pmbQ077.png">](https://i.imgur.com/pmbQ077.png)

---

## IntuneWinApputil.exe
````ps1
# 1.8.4.0
.\IntuneWinAppUtil.exe
Please specify the source folder: $env:temp
Please specify the setup file: $env:temp\install.ps1
Please specify the output folder: $env:temp
Do you want to specify catalog folder (Y/N)?n
````

[<img src="https://i.imgur.com/1vZEPbN.png">](https://i.imgur.com/1vZEPbN.png)

---

## NewApp
1. App type : Windows app (Win32)
2. Select file : install.intunewin
3. Name : Bios-ThinkCentre m92p 2018
4. Desc : `# Bios-ThinkCentre m92p 2018`
5. Publisher : Lenovo
6. Logo : takeIt on google\Image
7. install command : `powershell -executionpolicy bypass -file Install.ps1`
8. uninstall command : `powershell -executionpolicy bypass -file Install.ps1`
9. Install behavior : system
10. OS arc. 64-bit
11. Min. OS : 2004
12. Rules format\Manually configure detection rules\Add\Rule type : Registry
13. key path : `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`
14. Value name : `BIOSVersion`
15. Detection method : string comparison
16. Operator : Equals
17. Value : `9SKT9CAUS`
18. Assignments\Required\Add group: `Intune_Postes_M92p`
19. Create

---

## Test
1. Forced sync from the client(run\companyPortal:) or the Intune (Time: ~6m)
2. Install newBios (~2min)
3. after
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : 9SKT9CAUS
Manufacturer      : LENOVO
Name              : 9SKT9CAUS
SerialNumber      : MJVHTXA
Version           : LENOVO - 1620
````
4. installed

[<img src="https://i.imgur.com/uXfhquo.png">](https://i.imgur.com/uXfhquo.png)

5. deviceName\manageApps

[<img src="https://i.imgur.com/HBqy8ah.png">](https://i.imgur.com/HBqy8ah.png)

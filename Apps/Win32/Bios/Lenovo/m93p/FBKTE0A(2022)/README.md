# Bios-ThinkStation m93p FBKTE0A (2022)

---

## Acronym
1. ddl - download direct link

---

## URL
1. [flash-bios-update-lenovo](https://support.lenovo.com/ca/en/downloads/ds035753-flash-bios-update-thinkcentre-e93-m73p-m83-m93-m93p-thinkstation-e32-p300)
2. [ddl-fbjye0usa.exe](https://download.lenovo.com/pccbbs/thinkcentre_bios/fbjye0usa.exe)

---

## Before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : FBKTE0AUS
Manufacturer      : LENOVO
Name              : FBKTE0AUS
SerialNumber      : PC03PK9M
Version           : LENOVO - 31453030
````

---

## Dynamic Group
1. Name : Postes_M93p
2. Rule syntax : `(device.deviceModel -eq "10AAA16JAU")`

---

## install.ps1
````ps1
# ver : 04-01-2023

$ErrorActionPreference = "stop"
$Log_file = "MAJ_Bios_Lenovo.xt"

try{
    Suspend-BitLocker c: -RebootCount 1
    # /sp --Preserve setup setting
    # /r --Preserve ALL SMBIOS structure during programming 
    # /q --Silent execution
    # /ign --Ignore BIOS version chec, if we have installed 2021
    # start .\AFUWIN.EXE -args "IMAGEFB.ROM /p /n /sp /r /q" -NoNewWindow
    # Time : 1m23s
    start wflash2.exe -args "IMAGEFB.ROM /rsmb /ign /quiet" `
        -RedirectStandardOutput $env:ProgramData\$Log_file -wait -winDowStyle Hidden
    Restart-Computer -Force
}catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

---

## IntuneWinAppUtil
````ps1
# -c : folder | -s setup_file | -o outfile | -q quiet
start .\IntuneWinAppUtil.exe -args "-c $env:temp -s $env:temp\install.ps1 -o $env:temp -q"
````

[<img src="https://i.imgur.com/0lMINjd.png">](https://i.imgur.com/0lMINjd.png)

---

## Upload file
1. App type : Windows app (Win32)
2. select file : install.intunewin
3. Name : `Bios-ThinkStation m93p FBJYE0USA (2022)`
4. Desc. : `# Bios-ThinkStation m93p FBJYE0USA (2022)`
5. Publisher : Lenovo
6. App Version : `FBJYE0USA (2022)`
7. logo : <logo.png>
8. install command / uninstall command : `powershell -executionpolicy bypass -file Install.ps1`
9. Install behavior : system
10. Device restart behavior : App install may force a device restart
11. OS arch. : 64-bit
12. Min. OS : 2004
13. Rule type : Registry
14. key path : `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`
15. value name : `BIOSVersion`
16. Dection method : String comparison
17. Operator : Equals
18. Value : `FBJYE0USA`
19. Associated with a 32-bit app on 64-bit clients : No
20. Assignments\Required\Add group : `Postes_M93p`

---

## Test
1. force sync\run\companyPortal:


---

### Same current bios rom image
````md

Lenovo Firmware Update Utility 4.3.2
(C) Copyright 1984-2012, Lenovo Group. All Rights Reserved.

BIOS ROM file is older than (or same as) the current BIOS ROM image.
Continue any way? (Y/y or N/n only)
````

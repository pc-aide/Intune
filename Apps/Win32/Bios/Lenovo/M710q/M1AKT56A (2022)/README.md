# ThinkCentre M710q - M1AKT56A (2022)

---

## URL
1. [download-page](https://support.lenovo.com/ca/en/downloads/ds120436-flash-bios-update-thinkcentre-m910t-m910s-m910q-m910x-m710q-thinkstation-p320-tiny)
2. [ddl-m1ajy56usa.exe-Inno_Setup](https://download.lenovo.com/pccbbs/thinkcentre_bios/m1ajy56usa.exe)

---

## Dynamic Group 
1. Name : Intune_Deploy_M710q_Lenovo
2. Rule : `(device.deviceModel -contains "10MQSABJ00")`

---

## Before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : M1AKT4DA
Manufacturer      : LENOVO
Name              : M1AKT4DA
SerialNumber      : MJ06YQ52
Version           : LENOVO - 14D0
````

---

## Extract
1. m1ajy56usa.exe

[<img src="https://i.imgur.com/eZfbusD.png">](https://i.imgur.com/eZfbusD.png)

---

# ps1
````ps1
# ver: 18-01-2023
 
$Log_file = 'MAJ_Bios_lenovo.txt'
$ErrorActionPreference = "stop"

try{
    Suspend-Bitlocker c: -RebootCount 1
    start wFlashGUIx64.exe -args 'IMAGEM1A.rom /quiet' `
        -RedirectStandardOutput $env:ProgramData\$Log_file -Wait -WindowStyle Hidden
    Restart-Computer -Force
}
catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

---

## IntuneWinAppUtil
````ps1
SRC folder : $env:temp
SRC setup : $env:temp\install.ps1
O/P folder : $env:temp
````

[<img src="https://i.imgur.com/mkGwTNA.png">](https://i.imgur.com/mkGwTNA.png)

---

## upload to Intune
1. Apps | All apps\Add
2. App type : Windows app (Win32)
3. select file : install.intunewin
4. Name : Bios-ThinkCentre M710q M1AKT56A (2022)
5. Desc: `# Bios-ThinkCentre M710q M1AKT56A (2022)`
6. Publisher : Lenovo
7. App Version : 1
8. Program\Install command : `powershell -executionpolicy bypass -file Install.ps1`
9. Program\UnInstall command : `powershell -executionpolicy bypass -file Install.ps1`
10. Requirements\Operating system architecture : 64-bit
11. Minimum OS : Windows 10 2004
12. Detection rules
13. Rule format : Manually configure detection rules
14. Type : Registry 
15. Key path : `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`
16. Value name : `BIOSVersion`
17. Detection method : String comparison
18. Operator : Equals
19. Value : `M1AKT56A`
20. Assignments : Intune_Deploy_M710q

---

## Test
1. run\companyPortal:
2. force sync
3. reboot
4. askBitlocker : yes

---

## after
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : M1AKT56A
Manufacturer      : LENOVO
Name              : M1AKT56A
SerialNumber      : MJ06YQ52
Version           : LENOVO - 1560
````

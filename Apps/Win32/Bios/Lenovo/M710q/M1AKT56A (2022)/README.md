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
4. ask Bitlocker : yes, but the PC reboot more twice, so the cause of the bitlocker ... !?

---

## wflashGUIx64.exe
````txt
wflashGUIx64.exe [option1] [option2] ... [optionX]                     *
*                                                                           *
*    [OPTIONS]                                                              *
*    /sccm            Support SCCM deployment                               *
*                       - see "Delaying Reboot in BIOS Update" below.       *
*    /sn:nnnnnnn      Update system serial number (up to 20 characters).    *
*    /mtm:nnnnnnn     Update machine type and model number (up to 25        *
*                     characters).                                          *
*    /pass:nnnnnnn    Input current system password.                        *
*    /quiet           Operating without physical presence.                  *
*    /logo:xxx.bmp    Change the power on logo                              *
*                                                                           *
*    The following example shows how to update system serial number         *
*    to "1234567" use command line:                                         *
*                                                                           *
*      wflashGUIx64.exe /sn:1234567                                         *
*                                                                           *
*    The following example shows how to update bios and update system       *
*    serial number by one command and delay the reboot:                     *
*                                                                           *
*      wflashGUIx64.exe imagem1a.rom /sn:1234567 /sccm                      *
*                                                                           *
*    The following example shows how to flash BIOS in silent mode.          * 
*                                                                           * 
*      wflashGUIx64.exe /quiet                                              *                
*              or                                                           *
*      wflashGUIx64.exe /quiet /pass:xxxxxx                                 *
*                                                                           *
*    The following example shows how to change the power-on logo.           *
*      wflashGUIx64.exe /logo:myfav.bmp                                     *
*                                                                           *
*    Notes:                                                                 *
*          - A flash update image using these program options should be     *
*            tested carefully before widespread usage.                      *
*****************************************************************************

*****************************************************************************
*                    5. Delaying Reboot in BIOS Update                      *
*                                                                           *
*  1.The BIOS update process consists of two phases. The first phase        *
*    happens in the OS. After the first flashing phase is complete, the     *
*    system will automatically reboot to perform the secondary              *
*    BIOS update phase.                                                     *
*                                                                           *
*  2.Deploy mode is designed for BIOS update deployment when the            *
*    administrator wants to manually control the system reboot after first  *
*    BIOS phase update has finished. With the parameter "/quiet" or "/sccm",*
*    the system won't reboot immediately after applying the first phase of  *
*    the BIOS update.                                                       *
*                                                                           *
*  3.The administrator must REBOOT/SHUTDOWN the system later to             *
*    trigger the secondary phase of the BIOS update.                        *
*                                                                           *
*    Important Notes:                                                       *
*                                                                           *
*    1. After the first phase of the BIOS update is complete, the           *
*       administrator must REBOOT or SHUTDOWN the system to trigger         *
*       the secondary phase.                                                *
*    2. The REBOOT mentioned in Note 1 will trigger the secondary phase     *
*       immediately. It only works if  Setup is set to boot in UEFI mode.   *
*       It does not work if Setup is set to boot the system in Legacy mode  *
*    3. The SHUTDOWN mentioned in Note 1 must be an actual                  *
*       S5 (System Power Off). The default shutdown behavior in             *
*       modern OS (WIN8/WIN8.1/WIN10 etc.) is S4 (Hibernate). To change     *
*       these modern OS shutdown behavior to S5, you need to turn off       *
*       the "Fast Startup" from "Control Panel -> Power options". If your   *
*       deployment software supports S5 shutdown, you can send a sleep S5   *
*       command by software to trigger the secondary BIOS update phase.     *
*       If the system is not shut down using the S5 shutdown, the flash     *
*       process will not continue, and will need to be restarted from       *
*       phase 1.                                                            *
*                                                                           *
*    The following example shows how to use /quiet to implement first phase *
*    BIOS update with deployment software:                                  *
*                                                                           *
*       flash.cmd /quiet                                                    *
*       or                                                                  *
*       wFlashGUIx64.exe /quiet                                             *
*                                                                           *
*    The following example shows how to use /sccm to implement first        *
*    phase BIOS update with deployment software:                            *
*                                                                           * 
*      wFlashGUIx64.exe imagem1a.rom /sccm                                  *
*                                                                           *
*    The deployment software can unpack the installation files into the     *
*    default folder without running the BIOS update program by using these  *
*    parameters on the OS flash package:                                    *
*                                                                           *
*    M1AJYnnUSA.exe /verysilent                                             *
*                                                                           *
*    The deployment software can also unpack the installation files into    *
*    the default folder and implement BIOS update automatically in quiet    *
*    mode using these parameters on the OS flash package:                   *
*                                                                           *
*    M1AJYnnUSA.exe /verysilent /quietflash                                 *
*                                                                           *
*    If the software package is already unpacked, you can run               *
*    Quietflash.cmd to silently update the system firmware.                 *
*                                                                           *
*****************************************************************************
````


---

## After
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : M1AKT56A
Manufacturer      : LENOVO
Name              : M1AKT56A
SerialNumber      : MJ06YQ52
Version           : LENOVO - 1560
````

5. installed

[<img src="https://i.imgur.com/FcNbOeR.png">](https://i.imgur.com/FcNbOeR.png)

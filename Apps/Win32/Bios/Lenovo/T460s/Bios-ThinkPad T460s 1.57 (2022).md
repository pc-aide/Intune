# Bios-ThinkPad T460s 1.57 (2022)

---

## Doc
1. [BIOS Update](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t460s/downloads/ds112117)
2. [ddl-Inno-Setup](https://download.lenovo.com/pccbbs/mobiles/n1cuj44w.exe)

---

## Acronym
1. ddl - download direct link
2. PS - PowerShell
3. util - utilitaire

---

## New Group Dynmaic
1. Name : Intune_Deploy_T460s_Lenovo
2. Membership type : Dynamic Device
3. Add dynamic query\Add expression
````md
1. Property : deviceModel
2. Operator : Equals
3. Value : 20FAS21100

* Other option :
(device.deviceModel -startsWith "20LH") -or (device.deviceModel -startsWith "20LJ")
````

---

## Bios util
1. Extract-installer-Inno-Setup

[<img src="https://i.imgur.com/3zpXRTy.png">](https://i.imgur.com/3zpXRTy.png)

2. install.ps1
````ps1
# ver: 23-11-2022
 
$Log_file = 'MAJ_Bios_lenovo.txt'

try{
     # -cac --Check AC power is on
     # -v --Enable flash verification
     # -file --Indicate BIOS image file for flash
     # -silent --Silent operation (no beeps)
    Suspend-Bitlocker c: -RebootCount 1
    start .\WinFlash64.exe -args '-cac -v -silent -file N1CET89W\$0AN1C00.FL1' `
        -RedirectStandardOutput $env:ProgramData\$Log_file -Wait -WindowStyle Hidden
}
catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

---

## intuneWin 
1. Generate .intuneWin with IntuneWinAppUtil.exe
2. SRC folder : "C:\Users\Admin\AppData\Local\Temp\New folder"
3. please specify the setup file : install.ps1
4. please specify the output folder : "C:\Users\Admin\AppData\Local\Temp\New folder"
5. Do you want to specify catalog folder (y/n)? n

[<img src="https://i.imgur.com/2D6kdUl.png">](https://i.imgur.com/2D6kdUl.png)
[<img src="https://i.imgur.com/AsMMyZX.png">](https://i.imgur.com/AsMMyZX.png)

---

## Intune\Apps
1. Apps | All apps\Add
2. App type : Windows app (Win32)
3. select file : install.intunewin
4. Name : Bios-ThinkPad T460s 1.57 (2022)
5. Desc: `# Bios-ThinkPad T460s 1.57 (2022)`
6. Publisher : Lenovo
7. App Version : 1
8. Program\Install command : `powershell -executionpolicy bypass -file Install.ps1`
9. Program\UnInstall command : `powershell -executionpolicy bypass -file Install.ps1`
10. Requirements\Operating system architecture : 64-bit
11. Minimum OS : Windows 10 2004
12. Detection rules 
````md
1. Rule format : Manually configure detection rules
2. Type : Registry 
3. Key path : HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS
4. Value name : BIOSVersion
5. Detection method : String comparison
6. Operator : Equals
7. Value : N1CET89W (1.57 )
````
13. Assignments : Intune_Deploy_T460s (Group dynamic)

---

## Test
1. run\CompanyPortal:

[<img src="https://i.imgur.com/HfOOR90.png">](https://i.imgur.com/HfOOR90.png)

2. checkUp\PS
````ps1
PS C:\Users\Admin> gwmi win32_bios

SMBIOSBIOSVersion : N1CET89W (1.57 )
Manufacturer      : LENOVO
Name              : N1CET89W (v1.57 )
SerialNumber      : PC0BSP6P
Version           : LENOVO - 0
````

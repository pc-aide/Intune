# Bios-ThinkPad T440s 2.54 (2020)

---

## URL
1. [BIOS Update-Lenovo](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t440s/downloads/ds035965)

---

## Before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : GJET79WW (2.29 )
Manufacturer      : LENOVO
Name              : GJET79WW (2.29 )
SerialNumber      : PC02LMK0
Version           : LENOVO - 2290
````

---

## Group Dynmaic
1. Name : Intune_Deploy_T440s_Lenovo
2. Membership type : Dynamic Device
3. Add dynamic query\Add expression : `(device.deviceModel -contains "20AQ006HUS")`

---

## Bios util
1. Extract-Inno-Setup

[<img src="https://i.imgur.com/NgZUOYO.png">](https://i.imgur.com/NgZUOYO.png)

2. install.ps1
````ps1
# ver: 03-01-2023
 
$Log_file = 'MAJ_Bios_lenovo.txt'

try{
     # -cac --Check AC power is on
     # -v --Enable flash verification
     # -file --Indicate BIOS image file for flash
     # -silent --Silent operation (no beeps)
    Suspend-Bitlocker c: -RebootCount 1
    start .\WinFlash64.exe -args '-cac -v -silent -file GJETA4WW\$01DF000.FL1' `
        -RedirectStandardOutput $env:ProgramData\$Log_file -Wait -WindowStyle Hidden
}
catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

---

## IntuneWin
````ps1
# -c --setup_folder | -s --setup_file | -o --outfile | -q --quiet
start IntuneWinAppUtil.exe -args "-c c:\temp -s c:\temp\install.ps1 -o c:\temp -q"
````

[<img src="https://i.imgur.com/9rUDl0s.png">](https://i.imgur.com/9rUDl0s.png)

---

## Intune\Apps
1. App type : Windows app (Win32)
2. Select file : install.intunewin
3. Name : `Bios-ThinkPad T440s 2.54 (2020)`
4. Desc.: `# Bios-ThinkPad T440s 2.54 (2020)`
5. Publisher : Lenovo
6. App Version : 2.54
7. Logo : <logo.jpg>
8. Install command : `PowerShell -ExecutionPolicy Bypass -File Install.ps1`
9. Uninstall command : `PowerShell -ExecutionPolicy Bypass -File Install.ps1`
10. Install behavior : system
11. Device restart behavior : app install may force a device restart
12. OS arch. : 64-bits
13. Min OS : 2004
14. Detection rules
````md
1. Rule format : Manually configure detection rules
2. Type : Registry
3. Key path : HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS
4. Value name : BIOSVersion
5. Detection method : String comparison
6. Operator : Equals
7. Value : GJETA4WW (2.54 )
````
15. Assignments\Required : Intune_Deploy_T440s_Lenovo

---

## Test
1. run\companyPortal: 

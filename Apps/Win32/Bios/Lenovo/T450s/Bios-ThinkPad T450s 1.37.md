# Bios-ThinkPad T450s 1.37

---

## URL
1. [dowload-page](https://support.lenovo.com/ca/en/downloads/ds102109-bios-update-utility-bootable-cd-for-windows-10-64-bit-81-7-32-bit-64-bit-thinkpad-t450-t450s)
2. [ddl-1.37-jbuj73ww.exe-InnoSetup](https://download.lenovo.com/pccbbs/mobiles/jbuj73ww.exe)

---

## before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : JBET73WW (1.37 )
Manufacturer      : LENOVO
Name              : Phoenix BIOS SC-T v2.1
SerialNumber      : PC08U64A
Version           : LENOVO - 1370
````

---

## Dynamic Group
1. Name Intune_laptop_T450s
2. Membership type : dynamic device
3. Add dynamic query\Rule Syntax : `(device.deviceModel -contains "20BWS2QX00")`

---

## Extract
1. Ext jbuj73ww
2. take `JBET73WW` -- detection rule

[<img src="https://i.imgur.com/cWJjVdr.png">](https://i.imgur.com/cWJjVdr.png)

---

## ps1
````ps1
# ver: 17-12-2022
 
$Log_file = 'MAJ_Bios_lenovo.txt'

try{
     # -cac --Check AC power is on
     # -v --Enable flash verification
     # -file --Indicate BIOS image file for flash
     # -silent --Silent operation (no beeps)
    Suspend-Bitlocker c: -RebootCount 1
    start .\WinFlash64.exe -args '-cac -v -silent -file JBET73WW\$0AJB000.FL1' `
        -RedirectStandardOutput $env:ProgramData\$Log_file -Wait -WindowStyle Hidden
}
catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

[<img src="https://i.imgur.com/P0gIUL2.png">](https://i.imgur.com/P0gIUL2.png)

---

## IntuneWinAppUtil.exe
````ps1
# 1.8.4
# -c <SRC_Folder>
# -s <SRC_setupFile>
# -o <O/P_Folder>
# -q <quiet>
start IntuneWinAppUtil.exe -args "-c $env:temp -s $env:temp\install.ps1 -o $env:temp -q"
````

[<img src="https://i.imgur.com/np8CYff.png">](https://i.imgur.com/np8CYff.png)

---

## NewApp
1. app type : Windows app (Win32)
2. select file : install.intuneWin
3. Name : Bios-ThinkPad T450s 1.37 (2019)
4. Desc : `# Bios-ThinkPad T450s 1.37 (2019)`
5. Publisher : Lenovo
6. AppVer : 1.37
7. Logo : <take_jpeg_on_google>
8. Install / uninstall command `powershell -executionpolicy bypass -file Install.ps1`
9. Install behavior : system
10. Device restart  behavior : app install may force a device restart
11. OS arch. x64
12. Min OS : 2004
13. Detection rules\Rules format\Manually configure dection rules\Add\Rule type\Registry
14. Key path : `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`
15. Value name : `BIOSVersion`
16. Detection method : String comparison
17. Operatior : Equals
18. Value : `JBET73WW (1.37 )`
19. Assignments\Required\GropMode : Intune_laptop_T450s
20. Time New app : ~5m

---

## Test
1. run\companyPortal:\sync

[<img src="https://i.imgur.com/RZxU10l.png">](https://i.imgur.com/RZxU10l.png)

2. Intune\<device>\ManagedApps\app installation completed (already installed)

[<img src="https://i.imgur.com/5Ow0FY7.png">](https://i.imgur.com/5Ow0FY7.png)

3. Installed

[<img src="https://i.imgur.com/9EkhXD2.png">](https://i.imgur.com/9EkhXD2.png)

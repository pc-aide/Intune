# Bios-ThinkPad T470s 1.48

---

## URL
1. [Download-page](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t470s/downloads/ds120418)
2. [ddl-InnoSetup-ver-5.11-n1wuj41w.exe](https://download.lenovo.com/pccbbs/mobiles/n1wuj41w.exe)

---

## before
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : N1WET54W (1.33 )
Manufacturer      : LENOVO
Name              : N1WET54W (1.33 )
SerialNumber      : PC0SVZ9N
Version           : LENOVO - 1330
````

---

## NewGroup
1. Name : Intune_laptop_T470s
2. Type : Dynamic device
3. Query : `(device.deviceModel -contains "20HGS02M00")`

---

## Extract
1. Ext n1wuj41w

[<img src="https://i.imgur.com/gUwuvvJ.png">](https://i.imgur.com/gUwuvvJ.png)

---

## ps1
1. new file `install.ps1` in your extract folder
````ps1
# ver: 03-12-2022
 
$Log_file = 'MAJ_Bios_lenovo.txt'

try{
     # -cac --Check AC power is on
     # -v --Enable flash verification
     # -file --Indicate BIOS image file for flash
     # -silent --Silent operation (no beeps)
    Suspend-Bitlocker c: -RebootCount 1
    start .\WinFlash64.exe -args '-cac -v -silent -file N1WET69W\$0AN1W00.FL1' `
        -RedirectStandardOutput $env:ProgramData\$Log_file -Wait -WindowStyle Hidden
}
catch{
    $_ | out-file $env:ProgramData\$Log_file
}
````

[<img src="https://i.imgur.com/J9mLGzl.png">](https://i.imgur.com/J9mLGzl.png)

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

[<img src="https://i.imgur.com/yshawYi.png">](https://i.imgur.com/yshawYi.png)

---

## Intune
1. Apps\Add\AppType : Windows app (Win32)
2. select file : install.intunewin
3. Name : Bios-ThinkPad T470s 1.48
4. Desc: `# Bios-ThinkPad T470s 1.48`
5. Publisher : Lenovo
6. Logo : <takeIt_googleImag>
7. Install command : `powershell -executionpolicy bypass -file Install.ps1`
8. Uninstall command : `powershell -executionpolicy bypass -file Install.ps1`
9. Install behavior : System
10. OS arch. : 64-bit
11. Min. OS : 2004
12. Rule format\Manually configure detection rules\Add\Rule type : Registry
13. Key path : `HKEY_LOCAL_MACHINE\HARDWARE\DESCRIPTION\System\BIOS`
14. Value name : `BIOSVersion`
15. Detection method : String comparison
16. Operator : Equals
17. Value : `N1WET69W (1.48 )`
18. Assignments\Add group: Intune_laptop_T470s
19. Create

---

## Test
1. run\companyPortal:
2. force sync on the client or Intune (Time : ~5m)
3. SCT_flash_uti_for_lenovo.jpg (Time: ~5m)

[<img src="https://i.imgur.com/xTE6SHa.jpg">](https://i.imgur.com/xTE6SHa.jpg)

4. Installed

[<img src="https://i.imgur.com/S1fKStk.png">](https://i.imgur.com/S1fKStk.png)

5. bios
````ps1
gwmi win32_bios

SMBIOSBIOSVersion : N1WET69W (1.48 )
Manufacturer      : LENOVO
Name              : N1WET69W (1.48 )
SerialNumber      : PC0SVZ9N
Version           : LENOVO - 1480
````

---

## Report
1. Intune\Device\<DeviceName>\Managed Apps\

[<img src="https://i.imgur.com/sz7ehoW.png"](https://i.imgur.com/sz7ehoW.png)

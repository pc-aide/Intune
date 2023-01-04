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

## WinFlash32.exe -help
````md
WinFlash32.exe -help
SCT Flash Utility for Lenovo
 for Windows V1.0.3.0
Copyright (c) 2011-2015 Phoenix Technologies Ltd.
Copyright (c) 2011-2015 Lenovo Group Limited.

Usage: Flash [COMMAND]

bak      [filename]       Backup BIOS ROM before flash.
bbl                       Flash boot block.
bcp      [EVSA binary]    Overwrite BCP data.
bcplogo  [BCP name] [file name] [Image ID] Replace logo image stored in BCP.
cac                       Check AC power is on.
cbp      threshold        Check battery power in percentage.
cvar                      Clear variables.
dat      string           Specify the asset tag DMI string.
dmc      string           Specify the chassis manufacturer DMI string.
dmm      string           Specify the motherboard manufacturer DMI string.
dks      string           Specify the SKU number DMI string.
dms      string           Specify the system manufacturer DMI string.
dos      [string;string2;...]|[index1 string1 ...] Specify the OEM DMI strings.
dpc      string           Specify the chassis asset tag number DMI string.
dpm      string           Specify the motherboard product ID DMI string.
dps      string           Specify the system product ID DMI string.
dsc      string           Specify the chassis serial number DMI string.
dsm      string           Specify the motherboard serial number DMI string.
dss      string           Specify the system serial number DMI string.
dus      [uuid] [overwrite] Specify the UUID DMI string.
dvc      string           Specify the chassis version DMI string.
dvm      string           Specify the motherboard version DMI string.
dvs      string           Specify the system version DMI string.
endkey                    Required key press after flashing.
ese                       Enable security examiner.
exit                      Exit program after flash completed.
file     filename         Indicate BIOS image file for flash.
help                      Show command list.
ipf      [region name]|all Flash specific region
logo     filename [ImageId] [filename] [ImageId] ... Replace logo.
ls       [ImageId] ...    Reserve logo in BIOS ROM.
mod      filename         Replace a FFS module.
nodelay                   No delay after flash.
nodrom                    No decomposing ROM when crisis recovery.
noerror                   Do not display error messages.
nowarn                    Do not display warning messages.
oc       string           Specify the OEM command line.
p                         Production mode. Disable simple text output.
prog     start size       Flash specific area. Both parameters in hexadecimal.
patch                     Patch mode. To patch particular data to current BIOS.
raw      GUID filename [Index] Replace raw section of FFS module.
rsbr     GUID1 GUID2 ...  Reserve sub-regions with specified GUIDs.
sd                        Skip BIOS build date time checking.
silent                    Silent operation (no beeps).
slp      filename         Replace SLP marker or MSDM key.
spu      filename 20|21   Replace SLP public key.
ss                        Skip all SLP sub-regions.
sn                        Skip part number checking.
shutdown                  Shutdown after flash completed instead of reboot.
v                         Enable flash verification.
vbl                       Enable Microsoft Bit-locker check.
vcpu     [filename]       Update variable size CPU microcode.
wb                        Flash without skipping same content blocks.
write    filename start [fdla] Write a binary file to specific physical address or FDLA.
wsbr     GUID filename    Write a binary file to specific sub-region.
````

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

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

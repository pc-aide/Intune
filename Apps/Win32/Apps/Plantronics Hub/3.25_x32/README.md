# Plantronics Hub 3.25_x32

---

## URL
1. [dowload-page](https://www.poly.com/ca/en/support/downloads-apps/hub-desktop)
2. [ddl-PlantronicsHubInstaller.exe](https://www.poly.com/content/dam/www/software/PlantronicsHubInstaller.exe)

---

## IntuneWinAppUtil
````ps1
# 1.8.4
# -c <SRC_folder> | -s <SRC_setup_file>
# -o <O/P_folder> | -q <quiet>
start IntuneWinAppUtil.exe -args "-c $env:temp -s $env:temp\PlantronicsHubInstaller.exe -o $env:temp -q"
````

[<img src="https://i.imgur.com/0fg2VvV.png">](https://i.imgur.com/0fg2VvV.png)

---

## Add App
1. sselect file : PlantronicsHubInstaller.intunewin
2. Name : Plantronics Hub 3.25_x32
3. Desc.
````md
# Plantronics Hub 3.25_x32
A client application that allows users to control the settings on their Plantronics audio device
````
4. Publisher : Poly
5. App Ver : `3.25.53800.37131`
6. logo : <img src="https://raw.githubusercontent.com/pc-aide/Intune/main/Apps/Win32/Apps/Plantronics%20Hub/3.25_x32/logo.png" width="50"/>

---

## Program
1. Install command : `PlantronicsHubInstaller.exe /install /quiet /norestart HIDEDESKTOPSHORTCUT=1`

* PendingReboot : yes --For install & uninstall

2. Uninstall command : `"C:\ProgramData\Package Cache\{28b4b465-8fc2-4598-8f73-7abad4728a70}\PlantronicsHubBootstrapper.exe" /uninstall /silent`

3. Instasll behavior : System

4. Devie restart behavior : Intune will force a mandatory device restart

---

## Requirements
1. OS arch. x64
2. Min OS : 2004

---

## Dection Rules
1. Rules format : Manually configure detection rules
2. Rule type : Registry
3. Key path : `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Plantronics`
4. Value name : `PLTHub_MHUUninstaller`
5. Detection method : Version comparison
6. Operator : Greater than or equal to 
7. Value : `3.25.53800.37131`
8. Associated with a 32-bit app on 64-bit clients : No

---

## Supersedence (preview)
1. Selected apps : Plantronic Hub 3.24_x32
2. Uninstall previous version : Yes

---

## Assignements
1. Required : all users

---

## NewApp
1. Time : ~1m  uploading Plantronics Hub 3.25_x32 | ~124Mb
2. will appear on the client (Time ~5m)

---

## Test
### Case 1 | old + new
0. uninstall old version + reboot
1. install new ver
2. run\companyPortal: 

3. Updated version :

[<img src="https://i.imgur.com/h72fTjf.png">](https://i.imgur.com/h72fTjf.png)

4. ΣReboot : 2 (old + new)

[<img src="https://i.imgur.com/T7m6dK2.png">](https://i.imgur.com/T7m6dK2.png)

---

### Case 2 | new overwrite old ?
1. 

---

### Case 3 | old ver not exist
1. 

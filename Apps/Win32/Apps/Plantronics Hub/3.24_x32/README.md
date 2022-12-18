# Plantronics Hub 3.24_x32

---

## URL
1. [download-page](https://www.poly.com/ca/en/support/downloads-apps/hub-desktop)
2. [silent-install.net-no_shortcut_desktop](https://silent-install.net/software/plantronics/hub_software/3.11.52216.23527)

---

## App information
1. Name : Plantronics Hub 3.24_x32
2. Desc. :
````md
# Plantronics Hub 3.24_x32
A client application that allows users to control the settings on their Plantronics audio device
````
3. Publisher : Poly
4. Ver : 3.24.53524.36336
5. logo : <img src="https://d3bql97l1ytoxn.cloudfront.net/app_resources/179154/overview/img6875211839127717565-2x.png" alt="isolated" width="100"/>

---

## Program
1. Install\command `PlantronicsHubInstaller.exe /install /quiet /norestart HIDEDESKTOPSHORTCUT=1 /log %ProgramData%\InstallPlantronicsHub3.24_x32.log`

* PendingReboot : yes -- for install & uninstall

* Warning : duplicate log files :

[<img src="https://i.imgur.com/PgtEZvc.png">](https://i.imgur.com/PgtEZvc.png)


2. Unisntall\command `"C:\ProgramData\Package Cache\{28b4b465-8fc2-4598-8f73-7abad4728a70}\PlantronicsHubBootstrapper.exe" /uninstall /silent /log %ProgramData%\UninstallPlantronicsHub3.24_x32.log`

3. Install behavior : sytem

4. Device restart behavior : Intune will force a mandatory device restart

---

## Requirements
1. OS Arch. x64
2. Min. OS : 2004

---

## Detection Rules
1. Rules format : Manually configure detection rules
2. Detection rules `HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Plantronics`
3. Value name : `PLTHub_MHUUninstaller`
4. Detection method : Version comparison
5. Operator : Greter than or equal to
6. Value : `3.24.53524.36336`

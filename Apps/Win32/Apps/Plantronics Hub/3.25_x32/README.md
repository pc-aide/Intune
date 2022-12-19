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
6. logo : <img src="https://d3bql97l1ytoxn.cloudfront.net/app_resources/179154/overview/img6875211839127717565-2x.png" width="50"/>

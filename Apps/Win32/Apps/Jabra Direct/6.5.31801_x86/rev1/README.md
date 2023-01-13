# Jabra Direct-6.5.31801_x86_rev1

---

## URL
1. [Download-page](https://www.jabra.ca/software-and-services/jabra-direct)
2. [ddl-JabraDirectSetup.exe](https://jabraxpressonlineprdstor.blob.core.windows.net/jdo/JabraDirectSetup.exe)

---

## Test

---

### Updates
1. no checkUp update

[<img src="https://i.imgur.com/7h03I4x.png">](https://i.imgur.com/7h03I4x.png)

---

### Settings
1. General
  * Automatic device updte: [default] off
  
[<img src="https://i.imgur.com/VyWXp8X.png">](https://i.imgur.com/VyWXp8X.png)

2. Notification
 * Fimrware updates & device information
 
````ps1
# default value : True
cat "$env:appData\Jabra Direct\config.json" |
 ConvertFrom-Json |
 select DirectShowNotification -ExpandProperty DirectShowNotification | 
 select Key,Value

key                    value
---                    -----
DirectShowNotification  True
````

[<img src="https://i.imgur.com/ehMmEef.png">](https://i.imgur.com/ehMmEef.png)

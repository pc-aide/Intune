# Get-WindowsAutoPilotInfo 3.9

---

## URL
1. [Get-WindowsAutoPilotInfo From PowerShell Gallery](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo/3.9)

---

## AutoPilot.bat
1. Create a folder named 'AutoPilot' on your Windows Setup drive
2. AutoPilot.bat :
````bat
@echo off
for /d %%i in (d:\*) do (
    C:\Windows\system32\WindowsPowerShell\v1.0\powershell.exe -ExecutionPolicy Bypass -Command "& {
Get-PSDrive -PSProvider FileSystem |
 ForEach-Object {
 if (Test-Path ($_.Root + 'AutoPilot') -PathType Container) {
 Invoke-Expression ('%%i\AutoPilot\autoPilot.ps1')
}
 }
 }"
)
````
<img src="https://i.imgur.com/oMz7kaZ.png">

3. AutoPilot.ps1
````ps1
$computerSystem = Get-WmiObject Win32_ComputerSystem
$manufacturer = $computerSystem.Manufacturer
$model = $computerSystem.Model

if ($manufacturer -eq "Lenovo") {
    $computerProduct = Get-WmiObject Win32_ComputerSystemProduct
    $lenovoModel = $computerProduct.Version

    if ($lenovoModel -like 'nuc*') {
        Write-Host "Cet ordinateur est un ordinateur de bureau Lenovo."
    } else {
        Write-Host "Cet ordinateur Lenovo a un modèle indéterminé."
    }
} elseif ($model -like '*laptop*' -or $model -like '*notebook*') {
    Write-Host "Cet ordinateur est un ordinateur portable."
} elseif ($model -like '*surface pro*') {
    Write-Host "Cet ordinateur est une Surface Pro (laptop)."
} elseif ($model -like 'surface pro') {
    Write-Host "Cet ordinateur est une Surface Pro (laptop)."
} else {
    Write-Host "Type d'ordinateur indéterminé."
}
````

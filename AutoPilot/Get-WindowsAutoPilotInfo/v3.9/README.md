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
$model = $computerSystem.Model

# Variable pour stocker le préfixe
$prefix = ""

if ($model -like '*laptop*' -or $model -like '*notebook*') {
    $prefix = "LW"  # Préfixe pour les ordinateurs portables (LW pour Laptop Windows)
} elseif ($model -like '*surface pro*') {
    $prefix = "LW"  # Préfixe pour les Surface Pro (LW pour Laptop Windows)
} elseif ($model -like 'nuc*') {
    $prefix = "DW"  # Préfixe pour les ordinateurs de bureau (DW pour Desktop Windows)
} else {
    $prefix = "XX"  # Préfixe par défaut pour un type d'ordinateur indéterminé (peut être modifié)
}

# Vous pouvez utiliser la variable $prefix pour d'autres opérations ou affichages.
Write-Host "Le préfixe de cet ordinateur est $prefix"
````

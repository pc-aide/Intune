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
# Vérifier si PowerShell est exécuté avec des privilèges d'administration
$isAdmin = [bool]([System.Security.Principal.WindowsIdentity]::GetCurrent().Groups -match "S-1-5-32-544")

# Si PowerShell n'est pas exécuté en tant qu'administrateur, relancer le script avec des privilèges d'administration
if (-not $isAdmin) {
    $scriptPath = $MyInvocation.MyCommand.Definition
    Start-Process "powershell" -ArgumentList "-NoProfile -ExecutionPolicy Bypass -File '$scriptPath'" -Verb RunAs
} else {
    # Continuer avec le script restant
    $computerSystem = Get-WmiObject Win32_ComputerSystem
    $model = $computerSystem.Model
    $bios = Get-WmiObject Win32_BIOS
    $serialNumber = $bios.SerialNumber

    # Variable pour stocker le préfixe
    $prefix = ""

    if ($model -like '*laptop*' -or $model -like '*notebook*' -or $model -like '*surface pro*') {
        $prefix = "LW"  # Préfixe pour les ordinateurs portables (LW pour Laptop Windows)
    } elseif ($model -like 'nuc*') {
        $prefix = "DW"  # Préfixe pour les ordinateurs de bureau (DW pour Desktop Windows)
    } else {
        $prefix = "XX"  # Préfixe par défaut pour un type d'ordinateur indéterminé (peut être modifié)
    }

    # Concaténer le préfixe avec le numéro de série pour former le nouveau nom d'ordinateur
    $newComputerName = $prefix + $serialNumber

    # Renommer l'ordinateur avec le nouveau nom
    Rename-Computer -NewName $newComputerName -Force -Restart
}
````

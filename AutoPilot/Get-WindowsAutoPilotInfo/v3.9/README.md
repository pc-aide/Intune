# Get-WindowsAutoPilotInfo 3.9

---

## URL
1. [Get-WindowsAutoPilotInfo From PowerShell Gallery](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo/3.9)

---

## AutoPilot
0. Files :

<img src="https://i.imgur.com/l2rOn0y.png">
   
2. Create a folder named 'AutoPilot' on your Windows Setup drive
3. AutoPilot.bat :
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

3. [Get-WindowsAutoPilotInfo.ps1](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo/3.9/Content/Get-WindowsAutopilotInfo.ps1)


5. AutoPilot.ps1
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
    Rename-Computer -NewName $newComputerName #-Force -Restart
}

# Auto enrollement
# Obtenir la liste des lecteurs disponibles
$drives = Get-WmiObject Win32_LogicalDisk | Where-Object { $_.DriveType -eq 3 }

# Nom du dossier à rechercher
$folderToFind = "AutoPilot"

# Boucler à travers les lecteurs
foreach ($drive in $drives) {
    $folderPath = Join-Path -Path $drive.DeviceID -ChildPath $folderToFind

    # Vérifier si le dossier AutoPilot existe sur ce lecteur
    if (Test-Path -Path $folderPath -PathType Container) {
        # Si le dossier est trouvé, exécuter le script Get-WindowsAutoPilotInfo.ps1
        $scriptPath = Join-Path -Path $folderPath -ChildPath "Get-WindowsAutoPilotInfo.ps1"
        if (Test-Path -Path $scriptPath -PathType Leaf) {
            Write-Host "Le dossier AutoPilot a été trouvé sur le lecteur $($drive.DeviceID). Exécution du script Get-WindowsAutoPilotInfo.ps1."
            Invoke-Expression -Command "PowerShell -ExecutionPolicy Bypass -File $scriptPath -Online"
            break  # Sortir de la boucle une fois que le script est exécuté
        }
    }
}

# Si le dossier AutoPilot n'est pas trouvé sur aucun lecteur
if (-not $scriptPath) {
    Write-Host "Le dossier AutoPilot n'a pas été trouvé sur les lecteurs disponibles."
}
````

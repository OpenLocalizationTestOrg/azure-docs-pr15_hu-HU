<properties
   pageTitle="Alkalmazások telepítésének virtuális gép kiterjesztésű automatizálása |} Microsoft Azure"
   description="Azure virtuális gép DotNet Core oktatóprogram"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Alkalmazás környezet, amelyben az erőforrás-kezelő Azure-sablonok

Miután minden Azure infrastrukturális követelmények azonosította, és üzembe sablon fordítani, a tényleges alkalmazás példányban kell foglalkozni. Az alábbi alkalmazások telepítési telepítése az alkalmazás tényleges bináris alakzatot Azure erőforrások hivatkozik. A zene-tár minta, .net telepítve és beállítva a virtuális gépeken szükséges alapvető és az IIS. A zene-tár bináris alakzatot a virtuális gép telepítésére van szükség, és a zene tároló adatbázis előre létrehozott.

A dokumentum adatai, hogyan virtuális gép bővítmények automatizálhatja az alkalmazások telepítésének és Azure virtuális gépeken futó beállításait. Függőségek és a egyedi konfigurációk kiemelt. Az optimális használat érdekében léptetheti egy példánya az Azure előfizetést és a munka az erőforrás-kezelő Azure-sablon együtt a megoldást. A teljes sablon megtalálható itt – [Zenét áruházból telepítési Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-Windows).

## <a name="configuration-script"></a>Konfigurációs parancsfájl

Virtuális gép bővítmények program speciális, amely a virtuális gépeken futó konfigurációs automatizálási megadására ellen végrehajtása. Bővítmények számos adott célra, például Anti-Virus, naplózásának konfigurálása és Docker konfigurációs érhetők el. Az egyéni parancsfájl bővítmény bármely parancsfájl futtassanak virtuális gép használható. A zene áruházból mintával van a az egyéni parancsfájl bővítmény beállítása a Windows virtuális gépeken futó, és a zene áruház alkalmazás telepítéséhez.

Részletező hogyan deklarált virtuális gép bővítmények egy erőforrás-kezelő Azure-sablont, mielőtt vizsgálja meg a futtatott parancsfájl. Ez a parancsfájl beállítja a Windows virtuális gépen szeretné üzemeltetni a zene áruház alkalmazás. Futtatásához a parancsfájl telepíti az összes szükséges szoftverek telepíti a zenei áruház alkalmazás adatforrás-vezérlő, és az adatbázis előkészítése. 

> Ez a példa a bemutató célra van.

```none
Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# install iis
Install-WindowsFeature web-server -IncludeManagementTools

# install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# download / config music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceserver>", $sqlserver } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replaceuser>", $user } | Set-Content C:\music\config.json
(Get-Content C:\music\config.json) | ForEach-Object { $_ -replace "<replacepass>", $password } | Set-Content C:\music\config.json

# workaround for db creation bug
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>Virtuális parancsprogram-bővítmény

Virtuális bővítmények futtatható virtuális gép ellen build időben együtt a bővítmény erőforrás az erőforrás-kezelő Azure-sablon. A bővítmény felvehetők a Visual Studio hozzáadása erőforrás varázslóval, vagy a sablonba érvényes JSON beszúrásával. A parancsprogram-bővítmény erőforrás van beágyazva a virtuális gép erőforrás; Ez a következő példában is láthatja.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Virtuális parancsfájl kiterjesztése](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339)a JSON minta megjelenítéséhez. 

Figyelje meg a alatti JSON, hogy a parancsprogram GitHub tárolja. Ez a parancsfájl is megtalálható Azure Blob-tárolóhoz. Azure erőforrás-kezelő sablonok lehetővé teszi, a parancsprogram végrehajtás karakterláncot kell megtervezni, hogy sablon paraméterek értékek paraméterként használható parancsfájl végrehajtása. Ebben az esetben adatai a sablonok telepítésekor, és ezeket az értékeket majd használhatók, amikor az parancsfájl.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Az egyéni parancsfájl bővítmény használatával kapcsolatos további tudnivalókért olvassa el az [Egyéni parancsfájl bővítmények, erőforrás-kezelő sablonok](./virtual-machines-windows-extensions-customscript.md)című témakört.

## <a name="next-step"></a>Következő lépés

<hr>

[További Azure erőforrás-kezelő sablonok feltárása](https://github.com/Azure/azure-quickstart-templates)

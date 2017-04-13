<properties
   pageTitle="Alkalmazások telepítésének virtuális gép kiterjesztésű automatizálása |} Microsoft Azure"
   description="Azure virtuális gép DotNet Core oktatóprogram"
   services="virtual-machines-linux"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines-linux"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/21/2016"
   ms.author="nepeters"/>

# <a name="application-deployment-with-azure-resource-manager-templates"></a>Alkalmazás környezet, amelyben az erőforrás-kezelő Azure-sablonok

Miután minden Azure infrastrukturális követelmények azonosította, és üzembe sablon fordítani, a tényleges alkalmazás példányban kell foglalkozni. Az alábbi alkalmazások telepítési telepítése az alkalmazás tényleges bináris alakzatot Azure erőforrások hivatkozik. A zene-tár minta, .net telepítve és beállítva a virtuális gépeken szükséges alapvető NGINX és felügyelő. A zene-tár bináris alakzatot a virtuális gép telepítésére van szükség, és a zene tároló adatbázis előre létrehozott.

A dokumentum adatai, hogyan virtuális gép bővítmények automatizálhatja az alkalmazások telepítésének és Azure virtuális gépeken futó beállításait. Függőségek és a egyedi konfigurációk kiemelt. Az optimális használat érdekében léptetheti egy példánya az Azure előfizetést és a munka az erőforrás-kezelő Azure-sablon együtt a megoldást. A teljes sablon megtalálható itt – [Zenét áruházból telepítési a Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="configuration-script"></a>Konfigurációs parancsfájl

Virtuális gép bővítmények program speciális, amely a virtuális gépeken futó konfigurációs automatizálási megadására ellen végrehajtása. Bővítmények számos adott célra, például Anti-Virus, naplózásának konfigurálása és Docker konfigurációs érhetők el. Egy egyéni parancsfájl bővítmény bármely parancsfájl futtassanak virtuális gép használható. A zene-tár mintával van a az egyéni parancsfájl kiterjesztése a Ubuntu virtuális gépeken futó konfigurálásához és a zene-tár alkalmazás telepítéséhez.

Részletező hogyan deklarált virtuális gép bővítmények egy erőforrás-kezelő Azure-sablont, mielőtt vizsgálja meg a futtatott parancsfájl. Ez a parancsfájl konfigurálása a Ubuntu virtuális gépen szeretné üzemeltetni a zene áruház alkalmazás. Futtatásához a parancsfájl telepíti az összes szükséges szoftverek telepíti a zenei áruház alkalmazás adatforrás-vezérlő, és az adatbázis előkészítése. 

Ha többet szeretne tudni a .net szolgáltatója Linux alkalmazás alapszintű [Linux munkakörnyezetben közzététel](https://docs.asp.net/en/latest/publishing/linuxproduction.html)találhat. 

> Ez a példa a bemutató célra van.

```none
#!/bin/bash

# install dotnet core
sudo sh -c 'echo "deb [arch=amd64] https://apt-mo.trafficmanager.net/repos/dotnet-release/ trusty main" > /etc/apt/sources.list.d/dotnetdev.list'
sudo apt-key adv --keyserver apt-mo.trafficmanager.net --recv-keys 417A0893
sudo apt-get update
sudo apt-get install -y dotnet-dev-1.0.0-preview2-003121

# download application
sudo wget https://raw.github.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/music-store-azure-demo-pub.tar /
sudo mkdir /opt/music
sudo tar -xf music-store-azure-demo-pub.tar -C /opt/music

# install nginx, update config file
sudo apt-get install -y nginx
sudo service nginx start
sudo touch /etc/nginx/sites-available/default
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/nginx-config/default -O /etc/nginx/sites-available/default
sudo cp /opt/music/nginx-config/default /etc/nginx/sites-available/
sudo nginx -s reload

# update and secure music config file
sed -i "s/<replaceserver>/$1/g" /opt/music/config.json
sed -i "s/<replaceuser>/$2/g" /opt/music/config.json
sed -i "s/<replacepass>/$3/g" /opt/music/config.json
sudo chown $2 /opt/music/config.json
sudo chmod 0400 /opt/music/config.json

# config supervisor
sudo apt-get install -y supervisor
sudo touch /etc/supervisor/conf.d/music.conf
sudo wget https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/music-app/supervisor/music.conf -O /etc/supervisor/conf.d/music.conf
sudo service supervisor stop
sudo service supervisor start

# pre-create music store database
/usr/bin/dotnet /opt/music/MusicStore.dll &
```

## <a name="vm-script-extension"></a>Virtuális parancsprogram-bővítmény

Virtuális bővítmények futtatható virtuális gép ellen build időben együtt a bővítmény erőforrás az erőforrás-kezelő Azure-sablon. A bővítmény felvehetők a Visual Studio hozzáadása erőforrás varázslóval, vagy a sablonba érvényes JSON beszúrásával. A parancsprogram-bővítmény erőforrás van beágyazva a virtuális gép erőforrás; Ez a következő példában is láthatja.

Kattintson erre a hivatkozásra, az erőforrás-kezelő sablon – [Virtuális parancsfájl kiterjesztése](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L359)a JSON minta megjelenítéséhez. 

Figyelje meg a alatti JSON, hogy a parancsprogram GitHub tárolja. Ez a parancsfájl is megtalálható Azure Blob-tárolóhoz. Azure erőforrás-kezelő sablonok lehetővé teszi, a parancsprogram végrehajtás karakterláncot megtervezni, hogy sablon paraméterek értékek paraméterként használható parancsfájl végrehajtása. Ebben az esetben adatai a sablonok telepítésekor, és ezeket az értékeket majd használhatók, amikor az parancsfájl.

```none
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-linux/scripts/config-music.sh"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('sudo sh config-music.sh ',variables('musicStoreSqlName'), ' ', parameters('adminUsername'), ' ', parameters('sqlAdminPassword'))]"
    }
  }
}
```

Az egyéni parancsfájl bővítmény használatával kapcsolatos további tudnivalókért olvassa el az [Egyéni parancsfájl bővítmények, erőforrás-kezelő sablonok](./virtual-machines-linux-extensions-customscript.md)című témakört.

## <a name="next-step"></a>Következő lépés

<hr>

[További Azure erőforrás-kezelő sablonok feltárása](https://github.com/Azure/azure-quickstart-templates)

<properties
    pageTitle="A CustomScript bővítmény használata egy Linux virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként alkalmazások a Linux virtuális gépeken futó a klasszikus telepítési modell a készült Azure-ban a CustomScript bővítmény használatával."
    editor="tysonn"
    manager="timlt"
    documentationCenter=""
    services="virtual-machines-linux"
    authors="gbowerman"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="multiple"
    ms.tgt_pltfrm="linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/13/2016"
    ms.author="guybo"/>

#<a name="deploy-a-lamp-app-using-the-azure-customscript-extension-for-linux"></a>Az Azure CustomScript bővítmény használatának Linux LÁMPA alkalmazások terjesztése#

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


A Microsoft Azure CustomScript kiterjesztése Linux biztosítja az szabhatja testre a virtuális gépeken futó (VMs) bármilyen parancsfájlok a virtuális (például Python és Bash) által támogatott nyelven írt tetszőleges kódot futtatásával. Ez nagyon rugalmas megoldást automatizálhatja a alkalmazás telepítési több gépek.

Az Azure klasszikus portálon, a Windows PowerShell vagy az Azure parancssori kezelőfelületről Azure CustomScript kiterjesztése telepítheti.

Ez a cikk használjuk egy Ubuntu virtuális egy egyszerű LÁMPA alkalmazás terjesztése az Azure CLI létrehozott klasszikus telepítési modellt használja.

## <a name="prerequisites"></a>Előfeltételek

Ebben a példában hozzon létre két Azure VMs 14.04 vagy újabb verziója fut a Ubuntu. A VMs *parancsprogram-virtuális* és *lámpa-virtuális*nevezik. Használjon olyan egyedi nevet a VMs létrehozásakor. Egy szolgál a CLI parancsokat, és egy szolgál a LÁMPA alkalmazások terjesztése.

Azure tárterület-fiók és egy kulcsot elérheti őket (érheti ezt az Azure klasszikus portálról) is szükséges.

Ha segítségre van szüksége Linux VMs létrehozott Azure [létrehozása egy virtuális gépen futó Linux](virtual-machines-linux-classic-createportal.md)vonatkoznak.

A telepítés parancs Ubuntu tegyük fel, de módosíthatja bármely támogatott Linux distro a telepítést.

A parancsprogram-virtuális virtuális telepítve, a munkaidő-kapcsolaton keresztül Azure Azure CLI kell szerepelnie. A Súgó tekintse át a [Telepítse és állítsa be az Azure parancssor](../xplat-cli-install.md).

## <a name="upload-a-script"></a>Töltse fel a parancsfájl

A CustomScript bővítmény használjuk futtathat egy távoli virtuális telepítheti a LÁMPA Papírhalom, és PHP lap létrehozása a. Annak érdekében, hogy a parancsprogram bárhonnan elérheti azt tölti fel, mint az Azure blob.

### <a name="script-overview"></a>Parancsfájl áttekintése

A parancsprogram példa (beleértve az egy csendes-telepítését, MySQL) Ubuntu LÁMPA Papírhalom telepíti, ír egy egyszerű PHP-fájlt, és Apache elindul.

    #!/bin/bash
    # set up a silent install of MySQL
    dbpass="mySQLPassw0rd"

    export DEBIAN_FRONTEND=noninteractive
    echo mysql-server-5.6 mysql-server/root_password password $dbpass | debconf-set-selections
    echo mysql-server-5.6 mysql-server/root_password_again password $dbpass | debconf-set-selections

    # install the LAMP stack
    apt-get -y install apache2 mysql-server php5 php5-mysql  

    # write some PHP
    echo \<center\>\<h1\>My Demo App\</h1\>\<br/\>\</center\> > /var/www/html/phpinfo.php
    echo \<\?php phpinfo\(\)\; \?\> >> /var/www/html/phpinfo.php

    # restart Apache
    apachectl restart

### <a name="upload-script"></a>Töltse fel a parancsprogram

A parancsprogram szövegfájl, például *install_lamp.sh*, mentse, és töltse fel a Azure-tárterületet. Ezt megteheti egyszerűen az Azure CLI. Az alábbi példa a fájlt tároló tároló "parancsfájlok" nevű feltölti. Ha a tároló nem létező kell először hozza létre.

    azure storage blob upload -a <yourStorageAccountName> -k <yourStorageKey> --container scripts ./install_lamp.sh

Megtudhatja, hogy miként töltheti le a parancsfájl Azure-tárhelyről JSON fájl is létrehozhat. Mentse a *public_config.json* ("mystorage" helyettesítve a tárterület-fiók neve):

    {"fileUris":["https://mystorage.blob.core.windows.net/scripts/install_lamp.sh"], "commandToExecute":"sh install_lamp.sh" }


## <a name="deploy-the-extension"></a>A bővítmény telepítése

Most már a következő parancs segítségével a Linux CustomScript bővítmény telepítése a távoli virtuális az Azure CLI használatával.

    azure vm extension set -c "./public_config.json" lamp-vm CustomScript Microsoft.Azure.Extensions 2.0

Az előző parancs letölti és a *install_lamp.sh* parancsfájl alábbiakon futtatható a virtuális *lámpa-virtuális*neve.

Mivel az alkalmazás webkiszolgálóra tartalmazza, ne feledje, hogy nyissa meg a távoli virtuális egy HTTP listening portjához a következő parancsot.

    azure vm endpoint create -n Apache -o tcp lamp-vm 80 80

## <a name="monitoring-and-troubleshooting"></a>Figyelés és hibaelhárítás

Ellenőrizheti, hogy mennyire az egyéni parancsfájl fut, a naplófájl meg a távoli virtuális megjeleníti a. *Lámpa-virtuális* és a következő parancsot a naplófájl képviselőknek SSH.

    cd /var/log/azure/customscript
    tail -f handler.log

A CustomScript bővítmény futtatása után tallózhat, a létrehozott információt PHP lapra. A példa a jelen cikkben a PHP lap *http://lamp-vm.cloudapp.net/phpinfo.php*.

## <a name="additional-resources"></a>További források

Az azonos alapvető lépések összetettebb alkalmazások terjesztése is használhatja. Ebben a példában az a telepítési parancsfájlt Azure-tárolóban lévő nyilvános blob-ként mentették. Egy nagyobb biztonságot nyújt lehetőséget akkor is megmarad a telepítési parancsfájlt [Biztonságos hozzáférés aláírás](https://msdn.microsoft.com/library/azure/ee395415.aspx) (Társítások) biztonságos blob-ként.

Ezután az Azure CLI, Linux és a CustomScript bővítmény további erőforrások sorolja fel.

[CustomScript bővítményében Linux virtuális testreszabási feladatok automatizálása](https://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)

[Azure Linux bővítmények (GitHub)](https://github.com/Azure/azure-linux-extensions)

[Linux és Azure a számítások Megnyitás forrás](virtual-machines-linux-opensource-links.md)

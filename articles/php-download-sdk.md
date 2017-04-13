<properties
    pageTitle="Töltse le az Azure SDK PHP"
    description="Megtudhatja, hogy miként töltheti le és telepítse az Azure SDK PHP."
    documentationCenter="php"
    services="app-service\web"
    authors="allclark"
    manager="douge"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="06/01/2016"
    ms.author="allclark;yaqiyang"/>

#<a name="download-the-azure-sdk-for-php"></a>Töltse le az Azure SDK PHP

## <a name="overview"></a>– Áttekintés

Az Azure SDK PHP-összetevők, amelyek lehetővé teszik, telepítése, valamint az Azure PHP-alkalmazások kezelése tartalmazza. Az Azure SDK PHP kifejezetten, az alábbiakat tartalmazza:

* **A PHP ügyfél képtárak Azure**. Ezek osztálytár egy felületet biztosít az Azure funkciókat, például adatok szolgáltatások elérése és cloud services.  
* **Azure parancssort for Mac, Linux, és a Windows (Azure CLI)**. Ez a telepítésére és Azure-szolgáltatásokkal, például az Azure-webhelyekre és Azure virtuális gépeken futó kezelésére vonatkozó parancsok. Bármely platformon, többek között például Mac Linux és a Windows Azure CLI munkát.
* **Azure PowerShell (csak Windows)**. Ez az üzembe helyezése és Azure szolgáltatások, például a Felhőszolgáltatások és a virtuális gépeken futó kezelése a PowerShell-parancsmagok csoportja.
* **Az Azure emulátor (csak Windows)**. A számítási és tárolási emulátor a felhőszolgáltatások és adatok kezelése szolgáltatások, amelyek lehetővé teszik az alkalmazások helyileg tesztelése helyi emulátor. Az Azure emulátor csak futtassa a Windows.

Az alábbi szakaszok ismertetik, hogyan töltheti le és telepítheti a fentebb ismertetett összetevőket.

Ez a témakör útmutatását tegyük fel, hogy [PHP] [ install-php] telepítve.

> [AZURE.NOTE] A PHP ügyfél tárak használata Azure 5,5 vagy újabb PHP kell rendelkeznie.

##<a name="php-client-libraries-for-azure"></a>Azure PHP ügyfél képtárak

Az Azure PHP ügyfél képtárak egy felületet biztosít az Azure funkciókat, például adatok szolgáltatások elérése és cloud services, az olyan operációs rendszerekhez. Ezeket a zeneszerző keresztül telepíthető.

Olvashat a PHP ügyfél tárak használata Azure megtudhatja, [hogy miként használhatja a Blob-szolgáltatás][blob-service], [a táblázat szolgáltatás használata] [ table-service] , és [hogy hogyan használhatja a várólista szolgáltatást][queue-service].

###<a name="install-via-composer"></a>Via zeneszerző telepítése

1. [Mely számjegy telepítése][install-git].


    > [AZURE.NOTE] A Windows rendszeren is szüksége lesz a mely végrehajtható számjegy hozzáadása a PATH környezeti változó.

2. Hozzon létre egy **composer.json** a legfelső szintű a projekt nevű fájlt, és a következő kód hozzáadása:

        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }

3. Töltse le a ** [composer.phar] [ composer-phar] ** a projekt legfelső szintű.

4. Nyisson meg egy parancssort, és hajtsa végre ezt a projekt legfelső szintű

        php composer.phar install

##<a name="azure-powershell-and-azure-emulators"></a>Azure PowerShell és Azure emulátor

Azure PowerShell egy PowerShell-parancsmagok az üzembe helyezése és Azure szolgáltatások (például a Felhőszolgáltatások és a virtuális gépeken futó) kezelése. Az Azure emulátor a felhőszolgáltatások és adatok kezelése szolgáltatások, amelyek lehetővé teszik az alkalmazások helyileg tesztelése emulátor. Ezek az összetevők által támogatott csak Windows.

A javasolt telepítse az Azure PowerShell és az Azure emulátor módja használja a [Microsoft webes Platform telepítő][download-wpi]. Fontos tudni, hogy akkor is választhatja, például PHP, SQL Server, a Microsoft SQL Server PHP és WebMatrix Drivers más fejlesztési összetevők telepítése.

Azure PowerShell használatával kapcsolatos további tudnivalókért megtudhatja, [hogy miként Azure PowerShell használata][powershell-tools].

##<a name="azure-cli"></a>Azure CLI

Az Azure CLI egy telepítésére és Azure-szolgáltatásokkal, például az Azure-webhelyekre és Azure virtuális gépeken futó kezelésére szolgáló parancsok. Azure CLI telepítésével kapcsolatos további tudnivalókért lásd: [telepítse az Azure CLI](xplat-cli-install.md).

## <a name="next-steps"></a>Következő lépések

További tudnivalókért lásd: a [PHP Developer Center](/develop/php/).


[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git

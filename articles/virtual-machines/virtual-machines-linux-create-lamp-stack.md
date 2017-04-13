<properties
    pageTitle="LÁMPA üzembe Linux virtuális gépre |} Microsoft Azure"
    description="Megtudhatja, hogy miként egy Linux virtuális a LÁMPA Papírhalom telepítése"
    services="virtual-machines-linux"
    documentationCenter="virtual-machines"
    authors="jluk"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="NA"
    ms.topic="article"
    ms.date="06/07/2016"
    ms.author="juluk"/>

# <a name="deploy-lamp-stack-on-azure"></a>LÁMPA Papírhalom a Azure terjesztése
Ez a cikk azt ismerteti, hogy egy Apache webkiszolgáló MySQL és PHP (LÁMPA Papírhalom) a Azure telepítéséről keresztül. Azure fiók (az[ingyenes próbaverziót első](https://azure.microsoft.com/pricing/free-trial/)) és az [Azure CLI](../xplat-cli-install.md) , amely [a Azure fiókjához kapcsolt](../xplat-cli-connect.md)lesz szüksége.

Az ebben a cikkben szereplő LÁMPA két módja van:

## <a name="quick-command-summary"></a>A parancsok rövid összefoglalása

1) Kattintson az új virtuális LÁMPA terjesztése

```
# One command to create a resource group holding a VM with LAMP already on it
$ azure group create -n uniqueResourceGroup -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
```

2) A meglévő virtuális LÁMPA terjesztése

```
# Two commands: one updates packages, the other installs Apache, MySQL, and PHP
user@ubuntu$ sudo apt-get update
user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql
```

## <a name="deploy-lamp-on-new-vm-walkthrough"></a>Új virtuális forgatókönyv a LÁMPA terjesztése

Új [erőforráscsoport](../azure-resource-manager/resource-group-overview.md) , amely tartalmazni fogja a virtuális létrehozásával indítható el:

    $ azure group create uniqueResourceGroup westus
    info:    Executing command group create
    info:    Getting resource group uniqueResourceGroup
    info:    Creating resource group uniqueResourceGroup
    info:    Created resource group uniqueResourceGroup
    data:    Id:                  /subscriptions/########-####-####-####-############/resourceGroups/uniqueResourceGroup
    data:    Name:                uniqueResourceGroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: null
    data:
    info:    group create command OK

A virtuális magát létrehozásához egy már írt Azure erőforrás-kezelő sablon található [a GitHub itt](https://github.com/Azure/azure-quickstart-templates/tree/master/lamp-app)is használhatja.

    $ azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json uniqueResourceGroup uniqueLampName

Néhány további bemenetben Rákérdezés választ kell megjelennie:

    info:    Executing command group deployment create
    info:    Supply values for the following parameters
    storageAccountNamePrefix: lampprefix
    location: westus
    adminUsername: someUsername
    adminPassword: somePassword
    mySqlPassword: somePassword
    dnsLabelPrefix: azlamptest
    info:    Initializing template configurations and parameters
    info:    Creating a deployment
    info:    Created template deployment "uniqueLampName"
    info:    Waiting for deployment to complete
    data:    DeploymentName     : uniqueLampName
    data:    ResourceGroupName  : uniqueResourceGroup
    data:    ProvisioningState  : Succeeded
    data:    Timestamp          :
    data:    Mode               : Incremental
    data:    CorrelationId      : d51bbf3c-88f1-4cf3-a8b3-942c6925f381
    data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/lamp-app/azuredeploy.json
    data:    ContentVersion     : 1.0.0.0
    data:    DeploymentParameters :
    data:    Name                      Type          Value
    data:    ------------------------  ------------  -----------
    data:    storageAccountNamePrefix  String        lampprefix
    data:    location                  String        westus
    data:    adminUsername             String        someUsername
    data:    adminPassword             SecureString  undefined
    data:    mySqlPassword             SecureString  undefined
    data:    dnsLabelPrefix            String        azlamptest
    data:    ubuntuOSVersion           String        14.04.2-LTS
    info:    group deployment create command OK

Most már létrehozott egy Linux virtuális a LÁMPA már telepítve van a azt. Ha kívánja, le a [ellenőrzése LÁMPA sikeresen telepítve] Ugrás ellenőrzéséhez a telepítést.

## <a name="deploy-lamp-on-existing-vm-walkthrough"></a>A meglévő virtuális forgatókönyv LÁMPA terjesztése

Ha segítségre van szüksége egy Linux virtuális létrehozása központi [Itt megtudhatja, hogy miként hozhat létre egy Linux virtuális] (. / virtual-machines-linux-quick-create-cli.md). Következő lépésként meg kell SSH be a Linux virtuális. Ha segítségre van szüksége egy SSH kulcs létrehozásával központi [Itt megtudhatja, hogy miként hozhat létre egy SSH kulcsot a Mac vagy Linux] (. / virtual-machines-linux-mac-create-ssh-keys.md).
Ha már van egy SSH kulcsot, előre és SSH ismertetőt találhat a Linux virtuális a `ssh username@uniqueDNS`.

Most, hogy a Linux virtuális belül dolgozik, akkor fogja végigvezetjük az LÁMPA objektumhalomban telepít Debian-alapú terjesztését. A pontos parancsok az egyéb Linux distros eltérhetnek.

#### <a name="installing-on-debianubuntu"></a>Debian/Ubuntu telepítése

Az alábbi csomagok telepítve van szüksége lesz: `apache2`, `mysql-server`, `php5`, és `php5-mysql`. Ezek közvetlenül ezekről a csomagokról grabbing, akár Tasksel telepítheti. Mindkét kérdésre utasítások felsorolása olvasható.
Telepítése előtt meg kell töltse le és csomag listák frissítése.

    user@ubuntu$ sudo apt-get update
    
##### <a name="individual-packages"></a>Egyes csomagok
Apt get használata:

    user@ubuntu$ sudo apt-get install apache2 mysql-server php5 php5-mysql

##### <a name="using-tasksel"></a>Tasksel használata
Másik lehetőségként Tasksel, több kapcsolódó csomagok telepített feladatként összehangolt"" alakzatot a rendszer Debian/Ubuntu eszköz letöltheti.

    user@ubuntu$ sudo apt-get install tasksel
    user@ubuntu$ sudo tasksel install lamp-server

A fenti lehetőségek valamelyikét futtatása után kéri ezekről a csomagokról és számos más függőségek telepítéséhez. Nyomja le az "i" és "Meg az ENTER billentyűt" továbbra is, és minden más megjelenő utasításokat követve MySQL-rendszergazdai jelszó megadása. Ez a MySQL PHP használatához szükséges minimális szükséges PHP bővítmények telepíti. 

![][1]

A következő parancsot lásd: más elérhető csomagként PHP-bővítmények:

    user@ubuntu$ apt-cache search php5


#### <a name="create-infophp-document"></a>Info.php dokumentum létrehozása

Most már tudja ellenőrizni, hogy melyik verziót Apache MySQL és PHP van a parancssorból keresztül beírásával `apache2 -v`, `mysql -v`, vagy `php -v`.

További vizsgálandó szeretné, ha egy rövid PHP adatai lapon a böngészőben is létrehozhat. Hozzon létre egy új fájlt Nano szövegszerkesztőben ezzel a paranccsal:

    user@ubuntu$ sudo nano /var/www/html/info.php

A GNU Nano text szerkesztője adja hozzá a következő sort:

    <?php
    phpinfo();
    ?>

Mentse, majd lépjen ki a szövegszerkesztőben.

Indítsa újra Apache ezzel a paranccsal, hogy az összes új példányát lép érvénybe, hogy.

    user@ubuntu$ sudo service apache2 restart

## <a name="verify-lamp-successfully-installed"></a>LÁMPA telepítése sikerült ellenőrzése

Most ellenőrizheti, hogy az imént létrehozott a böngészőben nyissa meg a http://youruniqueDNS/info.php PHP adatai lapon, ez hasonlóan kell kinéznie.

![][2]

Apache telepítésének ismételt megjelenítéséhez, http://youruniqueDNS/ a Apache2 Ubuntu alapértelmezett lap megtekintésével érdemes. Megjelenik az alábbihoz hasonló.

![][3]

Gratulálunk, van csak a telepítő LÁMPA Papírhalom a az Azure virtuális!

## <a name="next-steps"></a>Következő lépések

Nézze meg a LÁMPA Papírhalom Ubuntu dokumentációja:

- [https://help.ubuntu.com/Community/ApacheMySQLPHP](https://help.ubuntu.com/community/ApacheMySQLPHP)

[1]: ./media/virtual-machines-linux-deploy-lamp-stack/configmysqlpassword-small.png
[2]: ./media/virtual-machines-linux-deploy-lamp-stack/phpsuccesspage.png
[3]: ./media/virtual-machines-linux-deploy-lamp-stack/apachesuccesspage.png

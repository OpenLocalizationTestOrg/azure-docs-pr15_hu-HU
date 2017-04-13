<properties
    pageTitle="MySQL-OpenSUSE virtuális telepítése |} Microsoft Azure"
    description="Olvassa el a MySQL-OpenSUSE Linux VMirtual gépen Azure-ban."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/19/2016"
    ms.author="cynthn"/>

# <a name="install-mysql-on-a-virtual-machine-running-opensuse-linux-in-azure"></a>Telepítse a MySQL egy virtuális gépen futó OpenSUSE Linux Azure-ban

[MySQL] [ MySQL] népszerű, a Megnyitás-forrás SQL-adatbázis. Ebből az oktatóanyagból megtudhatja, hogy hogyan OpenSUSE Linux operációs rendszert futtató virtuális gép létrehozása, majd telepítse az MySQL.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


<br>


## <a name="create-a-virtual-machine-running-opensuse-linux"></a>OpenSUSE Linux operációs rendszert futtató virtuális gép létrehozása

[AZURE.INCLUDE [create-and-configure-opensuse-vm-in-portal](../../includes/create-and-configure-opensuse-vm-in-portal.md)]

## <a name="install-and-run-mysql-on-the-virtual-machine"></a>Telepítése és futtatása a MySQL a virtuális gépen

[AZURE.INCLUDE [install-and-run-mysql-on-opensuse-vm](../../includes/install-and-run-mysql-on-opensuse-vm.md)]

## <a name="next-steps"></a>Következő lépések
MySQL kapcsolatos részletekért lásd: a [MySQL-dokumentáció][MySQLDocs].

[MySQLDocs]: http://dev.mysql.com/doc/index-topic.html
[MySQL]: http://www.mysql.com


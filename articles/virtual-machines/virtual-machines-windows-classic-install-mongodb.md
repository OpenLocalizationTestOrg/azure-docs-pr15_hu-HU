<properties
    pageTitle="MongoDB telepítése a Windows virtuális |} Microsoft Azure"
    description="Megtudhatja, hogy miként az a Windows Server operációs rendszert futtató klasszikus telepítési modell készült Azure virtuális MongoDB telepítése."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="iainfou"/>

#<a name="install-mongodb-on-a-windows-vm"></a>A Windows virtuális MongoDB telepítése

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Szeretné telepíteni, és állítsa be az erőforrás-kezelő telepítési modell MongoDB, [ismertető](virtual-machines-windows-classic-install-mongodb.md)témakört is.

[MongoDB] [ MongoDB] népszerű Megnyitás-forrást, a nagy teljesítményű NoSQL adatbázis. Ez a cikk végigvezeti Önt a Windows Server virtuális gép (virtuális) az [Azure klasszikus portál]létrehozása[AzurePortal]. Létrehozása és adatok lemezen csatolása a virtuális való telepítéséről és konfigurálásáról MongoDB előtt. Ha egy meglévő virtuális, amely a használni kívánt Azure-ban van, akkor nekiláthat [való telepítéséről és konfigurálásáról MongoDB](#install-and-run-mongodb-on-the-virtual-machine).


## <a name="create-a-virtual-machine-running-windows-server"></a>A Windows Server operációs rendszert futtató virtuális gép létrehozása

Kövesse ezeket az utasításokat követve hozzon létre egy virtuális számítógépre.

[AZURE.INCLUDE [virtual-machines-create-WindowsVM](../../includes/virtual-machines-create-windowsvm.md)]

> [AZURE.NOTE] Zárólap MongoDB adja meg a virtuális gép létrehozásakor, és állítsa be az alábbi képlettel történik: **Mongo**, nevezze el a protokoll használata **TCP** és beállítása a nyilvános és titkos portokat **27017**.

## <a name="attach-a-data-disk"></a>Lemezen adatok csatolása
A virtuális gép a tárhely szükséges, adatok lemezen csatolni, és majd inicializálni azt, hogy a Windows vele. Ha már van adat lemezen, meglévő lemezről csatolhat, vagy üres lemez csatolhat.

[AZURE.INCLUDE [howto-attach-disk-windows-linux](../../includes/howto-attach-disk-windows-linux.md)]

A lemez inicializálása, tanulmányozza "hogyan: a Windows Server új adatok lemezen inicializálni" [hogyan csatolhat Windows virtuális géphez adatok lemezen](virtual-machines-windows-classic-attach-disk.md)a.

## <a name="install-and-run-mongodb-on-the-virtual-machine"></a>Telepítése és futtatása MongoDB a virtuális gépen

[AZURE.INCLUDE [install-and-run-mongo-on-win2k8-vm](../../includes/install-and-run-mongo-on-win2k8-vm.md)]

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban megtanulta, hogyan hozhat létre a Windows Server operációs rendszert futtató virtuális géphez, távolról való és adatok lemezen csatolása.  Emellett megtanulta miként telepítheti és MongoDB beállítása a Windows-alapú virtuális gépen. Most már hozzáférhet MongoDB a Windows-alapú virtuális gépen követve a Speciális témakörök [MongoDB dokumentáció][MongoDocs].

[MongoDocs]: http://docs.mongodb.org/manual/
[MongoDB]: http://www.mongodb.org/
[AzurePortal]: http://manage.windowsazure.com
